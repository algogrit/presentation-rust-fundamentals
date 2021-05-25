layout: true

.signature[@algogrit]

---

class: center, middle

# Rust Fundamentals

Gaurav Agarwal

---
class: center, middle

Rust is a compiled language. Every program needs to be compiled before it can be run!

---

Looking at `scratchpad`

---

## Running & Building

- `rustc`

  - For compiling, a `.rs` to a binary

- `cargo`

  - For managing, rust projects.
    - `cargo build` for compiling
    - `cargo run` for compiling and running (during development)
    - `cargo test` for testing
    - ...

---

class: center, middle

## Naming Conventions

---

- functions, variables (let), modules & filenames

  `snake_case`

- constants

  `SCREAMING_SNAKE_CASE`

- Enums, Structs, Traits

  `PascalCase` or `UpperCamelCase`

---
class: center, middle

## Variables & Types

---
class: center, middle

### Variables

---

- Immutable by default
- `mut` keyword for mutability
- Strongly typed
- Type can be inferred
- Allows shadowing!
- Enum for Errors (`Result{ Ok<T>, Err }`)
  - `match` keyword
- Types classification:
  - Scalar - integers, floating-point numbers, Booleans, and characters
  - Compound
    - Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

---
class: center, middle

#### Scalar

---

##### Integers

- `i8` to `i128`;
- `u8` to `u128`
- `isize` & `usize` (`arch` dependent)
- Can use `_` as decimal separator
- Rust defaults to `i32`

---

##### Floats

- `f32` & `f64`
  - `f64` is default
- `IEEE-754` standard

---

##### Boolean

- `bool`

---

##### Character

- `char`
  - four bytes in size and represents a Unicode Scalar Value
- a "character" isn’t really a concept in Unicode, so your human intuition for what a “character” is may not match up with what a char is in Rust

---
class: center, middle

#### Compound

---

##### Tuple Type

- A tuple is a general way of grouping together a number of values with a variety of types into one compound type.
- Tuples have a fixed length: once declared, they cannot grow or shrink in size.

- Eg:

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
```

---

- To get the individual values out of a tuple, we can use pattern matching to destructure a tuple value

```rust
let (x, y, z) = tup;
```

---

- In addition to destructuring through pattern matching, we can access a tuple element directly by using a period (.) followed by the index of the value we want to access.

```rust
let six_point_four = x.1;
```

- As with most programming languages, the first index in a tuple is 0.

---

##### Array Type

- Every element of an array must have the same type.
- arrays in Rust have a fixed length

- Eg:

```rust
let a = [1, 2, 3, 4, 5];
```

---

- Arrays are useful when you want your data allocated on the stack rather than the heap

- A vector is a similar collection type provided by the standard library that is allowed to grow or shrink in size.

---

- Eg:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

- you can specify the initial value, followed by a semicolon, and then the length of the array in square brackets

```rust
let a = [3; 5];
```

The array named a will contain 5 elements that will all be set to the value 3 initially.

---
class: center, middle

What happens if you try to access an element of an array that is past the end of the array?

---
class: center, middle

### Constants

---

- declared using the `const` keyword
- constants cannot be shadowed!

---
class: center, middle

## Functions

---

### Passing arguments

```rust
fn main() {
  add(10, 20);
}

fn add(x: i32, y: i32) {
  println!("{}", x + y);
}
```

---

### Returning values

```rust
fn five() {
  return 5
}
```

- `return` is optional

---
class: center, middle

## Control Flow

---

### `if`

- Similar to `if ... else` in many other languages
- Can be used for assignment

---
class: center, middle

### Repetition with loops

---
class: center, middle

Rust has three kinds of loops: `loop`, `while`, and `for`.

---

#### `loop`

- You can `loop` forever in rust
- Control execution using `break` & `continue`
- Usually used as a retry mechanism
- Also useful for assignment as well

---

#### `while`

- Normal

---

#### `for`

- Can only be used as `for <el> in <iter>` format

---
class: center, middle

## Ownership

---
class: center, middle

Ownership is Rust’s most unique feature, and it enables Rust to make memory safety guarantees without needing a garbage collector.

---
class: center, middle

All programs have to manage the way they use a computer’s memory while running. Some languages have garbage collection that constantly looks for no longer used memory as the program runs; in other languages, the programmer must explicitly allocate and free the memory.

---
class: center, middle

Rust uses a third approach: memory is managed through a system of ownership with a set of rules that the compiler checks at compile time. None of the ownership features slow down your program while it’s running.

---
class: center, middle

### What happens when you assign values and pass them around?

---
class: center, middle

### Stack vs Heap

---

#### Stack

- All data stored on the stack must have a known, fixed size.

#### Heap

- Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

---

- Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a place to store new data

- Accessing data in the heap is slower than accessing data on the stack because you have to follow a pointer to get there.

---
class: center, middle

Keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused data on the heap so you don’t run out of space are all problems that ownership addresses.

---
class: center, middle

### So, what happens when you assign values and pass them around?

---

- All primitive types (and types allocated on stack) implement a `Copy` trait
- If a type implements the `Copy` trait, an older variable is still usable after assignment

---
class: center, middle

#### We will discuss traits later...

---
class: center, middle

#### ... let's take a look at `std::string::String`

---

In order to support a mutable, growable piece of data, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents:

- The memory must be requested from the memory allocator at runtime.

- We need a way of returning this memory to the allocator when we’re done using it

---

- `String` is allocated over the Heap
- It has a slightly different behavior when:
  - assigned
  - passed to or returned from a function

---

### Ownership Rules (applies to all variables)

- Each value in Rust has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

---
class: center, middle

Sometimes you do want to share data with other functions

---
class: center, middle

### Borrowing

---
class: center, middle

Borrowing doesn't transfer ownership!

---
class: center, middle

#### Using references

---

- Using `&` and `*`
  - looks similar to pointers in other languages
- Note: In rust, the type info contains `&` instead of `*`

---
class: center, middle

References can be mutable too!

---

- Like variables, references aren't mutable by default
- You can use the `mut` keyword to create mutable references
- Cannot mix immutable variables with mutable references

---
class: center, middle

Mutable references have one big restriction: you can have only one mutable reference to a particular piece of data in a particular scope. This code will fail:

---

This prevents data races.

- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data.

Users of an immutable reference don’t expect the values to suddenly change out from under them!

---

##### Rules of References

- At any given time, you can have either one mutable reference or any number of immutable references.
- References must always be valid.

---
class: center, middle

#### Using slices

---
class: center, middle

Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.

---

Eg. `[start_index..end_index]`

---

Some of the common problems avoided by Rust's ownership and borrowing:

- Double free (Transferring)
- Dangling Pointers (Borrowing)
- Data Races (Single Mutable References)
- Index out of bound (slices)

---

class: center, middle

Code
https://github.com/algogrit/presentation-rust-fundamentals

Slides
https://rust-fundamentals.slides.algogrit.com
