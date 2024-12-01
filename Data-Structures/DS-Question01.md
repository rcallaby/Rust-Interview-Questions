# Question 01 - Data Structures

## Can you please explain the differences between enum and struct usage in Rust?

In Rust, **`enum`** and **`struct`** serve distinct purposes in expressing data structures. Here's a breakdown of their differences, usage, and the scenarios where each would be preferred:

---

### **Key Differences**
| Feature                  | `struct`                                        | `enum`                                        |
|--------------------------|------------------------------------------------|----------------------------------------------|
| **Purpose**              | Groups related data into a single cohesive unit. | Represents one of several possible variants of a type. |
| **Use Case**             | Modeling entities with fixed attributes.        | Modeling data with multiple, distinct states. |
| **Variants**             | Has a single shape (fixed fields).              | Has multiple variants, each with its own shape. |
| **Pattern Matching**     | Not pattern matched (direct field access).      | Frequently used with pattern matching for control flow. |
| **Flexibility**          | Best for homogeneous, consistent data.          | Best for heterogeneous, state-dependent data. |

---

### **When to Use `struct`**
A **`struct`** is used to define an object or entity with a fixed set of attributes. It's similar to a class or object in other programming languages. You'd use a `struct` when:
- You want to represent an entity with a **fixed structure**.
- The entity always has the same attributes.
- The attributes have consistent and predictable types.

#### Example:
```rust
struct User {
    username: String,
    email: String,
    age: u8,
}

fn print_user(user: &User) {
    println!("User: {}, Email: {}, Age: {}", user.username, user.email, user.age);
}
```

Here, `User` always has a `username`, `email`, and `age`. The structure is fixed and predictable.

---

### **When to Use `enum`**
An **`enum`** is used to represent a type that can have multiple, mutually exclusive states or variants. It's especially useful for handling state-dependent data or logic where each variant might carry different data.

You'd use an `enum` when:
- You need to represent a **choice** or **state**.
- The data can take on one of several **different forms**.
- You want to leverage **pattern matching** to cleanly handle each variant.

#### Example:
```rust
enum PaymentMethod {
    CreditCard { number: String, expiry: String },
    PayPal { email: String },
    BankTransfer { account_number: String },
}

fn process_payment(payment: PaymentMethod) {
    match payment {
        PaymentMethod::CreditCard { number, expiry } => {
            println!("Processing credit card: {} (exp {})", number, expiry);
        }
        PaymentMethod::PayPal { email } => {
            println!("Processing PayPal payment for email: {}", email);
        }
        PaymentMethod::BankTransfer { account_number } => {
            println!("Processing bank transfer to account: {}", account_number);
        }
    }
}
```

Here, `PaymentMethod` models three distinct ways of paying, and each has its own associated data. The structure is inherently variable depending on the variant.

---

### **Choosing Between `enum` and `struct`**
- Use **`struct`** when:
  - The data **structure is uniform**.
  - You want **direct field access** without needing to pattern match.
  - You are modeling entities like users, products, or configurations.

- Use **`enum`** when:
  - The data represents **different states or categories**.
  - Variants need to carry **different types of data**.
  - You want to use **pattern matching** for cleaner and safer control flow.

---

### **Advanced Use: Combining `struct` and `enum`**
Often, `struct` and `enum` are used together for more complex data models. For example:

```rust
struct Rectangle {
    width: f64,
    height: f64,
}

struct Circle {
    radius: f64,
}

enum Shape {
    Rectangle(Rectangle),
    Circle(Circle),
}

fn calculate_area(shape: Shape) -> f64 {
    match shape {
        Shape::Rectangle(rect) => rect.width * rect.height,
        Shape::Circle(circle) => 3.14159 * circle.radius * circle.radius,
    }
}
```

Here:
- `Rectangle` and `Circle` are `struct`s used for fixed shape data.
- `Shape` is an `enum` used to represent one of several possible shapes.

This combination lets you model both fixed attributes and variant-specific behavior effectively.

---

### **Summary**
- Use `struct` to model **uniform, fixed** entities.
- Use `enum` to model **variant-based, state-dependent** data.
- When in doubt:
  - If the data has **consistent structure**, use `struct`.
  - If the data has **distinct states or types**, use `enum`.
