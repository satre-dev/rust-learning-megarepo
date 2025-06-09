# Fundamental Types

## Static Typing
Rust is a statically typed language, which means that the type of a variable is known at compile time. This is in contrast to dynamically typed languages like Python, where the type of a variable is determined at runtime.
This requires more planning and attention to detail, but it also provides several benefits, such as improved performance and better error detection.
Given the types you've defined, Rust's type system ensures that operations are performed correctly and safely. For example, if you have a variable of type `i32`, you can only perform integer arithmetic on it, and Rust will prevent you from accidentally using it as a floating-point number.

## Type Inference

The Rust compiler can also do some work for you with `type inference`. For example, you could spell out every time in a function, like this:
```Rust
fn build_vector() -> Vec<i16> {
    let mut v: Vec<i16> = Vec::<i16>::new();
    v.push(10i16);
    v.push(20i16);
    v
}
```
But this is cumbersome. The same functionality can be written using `type inference` as below:
```Rust
fn build_vector() -> Vec<i16> {
    let mut v = Vec::new();
    v.push(10);
    v.push(20);
    v
}
```
Here we can see the function signature does the heavy lifting for us, and the compile can then infer that the following call to `Vec::new()` should create one of type `i16`.
We are also able to push integer literals 10 and 20 to the vector without having to cast these as i16 above for the same reason.

## Fixed-Width Numeric Types

Chosen to match the types that almost all modern processors implement directly in hardware.

Fixed-width numeric types are used to represent integers with a fixed number of bits. They are useful when you need to represent a specific range of values, such as a byte or a word. Rust provides several fixed-width numeric types, including `i8`, `u8`, `i16`, `u16`, `i32`, `u32`, `f32`, `i64`, `u64`, `f64`, `i128`, `u128`, and `isize`, `usize`.

A `machine word` is the natural size of a processor's data bus, which is typically 32 or 64 bits. Rust provides the `usize` type, which is the same size as a machine word on the target platform. This type is useful for indexing arrays and other data structures, as it can represent the maximum size of an array on the target platform.

### Integer Types

Rust provides several integer types, including `i8`, `u8`, `i16`, `u16`, `i32`, `u32`, `i64`, `u64`, `i128`, `u128`, and `isize`, `usize`.

Table of Rust's *unsigned* integer types
| Type | Range |
| - | - |
| u8 | 0 to 255 |
| u16 | 0 to 65535 |
| u32 | 0 to 4294967295 |
| u64 | 0 to 18446744073709551615 |
| u128 | 0 to 340282366920938463463374607431768211455 |
| usize | 0 to 18446744073709551615 |

Rust's *signed* integer types use the two's complement representation, using the same bit pattern as the corresponding unsigned type to cover a range of positive and negative values.
Table of Rust's *signed* integer types
| Type | Range |
| - | - |
| i8 | -128 to 127 |
| i16 | -32768 to 32767 |
| i32 | -2147483648 to 2147483647 |
| i64 | -9223372036854775808 to 9223372036854775807 |
| i128 | -170141183460469231731687303715884105728 to 170141183460469231731687303715884105727 |
| isize | Either -9223372036854775808 to 9223372036854775807  or 0 to 18446744073709551615 |

## Characters

Unlike C and C++, Rust treats characters as distinct from the numeric types: a `char` is not a `u8`, nor is it a `u32` (though it is 32 bits long).

Character types represent a single Unicode character, which can be any character from the Unicode standard. An important thing to remember is that Rust uses the char type for single characters in isolation, but uses the UTF-8 encoding for strings and streams of text. So, a `String` represents its text as a sequence of UTF-8 bytes, not as an array of characters.


## Pointer Types

Rust has several types that represent memory addresses.

A big distinction between Rust and most languages with garbage collection is that, for example, in something like Java if a class contains a field which is itself another object, then the class instance of that field in memory is actually a reference to another seperately created instance of the object. The parent object who owns the field does not *contain* it.

Rust is different. The language is designed to keep allocations to a minimum. Values nest by default. The value `((0, 0), (1440,900))` is stored in four adjacent integers. That is to say the memory is contiguous. No pointers, no heap allocation.

This is good for memory efficiency, but as a consequence, when a Rust program needs values to point to other values, it must use pointer types explicitly. The good news is that the pointer types used in safe Rust are constrained to eliminate undefined behaviour, so pointers are much easier to use correctly in Rust than C++.

There are three main pointer types to discuss here: references, boxes, and unsafe pointers.

### References
