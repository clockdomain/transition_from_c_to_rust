# Chapter 1

Rust *makes it look easy* by enforcing a strict set of rules at compile time.

* Strong emphasis on safety and correctness from the ground up.
* Catches most common programming errors during compile time.
* Mitigates or eliminates undefined behavior.
* Naturally promotes reliable software development.
* Does not rely on static analysis tools, which generates many false positives and 
  may not catch all possible problems.


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

* In Rust, you cannot have a variable that is "null" or "undefined." This prevents null access from the start.

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
    let x: i32 = 42;
    let y: f64 = x as f64; // Explicit type casting
    
    // Attempting to perform implicit type conversion would result in a compile-time error.
    // let z: f64 = x; // Error: cannot assign `i32` to `f64`
}
```
