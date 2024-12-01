# Question 02 - Error Handling

## How would you go about implementing std::error::Error and Debug/Display in Rust

### Expert Explanation: Implementing `std::error::Error` and `Debug`/`Display` in Rust

In Rust, the `std::error::Error` trait is used to define custom error types. It works in conjunction with the `Debug` and `Display` traits, which are essential for error reporting and debugging. Let’s break down the concepts and implementation details to help you prepare for a technical interview.

---

#### **1. `std::error::Error` Trait**

The `std::error::Error` trait is designed to represent errors in a way that is consistent across the Rust ecosystem. It provides methods for describing the error and obtaining its cause (if any). Implementing this trait allows your error type to integrate seamlessly with the standard library and popular error-handling libraries like `thiserror` or `anyhow`.

Key methods of the `Error` trait:
- **`fn source(&self) -> Option<&(dyn Error + 'static)>`**: Provides the underlying cause of the error, if any. This is useful for error chaining.
- **`fn description(&self) -> &str`** *(deprecated)*: This method was used for simple string-based error descriptions but is now replaced by `Display`.

---

#### **2. `Debug` Trait**

The `Debug` trait is used for formatting the error for debugging purposes. It provides a machine-readable and detailed representation of the error type.

- You can derive the `Debug` trait using `#[derive(Debug)]` for most cases.
- For custom implementations, you implement the `fmt` method in the `Debug` trait.

---

#### **3. `Display` Trait**

The `Display` trait is used for user-friendly error messages. It defines how the error should be presented in human-readable format. This is crucial for printing error messages to logs or the console.

- To implement `Display`, you provide a custom implementation of the `fmt` method.

---

#### **Why Implement `Debug` and `Display` Together?**

Rust's error-handling ecosystem relies on both `Debug` and `Display`:
- `Debug` provides detailed information for developers.
- `Display` gives a user-friendly description for end users.
- The `Error` trait requires `Debug` to be implemented.

---

#### **4. Step-by-Step Implementation**

Here’s how you can implement a custom error type with `std::error::Error`, `Debug`, and `Display`.

```rust
use std::error::Error;
use std::fmt;

// Step 1: Define your custom error type
#[derive(Debug)]
struct MyError {
    message: String,
    source: Option<Box<dyn Error>>, // Optional source error for chaining
}

// Step 2: Implement the `Display` trait for user-friendly error messages
impl fmt::Display for MyError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{}", self.message) // Display the main error message
    }
}

// Step 3: Implement the `Error` trait for integration with Rust's error system
impl Error for MyError {
    fn source(&self) -> Option<&(dyn Error + 'static)> {
        self.source.as_deref() // Return the source error, if any
    }
}

// Example usage
fn main() {
    let underlying_error = std::io::Error::new(std::io::ErrorKind::Other, "disk not found");
    let custom_error = MyError {
        message: "Failed to perform the operation".to_string(),
        source: Some(Box::new(underlying_error)),
    };

    println!("Error: {}", custom_error); // Uses Display
    println!("Debug: {:?}", custom_error); // Uses Debug

    if let Some(source) = custom_error.source() {
        println!("Caused by: {}", source);
    }
}
```

---

#### **5. Key Points for Interviews**

1. **Traits Overview**:
   - `Debug`: For developer-focused error representation.
   - `Display`: For user-friendly error messages.
   - `Error`: For integrating custom error types into Rust's error-handling ecosystem.

2. **Error Chaining**:
   - Use the `source` method in the `Error` trait to point to underlying errors.
   - This facilitates detailed debugging and better error reporting.

3. **Usage of `Box<dyn Error>`**:
   - Enables dynamic dispatch for storing any error type in your custom error.

4. **Integration with Libraries**:
   - Libraries like `thiserror` or `anyhow` can simplify custom error handling, but understanding manual implementation demonstrates in-depth Rust knowledge.

---

#### **6. Advanced Tips**

- **Customizing `Debug`**:
  For more control over the debug output:
  ```rust
  impl fmt::Debug for MyError {
      fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
          write!(f, "MyError {{ message: {:?}, source: {:?} }}", self.message, self.source)
      }
  }
  ```

- **Error Composition**:
  Use enums to handle multiple error types in a single custom error.
  ```rust
  #[derive(Debug)]
  enum AppError {
      IoError(std::io::Error),
      ParseError(String),
  }

  impl fmt::Display for AppError {
      fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
          match self {
              AppError::IoError(e) => write!(f, "I/O Error: {}", e),
              AppError::ParseError(e) => write!(f, "Parse Error: {}", e),
          }
      }
  }

  impl Error for AppError {
      fn source(&self) -> Option<&(dyn Error + 'static)> {
          match self {
              AppError::IoError(e) => Some(e),
              AppError::ParseError(_) => None,
          }
      }
  }
  ```

---

#### **Conclusion**

Understanding how to implement `std::error::Error`, `Debug`, and `Display` is essential for writing robust, idiomatic Rust code. These traits ensure your custom error types integrate well with the broader Rust ecosystem, enable effective debugging, and provide clear error messages. During the interview, focus on:
- Explaining the role of each trait.
- Demonstrating error chaining with `source`.
- Showcasing practical examples of custom error implementations.