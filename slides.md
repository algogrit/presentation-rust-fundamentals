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

- Immutable by default (`mut` keyword for mutability)
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

Code
https://github.com/algogrit/presentation-rust-fundamentals

Slides
https://rust-fundamentals.slides.algogrit.com
