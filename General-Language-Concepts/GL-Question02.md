# Question 02 - General Language Concepts

## How does Rust ensure memory safety without a garbage collector?

Rust achieves memory safety by enforcing strict rules at compile time through its ownership and borrowing system. Here are the key mechanisms:

Ownership and Lifetime Rules:
    Each value has a single owner.
    References to a value cannot outlive the owner, preventing use-after-free errors.
    When the owner is dropped, the memory is safely deallocated.

Borrowing:
    Instead of duplicating ownership, Rust allows values to be "borrowed" temporarily, either immutably or mutably, ensuring safe concurrent access.
    Immutable borrows allow read-only access, while mutable borrows allow exclusive write access.

Move Semantics:
    When a value is moved, its ownership is transferred to the new owner, ensuring the original owner cannot access it anymore, preventing double-free errors.

Data Race Prevention:
    At most, one mutable reference or multiple immutable references can exist at any time.
    This guarantees that no two threads can simultaneously write to the same memory, eliminating data races.

Compile-Time Checks:
    Rust’s compiler performs static analysis to ensure that all these rules are followed before the program runs.

Example:
```rust
let mut s = String::from("hello");
let r1 = &s;  // Immutable borrow
let r2 = &s;  // Immutable borrow is fine
let r3 = &mut s; // Compile-time error: cannot borrow `s` as mutable because it is also borrowed as immutable.

```