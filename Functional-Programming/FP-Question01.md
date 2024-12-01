# Question 01 - Functional Programming

## Explain how closures work in Rust. How do they differ from regular functions?

Closures in Rust are a powerful feature that allows you to create anonymous functions with the ability to capture variables from their surrounding environment. Here's a detailed explanation:

---

### **Closures in Rust**
A **closure** is a function-like construct that can capture variables from its surrounding scope. They are defined using a concise syntax:

```rust
let add_one = |x| x + 1;
```

In the example above, `add_one` is a closure that takes one argument `x` and returns `x + 1`.

---

### **Key Characteristics of Closures**

1. **Inference of Parameter and Return Types**  
   Unlike regular functions, closures can often infer the types of their parameters and return values. For example:

   ```rust
   let multiply = |x, y| x * y;
   ```

   Rust infers the types of `x` and `y` from the context where the closure is used. If type inference isn't possible, you can annotate the types explicitly:

   ```rust
   let multiply = |x: i32, y: i32| -> i32 { x * y };
   ```

2. **Capture of Environment Variables**  
   Closures can capture variables from their enclosing environment in one of three ways:
   - **By Borrowing (`&T`)**: Immutable borrow of variables.
   - **By Mutably Borrowing (`&mut T`)**: Mutable borrow of variables.
   - **By Taking Ownership (`T`)**: Consumes the variables.

   Rust determines the capture mode automatically, depending on how the closure uses the variables. For instance:

   ```rust
   let x = 10;
   let print_x = || println!("{}", x); // Captures `x` by borrowing
   print_x();
   ```

   If the closure mutates a variable, it captures it by mutable borrow:

   ```rust
   let mut count = 0;
   let mut increment = || count += 1; // Captures `count` mutably
   increment();
   println!("{}", count);
   ```

3. **Closures are Flexible**  
   Closures are implemented using three traits:
   - **`Fn`**: For closures that capture variables immutably.
   - **`FnMut`**: For closures that capture variables mutably.
   - **`FnOnce`**: For closures that consume variables (take ownership).

   These traits dictate how closures can be used and are chosen automatically by Rust based on the closure's body.

4. **Stored on the Stack**  
   Closures are often stored on the stack as a concrete type. When passed as function arguments, they are passed as a generic type or a trait object.

---

### **How Closures Differ from Regular Functions**

| Feature                  | Closures                                   | Regular Functions                        |
|--------------------------|--------------------------------------------|------------------------------------------|
| **Environment Access**   | Can capture variables from the environment | Cannot capture variables; rely only on arguments |
| **Type Inference**       | Types of parameters and return values are often inferred | Types must be explicitly defined         |
| **Anonymous**            | Typically anonymous, can be stored in variables | Named with explicit definitions          |
| **Traits**               | Implement `Fn`, `FnMut`, or `FnOnce`      | Do not implement these traits            |
| **Usage Flexibility**    | Can be passed as arguments, returned, or stored | Cannot capture or rely on outer variables |

---

### **Example: Closure vs Regular Function**

```rust
// Regular Function
fn multiply_by_two(x: i32) -> i32 {
    x * 2
}

// Closure
let multiply_by_two_closure = |x: i32| x * 2;

let num = 5;
println!("{}", multiply_by_two(num));        // Output: 10
println!("{}", multiply_by_two_closure(num)); // Output: 10
```

Here, both achieve the same outcome, but the closure is more concise and can access its surrounding environment if needed.

---

### **Advanced: Move Keyword with Closures**
Closures can explicitly take ownership of their environment using the `move` keyword:

```rust
let s = String::from("Hello");
let consume_string = move || println!("{}", s);
// `s` is moved into the closure and cannot be used afterward
```

---

By understanding closures and their traits (`Fn`, `FnMut`, `FnOnce`), you'll be well-prepared to explain and use closures effectively in Rust, a critical skill for technical interviews involving Rust.