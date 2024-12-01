# Question 03 - General Language Concepts

## What are dangling references, and how does Rust prevent them?

Definition: A dangling reference occurs when a reference points to memory that has already been deallocated.
Rust's Prevention:

    Ownership ensures memory is not deallocated while there are still valid references.
    The borrow checker enforces that references cannot outlive the data they point to.

Example:
```rust
let r;
{
    let x = 5;
    r = &x; // Compile-time error: `x` does not live long enough.
}
println!("{}", r);

```