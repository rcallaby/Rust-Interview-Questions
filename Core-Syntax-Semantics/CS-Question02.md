# Question 02 - Core Syntax

## What is shadowing, and how is it different from using mut?

---

### **Shadowing**

**Definition**:
- Shadowing is a feature in Rust that allows you to **redeclare a variable with the same name** in the same scope or a nested scope. 
- The new declaration effectively "shadows" the previous one, making the previous variable inaccessible while the shadowed version is in scope.

**Key points**:
1. Shadowing creates a new binding.
2. The new binding can have a different type or mutability than the original.
3. It is primarily used to transform or update a variable's value while keeping the same name, without mutating the original binding.

**Example**:
```rust
fn main() {
    let x = 5; // Initial declaration
    let x = x + 1; // Shadowed: x now equals 6
    let x = "shadowed"; // Shadowed again: x is now a string
    println!("{}", x); // Outputs: shadowed
}
```

Here:
- The first `x` is an integer.
- The second `x` is a new variable, shadowing the first and adding 1.
- The third `x` changes the type to a string.

**Use cases**:
- Data transformation where immutability of the original binding is desired.
- Avoiding unnecessary variable names while applying transformations or updates.

---

### **Using `mut`**

**Definition**:
- `mut` is a keyword in Rust that allows a variable to be **mutable**, meaning its value can be changed after it is declared.

**Key points**:
1. It modifies the behavior of a variable, allowing it to be reassigned to new values.
2. The type of the variable cannot change during its lifetime.
3. It does not create a new binding; the original variable is updated.

**Example**:
```rust
fn main() {
    let mut x = 5; // Mutable variable
    x += 1; // Value updated: x now equals 6
    println!("{}", x); // Outputs: 6
}
```

Here:
- The variable `x` is declared as mutable with `mut`.
- Its value is directly updated in place.

**Use cases**:
- Situations requiring in-place updates, such as counters, accumulators, or algorithms involving iterative updates.

---

### **Key Differences**

| Aspect               | Shadowing                                  | `mut`                                      |
|-----------------------|--------------------------------------------|--------------------------------------------|
| **Definition**        | Creates a new variable binding.           | Modifies the existing variable.            |
| **Type flexibility**  | The new variable can have a different type.| The type remains fixed.                    |
| **Mutability**        | Each shadowed binding can be immutable or mutable independently. | Mutability is fixed at declaration.        |
| **Memory behavior**   | May allocate new memory (depending on type).| Updates the same memory location.          |
| **Use cases**         | Transformations, type changes, keeping immutability of original variable. | Iterative updates, counters, direct value changes. |
| **Scope**             | Affects only the scope it’s redeclared in.| Affects the entire variable's lifetime.    |

---

### **Best Practices**
- **Use shadowing** when you need to transform a value, change types, or keep the original variable immutable for safety and clarity.
- **Use `mut`** when you need to modify a variable directly and there’s no need for additional bindings or type changes.

---

### **Example Combining Both**
```rust
fn main() {
    let x = 5; // Immutable binding
    let x = x + 1; // Shadowing: x now equals 6
    let mut y = x * 2; // Mutable variable: y equals 12
    y += 3; // Updating y: y now equals 15
    println!("x: {}, y: {}", x, y); // Outputs: x: 6, y: 15
}
```

- Here, `x` is shadowed to transform its value immutably.
- `y` is declared mutable because its value is updated multiple times.

---

By explaining these points clearly and providing relevant examples, you demonstrate a deep understanding of Rust's features, their distinctions, and appropriate use cases.