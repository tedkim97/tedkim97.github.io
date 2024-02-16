---
layout: post
title: Failed Branchless Optimization - are we actually optimizing anything?
subtitle: Profiling a toy study with DNS headers
published: true
enable_latex: false
permalink: branch_fail
frontpage: true
technical: true
funstuff: true
tags:
  - programming
  - software
  - rust
  - architecture
  - assembly
concepts:
  - optimizations
  - profiling  
  - assembly
  - rust
---

# TLDR

This code change makes your code faster - but the reason is really simple/dumb. 

```rust
let mut flags: u16 = 0;
// original
if value.field1 {
    flags |= dns_header_masks::QR;
}
// faster
flags |= dns_header_masks::QR * (value.field1 as u16);
```


# Context

If you're not interested in the context and just want to get to the programming part, you can skip this section.

In DNS land, all communication is done as "messages" in a specific wire format originally defined in [RFC1035#4.1](https://datatracker.ietf.org/doc/html/rfc1035.html#section-4.1). I'm not going to dive into the RFC, but the relevant part is the "header" section. Every DNS Message includes a "header" that specify the remaining sections of the message for parsing, in addition to including query information like response codes, query parameters, etc.

The header format has change a little, but the current best practice ([RFC6895#2](https://datatracker.ietf.org/doc/html/rfc6895.html#section-2)) defines the wire-encoded format like this: 

```rfc
                               1  1  1  1  1  1
 0  1  2  3  4  5  6  7  8  9  0  1  2  3  4  5
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
|                      ID                       |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
|QR|   Opcode  |AA|TC|RD|RA| Z|AD|CD|   RCODE   |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
|                    QDCOUNT                    |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
|                    ANCOUNT                    |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
|                    NSCOUNT                    |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
|                    ARCOUNT                    |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
```

Even though the wire format exists, most of the DNS libraries I've seen include a logical representation of a "message". I.E a struct that would look something like this.

```rust
struct DnsHeader {
    id: u16,
    qr: bool,
    opcode: u8,
    aa: bool,
    tc: bool,
    rd: bool,
    ra: bool,
    ad: bool,
    cd: bool,
    rcode: u8,
    qdcount: u16,
    ancount: u16,
    nscount: u16,
    arcount: u16,
}
```

# What's the point?

History aside, what's the point?

Code that interacts with logical representations (i.e struct) need to convert to the wire format (i.e bytes) when sending the data over the socket. In the RFC we have logical bools that represent a message. The `QR` (query), `AA` (authoritative answer), `TC` (trucation), `AD` (authentic data), `RD` (recursion desired), and `CD` (checking desired) are all booleans/1 bit. 

This means when we convert the logical code to the wire format, we often employ an `if` statement that leads to simple branches like this:

```rust
let mut header_flags = 0u16;
if header.authoritative {
	header_flags |= 0b0000_0100_0000_0000;
}
if header.recursion_desired {
	header_flags |= 0b0000_0001_0000_0000;
}
```


Don't take my word for it, we can see some real life examples on some open-source repos.

This [`dns-parser` library will apply a Bitwise OR to the value that will be written to the wire](https://github.com/tailhook/dns-parser/blob/191266705b30feabe9fbf9289b4cb4da0ea2be31/src/header.rs#L72-L90) - it looks something like this: 

```rust
pub fn convert(&self, ...) {
	// other conversions/checks
	let mut flags = 0u16;
	// ...
	if self.authoritative {
		flags |= 0b0000_0100_0000_0000;
	}
	// continue
}
```

[`Hickory-dns` (formerly trust-dns) header code will also apply a bitwise OR depending on the logical value](https://github.com/hickory-dns/hickory-dns/blob/main/crates/proto/src/op/header.rs#L521-L535). It would boil down to something like this 

```rust
let mut flags = 0u16;
// ...
// I would describe this as more of a ternary-like operator..?
flags |= if self.authoritative {
	0b0000_0100_0000_0000;
} else {
	0b0000_0000_0000_0000;
}
```

This is a branch - and where there is a branch, there's room for a branchless optimization (sometimes...).

## Branchless Optimizations & Pre-Optimizations

The reason why I emphasize branches is because branches can be *potentially slower* when it comes to branch mispredictions, and employing branchless patterns *can potentially* speed up codes. The reason why I emphasize *potentially* is because it's highly dependent dependent on compiler, CPU architecture, how well branches are predicted, etc. 

> TL:DR what is a branch prediction? 
> Modern CPUs are complicated, and there are a lot of optimizations that make your code go fast. One of those optimizations is **[branch predictions](https://en.wikipedia.org/wiki/Branch_predictor)**[^1].
> Essentially the processor will try to guess the outcome of a branch, and perform the subsequent computation. Predicting the outcome of a branch is a way of improving throughput on modern CPUs. If the branch is guessed correctly, great! If it was a wrong guess, your CPU will need to do backtrack throw away the results, backtrack and compute the correct branch. This is expensive, and the more frequently your CPU guesses wrong, the bigger a penalty your code pays (In practice CPUs are quite good at this). 
> I wouldn't consider myself an expert on branch-predictions, so I recommend these resources: 
> https://danluu.com/branch-prediction/ 
> https://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-processing-an-unsorted-array
> https://en.algorithmica.org/hpc/pipelining/ (this chapter in general)
> https://blog.cloudflare.com/branch-predictor


[^1]: Branch predictions are also related to a set of [CPU vulnerabilities](https://en.wikipedia.org/wiki/Spectre_(security_vulnerability))

For instance, here is one of the simplest branched/branchless comparison:
 
```cpp
// branched
return (a > b) ? a : b;
// branchless
return (a > b) * a + (a <= b) * b;
```

The tradeoff with the branchless version of code is that you're computing more (in this case we're performing additional arithmetic operations), while the branched version isn't registering the additional compute. Moreover, if the CPU correctly guesses the branch 100% of the time, the function will "free" compared to branchless code. However, depending on the language, compiler, CPU architecture, the branchless optimized assembly may have more or less instructions the non-optimized version. 

Another tradeoff you'll get is that employing branchless logic really runs against "semantic" coding principles. If I saw a random optimzied branchless function, I wouldn't be able to tell if an idiot or a genius wrote it. That's a non-trivial argument that the branchless version hurts readability, thereby hurting maintainability. 

Finally, you should never assume your "branchless optimization" actually reduces branches. Smart compilers will be able to utilize the [`cmov` instruction and reduce their vulnerability to branch prediction failures](https://stackoverflow.com/questions/14131096/why-is-a-conditional-move-not-vulnerable-to-branch-prediction-failure) and compile `if/else` statements to use conditional moves to avoid branches. IN FACT, I expected this to be the case for rust. The rust compiler (and/or LLVM) should knows that we should just use a conditional move (`cmov`). Spoiler alert, the rust compiler is pretty good about that here, but I think you should still read on. 

Finally, there's also the meta that branchless programming usually isn't where the biggest wins are going to be for performance. Odds are, there are other places to hunt for performance gains such as database/query optimizations, software architecture, or messy memory usage,

# Benchmarking & Investigating

## Microbenchmark
I wrote three implementations of the function that look like this:

```rust
pub fn branchless(header: &DnsHeader, bytes: &mut [u8]) {
    // ...
    let mut flags: u16 = 0;
    flags |= dns_header_masks::QR * (header.qr as u16);
    flags |= u16::from(header.opcode) << 11;
    flags |= dns_header_masks::AA * (header.aa as u16);
    flags |= dns_header_masks::TC * (header.tc as u16);
    // ...
}

pub fn branched_1(header: &DnsHeader, bytes: &mut [u8]) {
    // ...
    let mut flags: u16 = 0;
    if header.qr {
        flags |= dns_header_masks::QR;
    }
    flags |= u16::from(header.opcode) << 11;
    if header.aa {
        flags |= dns_header_masks::AA;
    }
    if header.tc {
        flags |= dns_header_masks::TC;
    }
    // ...
}

pub fn branched_2(header: &DnsHeader, bytes: &mut [u8]) {
    // ...
    let mut flags: u16 = 0;
    flags |= u16::from(header.opcode) << 11;

    flags |= if header.qr {
        dns_header_masks::QR
    } else {
        0u16
    };
    flags |= if header.aa {
        dns_header_masks::AA
    } else {
        0u16
    };
    flags |= if header.tc {
        dns_header_masks::TC
    } else {
        0u16
    };
    // ... 
}
```

I modeled `branched_1` to be like tailhook/dns-parser snippets, and `branched_2` to be like the hickory-dns lib. I did the basics of avoiding the compiler optimizing away the important bits here. [Repo is here for reproduction](https://github.com/tedkim97/dnsheader-conversion-microbenchmark).

When I actually ran the benchmarks, I was genuinely surprised (and annoyed) that the branchless version was slightly faster. Out of 10K calls, we saved a whole 18 microseconds/iteration compared to the slowest implementation...! Increasing the number of trials maintained this speed difference - I'm going to skip the stat test for my sanity. 

```bash
cargo bench
...
test header_conversion::tests::bench_wire_format_query_header_branched_1        ... bench:      55,772 ns/iter (+/- 735)
test header_conversion::tests::bench_wire_format_query_header_branched_2        ... bench:      61,655 ns/iter (+/- 487)
test header_conversion::tests::bench_wire_format_query_header_branchless        ... bench:      43,204 ns/iter (+/- 962)
...
```

## Profiling w/ `perf`
However, was this speedup actually because we've magically reduced our branch mis-prediction rate? Or is there something else at play?

To see if we've actually improved performance, we need to profile the code. I used `perf stat` with a non-benchmarked version of the code (it semi-randomly generated structs, marking the output as non-optimizable, etc). Note that the creation of these structs take up a decent chunk of the perf runtime here, so the absolute times are noise. Here are the results I got:

### Branchless
```bash
 Performance counter stats for './target/release/random_bench branchless 5000000' (1000 runs):

            241.31 msec cpu-clock                 #    0.999 CPUs utilized            ( +-  0.22% )
            # ... 
       393,247,063      branches                  # 1629.638 M/sec                    ( +-  0.03% )  (72.93%)
            24,491      faults                    #    0.101 M/sec                    ( +-  0.00% )
       393,160,350      branches                  # 1629.278 M/sec                    ( +-  0.02% )  (72.30%)
           708,438      branch-misses             #    0.18% of all branches          ( +-  4.95% )  (70.70%)

          0.241632 +- 0.000522 seconds time elapsed  ( +-  0.22% )

```

### Branched 1
```bash
 Performance counter stats for './target/release/random_bench branched1 5000000' (1000 runs):

            248.24 msec cpu-clock                 #    0.999 CPUs utilized            ( +-  0.14% )
            # ... 
       394,276,819      branches                  # 1588.313 M/sec                    ( +-  0.03% )  (72.36%)
            24,491      faults                    #    0.099 M/sec                    ( +-  0.00% )
       396,680,480      branches                  # 1597.995 M/sec                    ( +-  0.02% )  (72.43%)
           665,171      branch-misses             #    0.17% of all branches          ( +-  0.04% )  (71.35%)

          0.248560 +- 0.000359 seconds time elapsed  ( +-  0.14% )
```

### Branched 2
```bash
 Performance counter stats for './target/release/random_bench branched2 5000000' (1000 runs):

            249.35 msec cpu-clock                 #    0.999 CPUs utilized            ( +-  0.15% )
            # ... 
       393,823,034      branches                  # 1579.386 M/sec                    ( +-  0.03% )  (72.24%)
            24,491      faults                    #    0.098 M/sec                    ( +-  0.00% )
       396,491,903      branches                  # 1590.089 M/sec                    ( +-  0.02% )  (72.39%)
           668,578      branch-misses             #    0.17% of all branches          ( +-  0.29% )  (71.39%)

          0.249680 +- 0.000386 seconds time elapsed  ( +-  0.15% )
```

So the branchless version still has the same branch miss rate as the other implementations AND the same order of magnitude of branches (I believe the branches are coming from the rand crate I pulled in). So the speed up can't be from the "branchless optimization" we *thought* we did. 

Maybe there's some caching at play? No our perf stats don't show any better cache locality usage:

```bash
Performance counter stats for './target/release/random_bench branchless 5000000' (1000 runs):

           241.31 msec cpu-clock                 #    0.999 CPUs utilized            ( +-  0.22% )
       10,132,540      cache-references          #   41.990 M/sec                    ( +-  0.78% )  (69.99%)
          423,594      cache-misses              #    4.181 % of all cache refs      ( +-  0.49% )  (70.45%)
      970,739,141      cycles                    #    4.023 GHz                      ( +-  0.20% )  (71.34%)
    2,044,234,986      instructions              #    2.11  insn per cycle           ( +-  0.04% )  (72.30%)
# ...

Performance counter stats for './target/release/random_bench branched1 5000000' (1000 runs):

           248.24 msec cpu-clock                 #    0.999 CPUs utilized            ( +-  0.14% )
       10,255,956      cache-references          #   41.315 M/sec                    ( +-  0.56% )  (70.73%)
          481,062      cache-misses              #    4.691 % of all cache refs      ( +-  0.24% )  (70.79%)
    1,000,298,005      cycles                    #    4.030 GHz                      ( +-  0.14% )  (70.91%)
    2,071,827,255      instructions              #    2.07  insn per cycle           ( +-  0.03% )  (71.44%)
# ...

Performance counter stats for './target/release/random_bench branched2 5000000' (1000 runs):

           249.35 msec cpu-clock                 #    0.999 CPUs utilized            ( +-  0.15% )
       10,254,911      cache-references          #   41.126 M/sec                    ( +-  0.43% )  (70.80%)
          481,199      cache-misses              #    4.692 % of all cache refs      ( +-  0.24% )  (70.86%)
    1,003,748,791      cycles                    #    4.025 GHz                      ( +-  0.15% )  (70.95%)
    2,117,250,613      instructions              #    2.11  insn per cycle           ( +-  0.03% )  (71.37%)
```

What is the difference here? The difference we should pay attention to here is the number of instructions run. `branched_2` has the most instructions and is the slowest, while `branchedless` has the least number of instructions (and is the fastest). Assuming every instruction has the same cost, if the CPU is running at a fixed rate, less instructions to do the same computation is faster. 

We can confirm this by analyzing the assembly.

## Analyzing the assembly

What's the underlying implementation of the three snippets here? For that we need to actually look at the assembly the code is generating. Using the crate `cargo-show-asm` (NOT `cargo asm` which is considered unmaintained and wasted a bunch of my time) we can get the generated assembly using `cargo-asm`.

```
cargo asm header_util::header_conversion::convert_to_wire_format_branched_1
cargo asm header_util::header_conversion::convert_to_wire_format_branched_2
cargo asm header_util::header_conversion::convert_to_wire_format_branchless
```

I'll just save the assembly here because it takes too much space: https://github.com/tedkim97/dnsheader-conversion-microbenchmark/tree/139548ccc1ca3fa81ff2c4bfb6e66753ff2732a3/generated_assembly. If we count the number of instructions, each version of the function has:
- Branched1 has 134 instructions
- Branched2 has 143 instructions
- Branchless has 126 instructions 

The reason why the branchless version is faster than the other is because the machine code **just has less instructions** - meaning we have faster code. This also lines up neatly with the relative speed ordering of all these three functions. 

# Conclusion + Other thoughts
Three thoughts: 

1. If you make 10 million QPS, you could save up to 0.018 seconds every second..! In a year you're saving 157 hours worth of time...  Jokes aside, I want to emphasize how pointless of an exercise this is. Serializing the header is only 12 bytes out potential 100's of bytes in a DNS message. There's a lot of other stuff that needs to occur, parsing, and the speed increase is probably not worth the amount of time necessary to actually confim/profile this behavior. 


2. I was honestly very surprised this "optimization" (aggressive air quotes)... "worked". I was expecting these three versions to all have the same speed (to the nanosecond). Mainly because I was expecting the branched versions of the code to use `cmov` instructions (which they do), but also I expected the final representations of two branched versions to be identical.
- I was actually more interested in analyzing how much randomness in those header bits affected the speed, and how manipulating the prediction rate affected conversion. I was sort of hoping to do an analysis [like the one here](https://en.algorithmica.org/hpc/pipelining/branching/) and make a cool visualization. I think the problem is a bit more interesting here because the branches are being predicted consequentially, depending on how the CPU behaves, we could observe the *entire* instruction having to be restart because of a pipeline flush. 

3. I do think language matters here. There are certain optimizations that C++ compiler can make that a Rust compiler wouldn't (I heard this is due to rust's aggressive safety checks). I'm curious how the C++ version with clang would behave, but that's only if I can finally decide on a build system...

## Appendix
What OS/kernal
- `uname -r` (`5.4.0-150-generic`) && `lsb_release -a` (`UBUNTU 18.04.05 LTS`)


Rust Version
- 

What about the debug versions (unoptimized) of the code?
- The debug version of the code DOES have branches, and running `perf` on them did show the branched versions having ~0.15-0.20% more branch misses than the branchless version. However, I didn't think it was worth doing an analysis here because people are going to complain that I analyzed a debug build.

Does the result change if use a struct method (`fn convert_to_wire_format_method_branchless(&self, bytes: &mut [u8])`)rather than a function like `convert_to_wire_format_branchless(header: &DnsHeader, bytes: &mut [u8])`
- I tested this and No


Does the assembly change with unsafe rust?
- I wrapped the inside body of each function with a giant `unsafe {}` (and changed the function signature to `unsafe`) and the assembly didn't change. That result makes sense given what they say about the unsafe in the rust book: https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html

What about the unoptimized versions of the code?
- The debug versions of these functions are performed *roughly* the same amount (and a lot slower than the optimized version). You can use `cargo bench` with the debug binaries by including this within cargo.toml

```
[profile.bench]
opt-level = 0
```

Did you target your CPU architecture?
- I ran these benchmarks and perf tests by including this in my CLI call (which I read on the internet somewhere that this was enough):
```
RUSTFLAGS='-C target-cpu=native' cargo bench
```