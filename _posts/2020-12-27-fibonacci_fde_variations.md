---
layout: post
title: Iterative & Tail Recursive Fast Fibonacci Sequence
subtitle: Say that twice as fast
published: true 
enable_latex: true
enable_d3: true
permalink: tail_recursive_fibonacci
frontpage: false
technical: true
funstuff: true
tags:
  - c
  - cs
  - python
  - programming
  - math
  - optimization
  - recursion
  - fibonacci
concepts:
  - fibonacci
  - recursion
  - TCO
---

# Introduction
As a programming exercise, I like to implement the iterative and tail-recursive versions of recursive functions (even if the language doesn't support [Tail Call Optimizations](https://en.wikipedia.org/wiki/Tail_call). This isn't really a rewarding habit, but I just find it fun. [After critiquing a medium post about "the new fastest Fibonacci sequence"]({% link _posts/2020-12-13-fib_critique.md %}), I decided to try to write the fast doubling algorithm iteratively and tail-recursively. This was one of the more complicated recursions I've converted, and I'll try to explain why. [If you just want to see the code, here's the repo.](https://github.com/tedkim97/fibonacci-fd-comparisons)

As a quick spoiler/note - my implementations are not *true* translations of the naive recursion. While their time and space complexities are **asymptotically** the same, the runtime complexities of my implementation are $ O(log(n)) + O(log(n)) $ rather than $ O(log(n)) $. Although the python iterative/tail-recursive implementations were faster then the recursive method. The trade-off is that, my versions take a bit more memory. 

### What is Tail Call Recursion? What is Tail Call Optimization (TCO)?
In an over-simplified description, [Tail Call Recursion](https://en.wikipedia.org/wiki/Tail_call) is a special recursive case where the last subroutine of a function is a call to itself. A very popular example is recursions for factorial. While a recursive version of the factorial function looks like this: 

```c
int fac(int n){
  if(n == 0) {
    return 1;
  }
  return n * fac(n - 1);
}
```

The tail recursive version can play with its parameters to just return a call to itself like this:
```c
int fac_tr(int n, int total) {
  if(n == 0) {
    return total;
  }
  return fac_tr(n - 1, total * n);
}
```

The reason why we might want this tail call recursion is because our compiler/interpreter might provide Tail Call Optimization. Tail Call Optimization (TCO) will write our machine code to avoid allocating space on the stack for our calling function, and just return the value. In this case, our TCO can eliminate the  `fac_tr(n-1, total * n)` but rather return its value. More specifically, we can examine the assembly generate by gcc by using `gcc -Wall -std=c11 -S -O3 -o example.asm fac_example.c`. You'll note that the naive recursion `fac` will have calls to itself in its assembly, while our `fac_tr` won't. 

```asm
fac:
... // other stuff that we can ignore
.L15:
  .cfi_restore_state
  leal  -2(%rbx), %edi
  call  fac // RECURSION HERE
``` 

```asm
fac_tr:
.LFB13:
  .cfi_startproc
  testl %edi, %edi
... // NO RECURSION HERE (but feel free to compile and check)
```

# Implementations
I've programmed my implementations in C, C#, and Python (I might do Haskell or F# later). My first version was written in Python, but CPython provides no TCO optimizations! The second version was written in C because the gcc compiler provides optimizations on sibling and tail recursive calls, but the algorithm only produces wrong answers as the unsigned 64-bit integer overflows (I know people provide Big Integer libraries - I just didn't feel like using it). For my third implementation I decided on C# because it provides high-level abstractions like standard library `BigInteger` with optimizations from the JIT. 

I'll display the python snippets because it's the simplest to understand. [The others (along with performance benchmarks) are on a git repo](https://github.com/tedkim97/fibonacci-fd-comparisons).

```python
# Tail Recursive Fast Double Exponentiation - O(log n)
def _fib_fd_tail_recursive(n: int, listn: list, ind: int):
    if(n == 0):
        tc, td = 0, 1
        for i in listn:
            tc, td = tc * (td * 2 - tc), (tc * tc) + (td * td)
            if(i % 2 != 0):
                tc, td = td, tc + td
        return tc
    
    listn[len(listn) - ind - 1] = n
    return _fib_fd_tail_recursive(n // 2, listn, ind + 1)

# Wrapper for the Tail Recursive Fast Double Exponentiation
def fib_fd_tr(n: int):
    if(n == 0):
        return 0
    num_fib_calls = math.floor(math.log(n, 2)) + 1
    listn = [None] * num_fib_calls
    return _fib_fd_tail_recursive(n, listn, 0)

# Iterative Version of the Fast Double Exponentiation Method - O(log n)
def fib_fd_iter(n: int):
    if(n == 0):
        return 0

    num_fib_calls = math.floor(math.log(n, 2)) + 1
    listn = [None] * num_fib_calls
    for ind in range(num_fib_calls):
        listn[num_fib_calls - ind - 1] = n
        n //= 2

    tc, td = 0, 1
    for i in listn:
        tc, td = tc * (td * 2 - tc), (tc * tc) + (td * td)
        if(i % 2 != 0):
            tc, td = td, tc + td

    return tc
```

# Results
All of these implementations were tested by computing `fib(100000)` for 1000 trials as a benchmark. One quirk is that C# is measured in ticks to get easier-to-read results.

##### C
Our iterative and tail recursive versions of our fast doubling algorithm perform very similarly - indicating that our TCO is working! We can examine if TCO is working by examining the assemblies of our optimized binaries (which we will do later). I didn't include the results of the naive recursive method for the fast doubling method because the results were incorrect (due to UB in my code) with higher optimization levels (`-O2` and `O3`). Even though the C code is really fast - our results are really wrong past the 93rd term. 

```bash
(ITER) Time elapsed 0.104128
(TR NAIVE) Time elapsed 0.038246
(FD ITER) Time elapsed 0.000078
(FD TR) Time elapsed 0.000076
```
{% include caption_image.html imgpath="/figures/calculations_shen_comix.png" alt="meme" caption="using C - credits to shencomix" %}

##### C#
In the C# version, our recursive, iterative, and tail recursive versions of our fast doubling methods have similar run-time averages (measured in ticks). The tail recursive version (113311 ticks) is faster than the iterative version (11425 ticks) which is faster than the naive recursion (11635 ticks). Realistically we can't claim that the tail recursive version is the fastest because of the standard deviations on our runtimes

Below are our results using the `Systems.Numeric.BigInteger library`.
```bash
Iteration(100000) <BigInteger> (ticks)
trials=1000 - avg=2503169.817 - std=1036866.4614311559 - min=1230671 - max=5125936
FD(100000) <BigInteger> (ticks)
trials=1000 - avg=11635.472 - std=1352.9710873540496 - min=10651 - max=20619
FD Iteration(100000) <BigInteger> (ticks)
trials=1000 - avg=11425.559 - std=826.2267004394132 - min=10663 - max=17409
FD Tail Recursion(100000) <BigInteger> (ticks)
trials=1000 - avg=11331.992 - std=718.4570564313501 - min=10694 - max=17592
```

Just for fun, I also included results using `ulong`. This datatype will run into the same problems as our C implementation.
```bash
Iteration(100000) <ulong> (ticks)
trials=1000 - avg=2763.548 - std=549.7612679118089 - min=2499 - max=8785
FD (100000) <ulong> (ticks)
trials=1000 - avg=9.499 - std=106.87950223967235 - min=3 - max=3374
FD Iteration(100000) <ulong> (ticks)
trials=1000 - avg=4.738 - std=70.77182600441799 - min=1 - max=2237
FD Tail Recursion(100000) <ulong> (ticks)
trials=1000 - avg=5.363 - std=76.05347612699912 - min=2 - max=2405
```

##### Python
Nothing exciting here. The iterative version is much faster than the naive and tail-recursive implementations of our fast doubling algorithm. 

```bash
Calculating speed for fib(100000) - 1000 times
fib_iter(100000) => 146.0427008
fib_fd(100000) => 4.733899500000007 # this implementation was excluded from the repo - I just used a popular one on the internet
fib_fd_iter(100000) => 3.0186159000000146
fib_fd_tr(100000) => 4.225305200000008
```

# Complications In Translation
You might be wondering why my implementations look so complicated and ugly, especially when they come from a [clean recursion this guy provides](https://www.nayuki.io/page/fast-fibonacci-algorithms). The `O(n)` algorithm has a simple iterative and tail recursive forms - why shouldn't the fast doubling method have one too?

```python
# Simple Iteration - O(n)
def fib_iter(n: int):
    n0, n1 = 0, 1
    while(n > 0):
        n0, n1 = n1, n0 + n1
        n = n - 1
    return n0

# Simple Tail Recursion O(n)
def fib_tail_recursive(n: int, n0, n1):
    if(n == 0):
        return n0
    return fib_tail_recursive(n-1, n1, n0 + n1)
```

### Why isn't there a simple solution?
Let's understand the complications of writing a "bottom-up" algorithm by visualizing recursions of the `O(n)` recursion and fast doubling method!

#### Visualizing O(n) Tail Recursion 
Below are recursive calls $Fib(7)$ would make. 

{% include fib_naive_vis.html %}

Creating an iterative or tail recursive version of the $O(n)$ Fibonacci sequence is "easy" because solving the problem "bottom-up" is simple. The ordering of $n$ doesn't matter because as long as we add our numbers correctly (and the right number of times), the "direction" of the operation doesn't matter. In terms of the visualization, it's easy to find a path from $Fib(n)$ from $Fib(0)$ and vice versa. 

#### Visualizing The Fast Doubling Method O(log(n))
Below are recursive calls $Fib(7), Fib(6), Fib(5), Fib(4)$ would make. (As a reminder: $Fib(n)$ returns a tuple with the $n$th and $n+1$th term of the Fibonacci sequence. Furthermore, the fast doubling method allows us to calculate $Fib(2n), Fib(2n+1)$ and given $Fib(n), Fib(n+1)$.)

{% include fib_fd_vis.html %}

The fast doubling recursion for $Fib(n)$ makes a recursive call to $Fib(\left \lfloor n / 2 \right \rfloor)$. Depending on the parity of $n$, our function will use $Fib(\left \lfloor n / 2 \right \rfloor)$ to compute $(Fib(n), Fib(n+1))$ (even) or $(Fib(n+1), Fib(n+2))$ (odd).

Implementing this with a "bottom-up" approach is complicated because there is no clear way of calculating all of the intermediate terms from $Fib(0)$ to $Fib(N)$. Our floor-division operation ($\left \lfloor N / 2 \right \rfloor$) is not [injective](https://en.wikipedia.org/wiki/Injective_function)[^1], meaning that it collapses values like $152$ or $153$ into the same value ($76$)! This means that given $Fib(n)$, it's unclear whether we should compute $Fib(2n)$ or $Fib(2n+1)$ to compute an arbitrary Fibonacci number. In order words, it's easy to compute a "top-down" path from $n$ to $0$, but difficult to compute a "bottom-up" path from $0$ from $n$. It might be easy to compute the sequence of N's for very small values like 2 or 3, but becomes difficult for large n like 10000, 999901, etc. 

In terms of this visualization, it's easy to figure out a path from any node on the tree to the root $Fib(0)$ (just divide by 2), but difficult to start at $Fib(0)$ and reach an arbitrary node.   

[^1]: Therefore not bijective - ohohoho.


### Asymptotically the same, but slightly different. 
Like I mentioned earlier, my iterative and tail-recursive versions are not true translations of the fast doubling method. While the time and space complexities are asymptotically the same, my implementations take a bit more time and space than the naive recursion. The iterative and recursive versions have an overhead of `O(log n) + O(log n)`. The process of calculating the n to iterate takes \~O(log n), and the fast doubling calculations also takes another \~`O(log n)`. The space complexity of the iterative and tail-recursive function is `O(log n)`. Some people might say that the space complexity of the naive fast doubling algorithm is `O(1)`, but if you factor in memory taken on the stack, then the space complexities are the same. Although my tail recursive version requires takes more parameters (and a bit more memory as a result). 

# Examining the Optimization
I decided to examine the assemblies of my C implementations to see if gcc was really applying TCO. 

Below is a snippet from the assembly using no optimization flags (`gcc -Wall -std=c11 -S -o fib_compiled_no_opt.s fibonacci.c -lm`):
```
fib_fde_tail_recursive:
.LFB4:
  .cfi_startproc
  pushq %rbp
  .cfi_def_cfa_offset 16
  .cfi_offset 6, -16
  movq  %rsp, %rbp
  .cfi_def_cfa_register 6
  subq  $80, %rsp
  movl  %edi, -52(%rbp)
  movq  %rsi, -64(%rbp)
  movl  %edx, -56(%rbp)
  movl  %ecx, -68(%rbp)
  ... ; blah blah blah
  movq  -64(%rbp), %rax
  movl  %edx, %ecx
  movl  %esi, %edx
  movq  %rax, %rsi
  call  fib_fde_tail_recursive ; HERE
```
Below is a snippet from the assembly using no optimization flags (`gcc -Wall -std=c11 -O3 -S -o fib_compiled_O3.s fibonacci.c -lm`):
```
fib_fde_tail_recursive:
.LFB16:
  .cfi_startproc
  testl %edi, %edi
  je  .L135
  movslq  %ecx, %rax
  movslq  %edx, %rdx
  subq  %rdx, %rax
  leaq  -4(%rsi,%rax,4), %rax
  ... ; blah blah blah
  jne .L134
  rep ret
  .p2align 4,,10
  .p2align 3
.L137:
  xorl  %eax, %eax
  ret ; NO RECURSION
  .cfi_endproc
```

# Conclusion
This was a lot of work for a small payoff. 