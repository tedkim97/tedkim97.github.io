---
layout: post
title: The repercussions of missing an Ampersand in C++ & Rust
subtitle: Copying vs Passing by reference
published: true
enable_latex: false
permalink: rust_cpp_missing_ampersand
frontpage: true
technical: true
funstuff: false
tags:
  - rust
  - c++
concepts:
  - programming
  - c++
  - memory
  - rust
---

# TL;DR

[There's a funny typo that causes someone to copy data instead of "referencing" in C++](https://youtu.be/Nm9-xKsZoNI?t=1367). Rust is nice because it provides defaults that protect you from some of these "dumb" mistakes[^1]. In this example, I'll go over how the "move by default" can prevent us from introducing this subtle behavior.

[^1]: Although these defaults can feel frustrating and cause other parts of the language to feel clunky to me

# Motivation

I originally hesitated to write this because I thought the topic was too "obvious", but I did it anyways after watching this [presentation discussing migrating from C++14 to C++20](https://youtu.be/Nm9-xKsZoNI). I was specifically [inspired by a performance bug due to a typo](https://youtu.be/Nm9-xKsZoNI?t=1367). This mistake is the "value param" vs "reference param" where your function **copies** a value instead of passing it by reference because an ampersand (`&`) was missing... Here's a minimum version of the difference below: 

```cpp
// You're the mythical 100x developer
void BusinessLogic(const Data& d) {
  // ...
}

// You're trash that deserves to be replace by AI
void BusinessLogicThatCopies(const Data d) {
  // ...
}
```

This simple typo is easy to miss and the penalty won't matter for people who aren't performance sensitive (although if you aren't strongly affected by stuff like this you probably don't need to be using C++). One could argue that this example is a one-off and no competent C++ developer would make this mistake, but I've even seen it in Google codebases (interpret that as you well). There are plenty of linters and tools to detect issues like this ([ex: clang-tidy can scan for unnecessary value params](https://clang.llvm.org/extra/clang-tidy/checks/performance/unnecessary-value-param.html)), but evidently these issues go unnoticed until a customer complains about it or someone actually bothers to profile the code. The fact that we have to be vigilant about such a minor behavior is exhausting, and that maybe we should design our language to guide us to sensible defaults.

# Rust Defaults 

I like Rust because it provides a handful of C++ patterns by default[^2]. Compared to the Rust hype marketing this benefit is quite small compared to "memory safety" and "fearless concurrency", but I like the improved ergonomics nonetheless. Adopting performance oriented defaults removes a lot of the weird "gotchas" early on in the C++ learning curve, as well as the toil about having the proper tooling setup. For brevity, I'll just focus on the concept of C++ ownership (`std::move`) and reference parameters to keep things short.

[^2]: Granted, these repercussions of these defaults also result in (in my opinion) verbose language constructs like `iter`, `into_iter`, `iter_mut`

1. By default "pass by value" in rust moves objects (unless the object implements the `Copy` trait) instead of copying them

Rust's "pass by value" behavior differs from C++'s behavior where it copies the object. This has some subtle implications and its hard to grasp why this is nice without visualizing the C++ code. So let's start with the toy example we started at the beginning. In the below C++ snippet, passing our expensive-to-copy struct "by value" will result in the struct being copied: 

```cpp
void BusinessLogic(const Data d) {
  d.DoThing();
}

Data expensive_to_copy = Data{...};
BusinessLogic(expensive_to_copy);
```

Copying is desirable for types that are more performant to copy (like int, bool, floats, etc), but for larger objects/heap allocated objects, it will slow down our code. If you're trying to execute based on the contents of the object, an improvement might "pass by reference" like so:

```cpp
// note the `const` + `&`
void BusinessLogic(const Data& d) {
  d.DoThing();
}

Data expensive_to_copy = Data{...};
BusinessLogic(expensive_to_copy);
```

Again, if we repeat that typo from the beginning (`const Data d`), only the linter will point out our mistake. 

There are some cases where you want your function to "take ownership" of the parameter you're passing to it, so you might employ a "move" using something like this:

```cpp
std::unique_ptr<Owner> CreateOwner(Data &&d) {
	// ...
	return std::make_unique<Owner>(s);
}

Data expensive_to_copy = Data{...};
auto data_owner = FactoryFunction(std::move(expensive_to_copy));
```

However, moving in this context adds extra restrictions to the original object. After an object that have been "moved from", that you're not supposed to use the original object after it's been moved or else you've introducing potential bugs. For example, even though the below compiles, the snippet is bad and linters will complain about it.

```cpp
Data expensive_to_copy = Data{...};
auto data_owner = FactoryFunction(expensive_to_copy);
expensive_to_copy.DoThing();  // Linter will complain about using expensive_to_copy after it has been moved from.
// The compiler won't say anything. The maintainer needs to watch out for accidental uses
``` 

With Rust executing a function for either case deploys the "optimal" version (reference or move) by default, moreover, the compiler (not the linter) will point out the any improper "use after moves".

```rust
struct Data {
  // Vec cannot implement "Copy" type
  data: Vec<i32>,
}

// Equivalent to "passing by const-ref" in C++
fn BusinessLogic(d :&Data) {
  d.DoThing();
}

// Equivalent to "move" in C++
fn FactoryFunction(d: Data) -> Owner {
  owner = Owner{data: d};
  // ...
  return owner
}
```

Rust prevents us from accidentally writing sub-optimal versions of the C++ function (`BusinessLogic(const Data d)`)... with the caveat that this choice propagates throughout the language, which can be unintuitive or confusing.

# Revisiting the bug with Rust

Now that we have established context with the fake example, let's try to see how rust could've prevented the presentation's problem in a practical instance.

## #1 `vec::retain` 

The rust library function for removing elements of a vector (`vec::retain`) doesn't give us an option to use a closure that copies by value. Even if we wanted to make a lambda that copies the elements of our vector, the compiler notices the type mismatch and rejects it.

If we were to take the approximation of the C++ code in the presentation (below)
```cpp
std::vector<Request> LoadRequests;

void OnComplete(int id) {
  // If you're confused why it removes elements from the vector like this, it's
  // used as a part of the "erase-remove" pattern. The context in the video is that
  // they were using C++14, so they couldn't use the C++20 std::erase_if
  const auto DeleteRange = std::ranges::remove_if(LoadRequests, [](const Request r){
     return r.id == id;
  });
  LoadRequests.erase(DeleteRange.begin(), DeleteRange.end());
}
```
and convert it to the idiomatic rust expression (below), and try to pass in a closure that has a typo (`Request` instead of `&Request`), the compiler will throw an error saying "type mismatch in closure arguments".

```rust
let mut LoadRequests : Vec<Request> = ...

fn OnCompleteDefault(id: i32, load_requests: &mut Vec<Request>) {
  load_requests.retain(|r: Request| r.id != id); // Throws a compiler error
} 
```

This is technically an example of a type system preventing us from making dumb mistakes rather than "moving by default" preventing these mistakes. Since "pass by value" is a move by default, the type system is able to come in and recognize the error. 

One could also argue that were comparing a  C++14 pattern to a newer standard library, isn't a fair comparison. So in order to convince the audience, I'll try to write an unidiomatic, suboptimal expression to see how far Rust can stop us from doing something dumb.

## #2 Weird Hypothetical Implementations

Suppose we hate using methods like `vec::retain` because you're trying to prove a point to strangers on the internet, let's try coding up a reasonable implementation. Let's start with the original "correct" version of the code, and add in random `&` typos to see what the compiler does. The "correct" version:

```rust
fn OnCompleteWeird(id: i32, LoadRequests: Vec<Request>) -> Vec<Request>{
  let mut filtered: Vec<Request> = Vec::new();
  for lr in LoadRequests.into_iter() {
    if lr.id == id {
        filtered.push(lr);
    }
  }
  filtered
}
```

Let's try making the parameter from the existing move (`Vec<Request>`) to borrow (`&Vec<Request>`). Trying to compile this variation results in an error because the `filtered.push` expects another move, but instead it got a reference (`expected Request, found &Request`). We can try following the compiler's recommendation to explicitly copy the element by using `.Clone()`, but that copy doesn't happen by accident (unless someone was blindly following what the compiler suggested to do).

Trying to compile the below fails because `iter()` will return references to the `Request` data, and we cannot convert this into a copy unless we explicitly `.Clone()` the underlying data. Again, I can't imagine a scenario where someone would do this when there are existing library functions that do this more efficiently... but it's nice to know that it's still hard to make the mistake.

## C++ can do this too

In C++'s Defense, C++ offers several ways to prevent copying, support automatically moving objects, etc. Things like delete copy constructors + assignors, make copy constructors explicit/move constructors implicit, leverage copy elision, etc. However, these methods can be annoying to use because you need to worry about rules like ["rules of 3/5/0"](https://en.cppreference.com/w/cpp/language/rule_of_three.html) and might be restricted to a specific version of C++.

In fact, a rust struct that doesn't derive/implement the `Copy` trait is similar to a C++ struct that makes its copy constructor explicit. Similarly, a rust struct that doesn't derive/implement the `Clone` trait is similar to a C++ struct that deleted its copy/copy assignment constructor. In a way, exclusion of these traits is another protective default.

# Conclusion

As a disclaimer, I'm not a fan of several aspects of Rust, but I do think some of its language defaults that are good for performant programs. More importantly, these defaults reduce the mental burden of double checking minor C++ traps, and lets me trust the compiler to do this for me.

# Appendix

## Pass by value, reference, pointer
This abseil source can probably explain this better than I can: abseil.io/tips/234

## Copy/Clone/Drop Traits
- https://doc.rust-lang.org/std/marker/trait.Copy.html
- https://doc.rust-lang.org/std/clone/trait.Clone.html