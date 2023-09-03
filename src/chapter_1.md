# Rust makes it look easy

* Strong emphasis on safety and correctness from the ground up.
* Naturally promotes reliable software development.
* Catches most common programming errors during compile time.
* Mitigates or eliminates undefined behavior.
* Does not rely on static analysis tools, which generates many false positives and 
  may not catch all possible problems.

Let's see some examples that illustrates the claims above.

* Rust prevents unnitialized memory from being accessed.
```rust,editable,mdbook-runnable
fn main() {
let x;
println!("x = {}", x);
}
```

* Rust prevents accidental modification by making variables immutable by default.
```rust,editable,mdbook-runnable
fn main() {
let x = 0_i32;
x = 42;
}
```

* Rust’s type safety requirements prevent comparisons between types. The code below won't compile.

```rust,editable,mdbook-runnable
fn main() {
   let a: i32 = 10;
   let b: u16 = 100;
  
   if a < b {
     println!("Ten is less than one hundred.");
   }
 }
```

* Rust's type safety prevents unintended type promotions and conversions that can occur in C

```rust,editable,mdbook-runnable
fn main() {
    let counter: i32 = 42;
    let temperature: f64 = x as f64; // Explicit type casting
    
    // Attempting to perform implicit type conversion would result in a compile-time error.
    // let z: f64 = x; // Error: cannot assign `i32` to `f64`
}
```
