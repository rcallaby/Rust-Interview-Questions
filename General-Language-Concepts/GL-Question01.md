# Question 01 - General Language Concepts

## Can you explain Rust's ownership model and how it differs from garbage-collected languages?

Rust’s ownership model is a set of rules that governs how memory is managed. At its core, it ensures that each piece of memory is managed by a single "owner," which is responsible for cleaning up that memory when it goes out of scope.

Core Rules of Ownership:
    Each value in Rust has a single owner at a time.
    When the owner goes out of scope, the value is dropped (deallocated).
    Ownership can be transferred (moved) to another variable.

Differences from Garbage-Collected Languages:
    No Runtime Garbage Collector: Unlike garbage-collected languages (e.g., Java, Python), Rust does not have a garbage collector to clean up unused memory. Instead, memory management is deterministic and enforced at compile time.
    Compile-Time Enforcement: Ownership rules are checked during compilation, ensuring memory safety without introducing runtime overhead.
    Predictable Performance: Because memory is deallocated immediately when it goes out of scope, Rust avoids unpredictable pauses caused by garbage collection cycles.

Example:
```rust
let s1 = String::from("hello"); // `s1` owns the string.
let s2 = s1;                   // Ownership is moved to `s2`. `s1` is no longer valid.
println!("{}", s1);            // Compile-time error: `s1` is invalid after the move.
```

Key Benefit: Rust’s ownership model eliminates issues like dangling pointers, data races, and memory leaks, which are common in manual memory management.