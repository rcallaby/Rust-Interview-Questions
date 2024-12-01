# Question 01 - Core Syntax

## What’s the difference between const and a let binding?

In Rust, `const` and `let` serve distinct purposes and are used in different scenarios. Here’s a detailed explanation of their differences:

### 1. **Mutability and Immutability**
   - **`const`**: A `const` declaration is always immutable. Once defined, its value cannot be changed, and attempting to do so will result in a compile-time error. It does not support `mut`.
   - **`let`**: By default, `let` bindings are immutable, but you can make them mutable using the `mut` keyword. This allows you to reassign values to the variable.

     ```rust
     const MAX_POINTS: u32 = 100; // Immutable and cannot be changed
     let x = 10;                  // Immutable by default
     let mut y = 20;              // Mutable, can be changed
     ```

### 2. **Compile-Time vs Runtime**
   - **`const`**: The value of a `const` must be known at compile time. This means it can only be assigned values that are deterministic and evaluable at compile time. For example, literals or constant expressions can be used.
   - **`let`**: A `let` binding's value can be determined at runtime. You can initialize it with values that are calculated or derived dynamically.

     ```rust
     const SECONDS_IN_A_DAY: u32 = 24 * 60 * 60; // Compile-time constant

     let runtime_value = get_input();            // `let` supports runtime values
     ```

### 3. **Scope**
   - **`const`**: Constants are implicitly static and have a global scope when defined at the top level. They are accessible throughout the module or program where they are defined.
   - **`let`**: A `let` binding has a more localized scope, limited to the block in which it is declared.

     ```rust
     const GREETING: &str = "Hello, world!"; // Accessible globally
     
     fn main() {
         let local_variable = "I'm local";   // Accessible only within main()
     }
     ```

### 4. **Type Annotation**
   - **`const`**: The type of a `const` must always be explicitly annotated. This ensures that its type is clear at compile time.
   - **`let`**: Type annotations for `let` bindings are optional and can often be inferred by the compiler.

     ```rust
     const PI: f64 = 3.14159; // Explicit type annotation is required

     let radius = 5.0;        // Type inferred as f64
     let diameter: f64 = 10.0; // Optional explicit type annotation
     ```

### 5. **Memory Allocation**
   - **`const`**: A `const` does not occupy a specific memory location during execution. Instead, the compiler directly inlines its value wherever it is used. This behavior can optimize performance and reduce memory overhead.
   - **`let`**: A `let` binding creates a variable in memory, and its value is stored at runtime. Even for immutable variables, memory is allocated, and the value is read from that memory location.

     ```rust
     const VALUE: i32 = 42; // No memory allocated, VALUE is inlined
     let variable = VALUE;  // Memory is allocated for `variable`
     ```

### 6. **Usage Contexts**
   - **`const`**: Used for defining constants that are globally accessible and should not change. These are ideal for settings, magic numbers, or fixed configuration values.
   - **`let`**: Used for defining variables whose values may be determined dynamically at runtime and can optionally be mutable.

     ```rust
     const BASE_URL: &str = "https://api.example.com"; // Constant value

     fn main() {
         let username = get_username(); // Value determined at runtime
     }
     ```

### 7. **Shadowing**
   - **`const`**: Constants cannot be shadowed. You cannot redefine a `const` with the same name in the same scope.
   - **`let`**: Variables declared with `let` can be shadowed, allowing you to reuse the same name in the same scope.

     ```rust
     let x = 5;
     let x = x + 1; // Shadowing is allowed

     const VALUE: i32 = 10;
     // const VALUE: i32 = 20; // Error: Cannot shadow a const
     ```

### Key Takeaway
Use `const` for values that are constant at compile time, global, and immutable, while `let` is better suited for variables that may be dynamic, scoped locally, or mutable.