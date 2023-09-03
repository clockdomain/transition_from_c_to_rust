# Unsafe Rust

Rust’s strict ownership and borrowing rules make it possible to write safe code, but in certain situations, mainly when working with low-level system programming, unsafe is necessary.

Rust's

Here are some common ways embedded developers might use unsafe code:

* Accessing Hardware Registers: Embedded systems often require direct manipulation of memory mapped hardware, meaning they are located at specific memory addresses and you need pointers to access that memory directly. Dereferencing a pointer is an unsafe operation as it cannot be checked by the compiler.

One of the most fundamental reasons to use unsafe is to deal with Rust’s raw pointer types: *const T and *mut T.

```rust,editable,mdbook-runnable

use core::ptr;
fn main() {
let gpioa = 0x4002_0000 as *mut u32; // Assume GPIOA register starts at memory address 0x4002_0000
  unsafe {
      // Reading from a hardware register
      let value = *gpioa; // Dereferencing the pointer to read from GPIOA

      // Writing to a hardware register
      *gpioa = 0x12345678; // Dereferencing the pointer to write to GPIOA
  }
}
```

* When working with Foreign Function Interface (FFI) calls.

