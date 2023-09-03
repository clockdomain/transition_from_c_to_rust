# Why should C programmers consider Rust?
C is too permissive. It is easy to make mistakes 

## Type usafety  
C's Weak Type System is a liability. 
* Implicit conversions and promotions.
* Silent overflow and underflow.
* Permissive type reinterpretation/casting.
* Function signature mismatch.


### Example
Implicit conversion of signed to unsigned integer caused the  FreeBSD getpeername bug.
``` c
#define KSIZE (1024)
char kbuf[KSIZE]
// memcpy uses unsigned size_t for size.
void* memcpy(void *dest, void* src, size_t n);

// copy_from_kernel takes signed int for size.
int copy_from_kernel(void* dest, void* src, int max_len) {
  int len = KSIZE < max_len ? KSIZE : max_len;
  memcpy(user, dest, kbuf, len);
  return len; 
}
```

## Memory unsafety 
* Null pointer de-references.
* No bounds checking => buffer overflows, out of bound accesses.
Manual memory management 
* Use after free (dangling pointers)
* Double free

### Example

OpenSSH Double Free vulnerability  allowed remote attackers to execute arbitrary code on the affected system. The vulnerability was tracked as CVE-2002-0639

```c
#include <stdlib.h>

int main() {
    char *buffer = (char *)malloc(10);

    if (buffer == NULL) {
        return 1; // Allocation failed
    }

    free(buffer); // First free

    // Some code that inadvertently tries to free the same memory again
    char *other_buffer = buffer;
    free(other_buffer); // Second free (double-free vulnerability)

    return 0;
}
``````
