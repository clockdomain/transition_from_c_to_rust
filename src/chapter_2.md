# Rust's Memory Safety

The three pillars of Rust's memory safety are ownership, reference and borrowing and lifetimes.

## Ownership
Rust's ownership model is an important concept for understanding how memory is managed in the language.

* Each value has a single owner.
* There can only be one owner at a time.
* When the owner goes out of scope the value is dropped.

```rust,editable,mdbook-runnable
struct Buffer {
  pub data : [i32; 3],
}

pub fn main() {
// Variable buffer owns the array.
let buffer =  Buffer { data : [1, 2, 3] };
// Ownership moves by assigment
let buffer1 = buffer;
println!("first element of buffer is {}" , buffer.data[0]);
// Or via function call
let checksum = calc_checksum(buffer1); // Ownership of the buffer array is moved to the calc_checksum function
// Attempting to use buffer here will result in a compilation error
println!("first element of buffer is {}" , buffer1.data[0]);
}
fn calc_checksum(a_buffer : Buffer) {
  // The ownership of a_buffer is transferred to this function
  // When the function returns, a_buffer will be dropped and deallocated
}
```
## Borrowing

Instead of transferring ownership, Rust allows variables to borrow *references* to values. Borrowing can be either mutable or immutable and allows multiple parts of code to access the data without transferring ownership.

### References are always valid

References are non-nullable by design. This means that a reference must always point to a value, and there's no concept of a null reference. This design choice eliminates null pointer exceptions.

They are used to borrow values rather than owning them, allowing multiple parts of the code to access data without transferring ownership.

### References can be mutable or immutable.

References can be either mutable (&mut T) or immutable (&T), and their mutability determines whether you can modify the underlying value.

### What are reference lifetimes?

* References can't outlive the owner.

* Reference lifetimes determine how long a reference or pointer to a data object remains valid and usable in a program.

References have lifetimes associated with them to ensure that they do not outlive the data they point to, preventing issues like dangling references.

```rust,editable,mdbook-runnable
fn main() {
    let r; 
    {
        let x = 42;
        r = &x; // Creating a reference to the local variable x
    } // x goes out of scope here and is deallocated
    
    println!("r: {}", r); // Attempting to use the reference r
}
```


## The Rust Advantage

Here's a comparison table that highlights the differences between Garbage Collection (GC), manual memory management in C++, and Rust's ownership model:

| Feature/Aspect               | Garbage Collection          | Manual Memory Management in C++ | Rust Ownership Model           |
|------------------------------|-----------------------------|----------------------------------|--------------------------------|
| **Memory Management**        | Automatic (background)      | Manual (programmer-controlled)  | Automatic (compiler-enforced)   |
| **Memory Cleanup**           | Automatic (GC collects and reclaims memory) | Manual (programmer must explicitly deallocate) | Automatic (when ownership goes out of scope) |
| **Runtime Overhead**         | Overhead for GC activities (e.g., tracing, mark-and-sweep) | Minimal runtime overhead | Minimal runtime overhead |
| **Explicit Deallocation**    | Not required, but manual memory management is limited | Required (programmer is responsible for freeing memory) | Not required (ownership system manages deallocation) |
| **Memory Safety**            | Reduces manual errors (e.g., memory leaks) but may introduce pauses (GC pauses) | Prone to errors (e.g., dangling pointers, memory leaks) | Strong memory safety, prevents common programming errors |
| **Concurrency and Parallelism** | Can introduce challenges due to unpredictable GC pauses | Potential for data races and synchronization issues | Safe parallelism through ownership and borrowing rules |
| **Ecosystem Maturity**       | Mature and well-established in languages like Java, C#, Python | Mature and widely used in C and C++ | Maturing but gaining popularity in Rust |
| **Predictable Performance**   | Less predictable due to GC pauses | More predictable but requires manual optimization | Predictable and safe performance |
| **Error Classes**            | Eliminates some manual memory management errors but introduces others (e.g., reference cycles) | Prone to various memory-related errors (e.g., segmentation faults) | Prevents many common memory-related errors |
| **Development Speed**        | Can accelerate development by managing memory for you | Can slow development due to manual memory management | Balance between speed and safety |
| **Learning Curve**           | Easier for programmers new to low-level memory management | Steeper learning curve for manual memory management | Moderate learning curve due to ownership concepts |
| **Resource Utilization**     | May have higher memory usage due to unpredictable GC behavior | Efficient resource utilization but requires careful management | Efficient resource utilization with predictable behavior |
| **Convenience vs. Control**  | Offers convenience but less control over memory | Offers fine-grained control but is error-prone | Offers control with safety and reasonable convenience |

In summary, Garbage Collection automates memory management but can introduce latency and may not be suitable for all use cases. Manual memory management in C++ provides full control but requires careful management to avoid errors. Rust's ownership model combines control and safety, making it a compelling choice for systems programming and other domains where memory safety and performance are critical.

