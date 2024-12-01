# Question 06 - General Language Concepts

## What is the difference between shallow copy and deep copy in Rust?

Shallow Copy: Transfers ownership without duplicating the underlying data (move semantics). 

Example:
```rust
let s1 = String::from("hello");
let s2 = s1; // `s1` is invalidated; only a pointer is copied.

```
Deep Copy: Explicitly duplicates the data. 

Example:
```rust
let s1 = String::from("hello");
let s2 = s1.clone(); // Creates a new allocation with the same data.
println!("{}", s1); // Valid because `s1` still owns its data.

```
By combining these explanations with examples, candidates can demonstrate a deep understanding of Rust's ownership and borrowing system, critical for writing safe and performant Rust code.