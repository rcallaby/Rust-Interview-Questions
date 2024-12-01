# Question 05 - General Language Concepts

## Can you explain the concept of lifetimes and how they relate to borrowing?

Definition: Lifetimes are annotations that specify how long references are valid.
Purpose: They ensure references do not outlive the data they point to, avoiding dangling references.
Syntax: Lifetimes are denoted by 'a, 'b, etc., and used in function signatures to specify relationships between lifetimes.

Example:
```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

```

