# Question 01 - Error Handling

## When and how do you use the ? operator for propogating errors in Rust?

The `?` operator in Rust is a shorthand for propagating errors in functions that return a `Result` or `Option`. It simplifies error handling by eliminating the need for verbose `match` expressions to unpack and propagate errors.

#### **How It Works**
The `?` operator can only be used in functions that return either:
1. A `Result` (`Result<T, E>`)
2. An `Option` (`Option<T>`)

When applied to a `Result` or `Option`, the `?` operator performs the following actions:
- If the value is `Ok(T)` (or `Some(T)`), it extracts and returns the value `T`.
- If the value is `Err(E)` (or `None`), it immediately returns the error (or `None`) from the function in which it is used.

Essentially, it is a shortcut for this pattern:
```rust
match result {
    Ok(value) => value,
    Err(err) => return Err(err),
}
```

#### **Usage**

Here is a step-by-step explanation with examples:

1. **Function that returns a `Result`:**
   The `?` operator can propagate errors up the call stack.
   ```rust
   use std::fs::File;
   use std::io::{self, Read};

   fn read_file_to_string(file_path: &str) -> Result<String, io::Error> {
       let mut file = File::open(file_path)?; // Propagates error if File::open fails
       let mut contents = String::new();
       file.read_to_string(&mut contents)?; // Propagates error if read_to_string fails
       Ok(contents)
   }

   fn main() {
       match read_file_to_string("example.txt") {
           Ok(contents) => println!("File contents: {}", contents),
           Err(e) => eprintln!("Error reading file: {}", e),
       }
   }
   ```

   In this example:
   - If `File::open` or `read_to_string` returns an `Err`, the `?` operator propagates it.
   - If both operations succeed, the function proceeds.

2. **Function that returns an `Option`:**
   The `?` operator propagates `None` for optional values.
   ```rust
   fn divide_numbers(a: i32, b: i32) -> Option<i32> {
       if b == 0 {
           None?
       } else {
           Some(a / b)
       }
   }

   fn main() {
       match divide_numbers(10, 2) {
           Some(result) => println!("Result: {}", result),
           None => println!("Cannot divide by zero!"),
       }
   }
   ```

   Here, `None?` acts like a `return None;` statement when `b` is zero.

3. **Combining `Result` and `Option`:**
   In functions that work with both types, the `Option` must be explicitly converted to a `Result` before using `?`.
   ```rust
   fn find_and_open_file(file_name: &str) -> Result<File, String> {
       let path = find_file(file_name).ok_or("File not found")?; // Convert Option to Result
       File::open(&path).map_err(|e| e.to_string()) // Convert IO error to string
   }

   fn find_file(file_name: &str) -> Option<String> {
       Some(format!("/tmp/{}", file_name)) // Example path resolution
   }

   fn main() {
       match find_and_open_file("example.txt") {
           Ok(file) => println!("File opened successfully: {:?}", file),
           Err(e) => eprintln!("Error: {}", e),
       }
   }
   ```

   The `ok_or` method is used to convert an `Option` into a `Result`.

4. **Custom Error Types:**
   If your function uses a custom error type, ensure it implements `From<E>` for automatic error conversion.
   ```rust
   use std::fs::File;
   use std::io;

   #[derive(Debug)]
   enum MyError {
       Io(io::Error),
       Parse(std::num::ParseIntError),
   }

   impl From<io::Error> for MyError {
       fn from(error: io::Error) -> Self {
           MyError::Io(error)
       }
   }

   impl From<std::num::ParseIntError> for MyError {
       fn from(error: std::num::ParseIntError) -> Self {
           MyError::Parse(error)
       }
   }

   fn parse_file(file_path: &str) -> Result<i32, MyError> {
       let mut file = File::open(file_path)?; // Automatically converts io::Error to MyError
       let mut contents = String::new();
       file.read_to_string(&mut contents)?;
       let number: i32 = contents.trim().parse()?; // Automatically converts ParseIntError to MyError
       Ok(number)
   }

   fn main() {
       match parse_file("number.txt") {
           Ok(num) => println!("Parsed number: {}", num),
           Err(e) => eprintln!("Error: {:?}", e),
       }
   }
   ```

   In this example:
   - `io::Error` and `ParseIntError` are seamlessly converted into `MyError` using `?`.

#### **Benefits**
- **Readability:** Reduces boilerplate and makes error handling concise.
- **Automatic Propagation:** Handles common patterns of propagating errors up the call stack.
- **Composability:** Works seamlessly with custom error types and conversion traits.

#### **Limitations**
1. The `?` operator cannot be used outside functions returning `Result` or `Option`.
2. Functions must have compatible error types; otherwise, conversions are required (`map_err`, `From`, etc.).
3. It only propagates errors; custom handling (like logging) requires additional code.

#### **Summary**
The `?` operator is an elegant tool in Rust's error-handling arsenal. It streamlines error propagation while maintaining clarity and safety. Mastery of `?` involves understanding its constraints (return type compatibility) and using complementary features like custom error types and conversion traits to handle real-world scenarios.