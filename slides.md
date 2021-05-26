layout: true

.signature[@algogrit]

---

class: center, middle

# Rust Fundamentals

Gaurav Agarwal

---
class: center, middle

![The Rust Book](assets/images/rust-book.jpg)

.content-credits[https://doc.rust-lang.org/book/title-page.html]

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

- functions, variables (let), modules & filenames, methods

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
- Types classification:
  - Scalar
  - Compound

---
class: center, middle

#### Scalar

integers, floats, boolean, character

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

tuple, array

---
class: center, middle

Compound types can group multiple values into one type.

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

## Compound Data Type: `struct`

---
class: center, middle

Created using `struct` keyword

---

- Like ruby, if variable name and field names are same, the field name can be avoided during initialization
- Use `struct update syntax`, to create a new struct using some fields of old struct
- Structs can also be tuples
- You can also create unit structs

---
class: center, middle

### Ownership of Struct Data

---
class: center, middle

### Debugging & Printing structs

---

- Printing requires implementing `Display` trait
- Using the `Debug` trait, and deriving it:

```rust
#[derive(Debug)]
```

---

- Can print using, `{:?}`

- Or use: `{:#?}` for pretty printing

---

### Methods for `struct`

- Defined using the `impl` block

- Should take `self` or `&self` reference as first arg

- Methods can take more than one arg

---

### Associated functions

- `impl` blocks can define other associated functions as well

- Eg. `String::from`

- Typical usage is for creating constructors (`new` is the convention)

---
class: center, middle

You can define multiple `impl` blocks

---
class: center, middle

## Enums

---
class: center, middle

Similar to algebraic data types in ML family of languages

---

- Defined using `enum` keyword

Eg:

```rust
enum Action {
  Jump(i32)
  Stop
  Turn(Direction)
  Walk
}
```

- Methods can be defined on `enums` as well, similar to `struct` using `impl`

---
class: center, middle

### Common Enums

---
class: center, middle

#### `Option<T>` (`std::option::Option`)

---

- Two variants: `Some<T>` and `None`

- Rust also doesn't have `nil` or `null`!

- Instead it relies on the `Option` enum in the standard library

---
class: center, middle

#### Result<T, Err>

---
class: center, middle

### Pattern matching

---

- Use `match`

- `match` is exhaustive

- You can use `_` (underscore), as a catch all "match arm"

- You can use `()` as a do nothing, `noop`!

---

- `if let` is a more concise way to perform the same things as above!

- can also have `else` & `else if let` blocks

---
class: center, middle

## Project Management

---

Rust has a number of features that allow you to manage your code’s organization, including which details are exposed, which details are private, and what names are in each scope in your programs.

These features, sometimes collectively referred to as the module system, include:

- Packages: A Cargo feature that lets you build, test, and share crates
- Crates: A tree of modules that produces a library or executable
- Modules and `use`: Let you control the organization, scope, and privacy of paths
- Paths: A way of naming an item, such as a `struct`, function, or module

---
class: center, middle

### Packages / Crates

---

- A package can have multiple binary crates by placing files in the src/bin directory: each file will be a separate binary crate.

- A crate will group related functionality together in a scope so the functionality is easy to share between multiple projects.

- If a package contains src/main.rs and src/lib.rs, it has two crates: a library and a binary, both with the same name as the package.

---

- Binary Packages: `src/main.rs`

- Library Packages: `src/lib.rs`

---
class: center, middle

### Modules

---
class: center, middle

Modules let us organize code within a crate into groups for readability and easy reuse. Modules also control the privacy of items, which is whether an item can be used by outside code (public) or is an internal implementation detail and not available for outside use (private).

Related keywords: `mod`, `pub`, `use`, `as`

---

- `mod`: used to define a module

  - Alternatively, can create a file with same name or `dir/mod.rs` file

- `pub`: Used to make modules & selected functionality public.

  - Related: `in`

---

- `use`: For importing a path to the current module

  - Related: `crate`, `super`, `self`, `extern`
  - Also has a multi-import syntax as well

- `as`: Is used to aliasing an import

---
class: center, middle

## Collections in std library

---

- Have different behavior cause of Heap allocations

- A rust programmer will need to understand ownership better before writing any meaningful applications

---

Rust’s collections (`std::collections`) can be grouped into four major categories:

- Sequences: `Vec`, `VecDeque`, `LinkedList`
- Maps: `HashMap`, `BTreeMap`
- Sets: `HashSet`, `BTreeSet`
- Misc: `BinaryHeap`

.content-credits[https://doc.rust-lang.org/std/collections/index.html]

---

We’ll discuss three collections that are used very often in Rust programs:

- A vector allows you to store a variable number of values next to each other.
- A string is a collection of characters. We’ve mentioned the String type previously.
- A hash map allows you to associate a value with a particular key. It’s a particular implementation of the more general data structure called a map.

---
class: center, middle

### Vectors

---

A contiguous growable array type, written as `Vec<T>` and pronounced ‘vector’.

- Creating a vector:
  - `Vec::new()`
  - `vec!()` macro

- Updating a vector:
  - `v.push(5)`

- Dropping a Vector Drops Its Elements

- Reading Elements of Vectors
  - Indexing: `v[n]`
  - get method: `v.get(n)`

- Iterating over the Values in a Vector
  - `for _ in _`

- Using an Enum to Store Multiple Types

---
class: center, middle

### String

A String is a wrapper over a `Vec<u8>`.

---
class: center, middle

Trying with “नमस्ते” or "Здравствуйте"!

---
class: center, middle

Slicing Strings: What are we slicing exactly?

---
class: center, middle

bytes!

---
class: center, middle

Iterating Over Strings

---
class: center, middle

### HashMap

---

- `use std::collections::HashMap;`

- `HashMap::new();`

- Inserting into the HashMap
  - `insert` method

- Another way of constructing a hash map is by using iterators and the collect method on a vector of tuples, where each tuple consists of a key and its value.

- Accessing Values in a Hash Map
  - `get` method

- Updating a Hash Map
  - Overwriting a Value: `insert` methods
  - Inserting if there is no value: `entry` & `or_insert` methods
  - Updating values via mutable references

---
class: center, middle

#### Hash Maps and Ownership

---
class: center, middle

For types that implement the `Copy` trait, like `i32`, the values are copied into the hash map. For owned values like `String`, the values will be moved and the hash map will be the owner of those values.

---
class: center, middle

### Hashing Functions

HashMap uses a hashing function called SipHash that can provide resistance to Denial of Service (DoS) attacks involving hash tables.

.content-credits[https://en.wikipedia.org/wiki/SipHash]

---
class: center, middle

## Errors & panics

---

Two options in Rust:

- `panic!`
  - No recovery

- `Result<T, E>`
  - Variants: `Ok(T)`, `Err(E)`

---
class: center, middle

### Panic

---
class: center, middle

Can get backtrace using: `RUST_BACKTRACE=1` env variable.

---

#### Turn off unwinding

```toml
[profile.release]
panic = 'abort'
```

---
class: center, middle

### Result<T, E>

---

Use `match`

- match on different errors using `kind` method

---

Use methods like

- `unwrap_or_else` <- allows err handling
- `unwrap` <- panic
- `expect` <- panic with custom message

---

Propagating errors

- `?` <- can return even from main
- Return type: `Result<(), Box<dyn Error>>`

---
class: center, middle

## Generics

---
class: center, middle

Generics are used for static dispatching.

---

You can define generic:

- Functions

- Struct

- Enums

- Methods

---
class: center, middle

Compiler does "Monomorphization"!

---
class: center, middle

Generics have zero-cost.

---
class: center, middle

## Traits

---

Allow us to define constraints on generic features.

- Defined using `trait` keyword

- Can contain default method definitions too!

---
class: center, middle

### Coherence (Orphan Rule)

---
class: center, middle

Trait can be implemented by structs in different modules too!

---

You can either

- implement a trait from same module for an external type

- implement an external trait for type from same module

---
class: center, middle

### Advanced Traits

---

- Traits can be used as restrictions for params using `impl`
  - This is a syntactic sugar

- Can specify multiple trait bounds using `+`

- `where` clause, an alternative to improve readability

- Using trait bounds you can also have a "selective" generic implementation

- We will discuss trait objects later

---
class: center, middle

## Lifetimes

---

Rust doesn't come with a GC.

- The compiler does come with a "Borrow Checker".

- Some code possible in other languages isn't possible in Rust

- The compiler needs to ensure that there aren't any dangling pointers in your code.

- It does it by keeping track of the lifetime of each variable & it's reference.

- In some cases, the compiler isn't smart enough to keep track of the lifetimes.

- In such cases, a programmer can provide hints: lifetime annotations.

---
class: center, middle

### Lifetime annotations

---
class: center, middle

Similar to generics.

---

- generics are for types

- lifetimes are for scopes

---

- Eg: `'a`

- You can define them for functions, structs & methods when working with references

- When passing multiple references with same lifetime annotation
  - the compile uses the lifetime of the reference which is shortest

- The compiler user "lifetime ellision rules" to auto-add lifetime annotations

- There is also `'static` lifetime

As rust programmers, we have an added responsibility of thinking of lifetimes while writing code.

---
class: center, middle

### Lifetime ellision rules

---

There are two kinds to lifetimes:

- input (parameter)

- output (return)

---

Some code in rust, using references, can be compiled without lifetime annotations, due to the following rules:

- Each param that is a reference gets its own lifetime parameter

- If there is only one input lifetime param, then that lifetime is assigned to all output lifetime parameters

- For methods: If there is `&self` or `&mut self` input param then that lifetime is assigned to all the outputs

---
class: center, middle

## Testing

---
class: center, middle

"Program testing can be a very effective way to show the presence of bugs, but it is hopelessly inadequate for showing their absence."

.content-credits[1972 essay “The Humble Programmer,” Edsger W. Dijkstra]

---

- Unit tests need to be defined in the same file as the Source code.

- Integration tests can be defined in the `/tests` directory.

- Rust also has doc tests.

---
class: center, middle

### Unit tests

---

- Need to have `#[cfg(test)]` before module

- Need to have `#[test]` before test case

- Can ignore test with `#[ignore]`

---

- Assertions: all macros!
  - assert_eq!
  - assert!

- Tests can fail, due to panics!

- Panics can be tested using `#[should_panic]`

- Can import `private` & `public` code from `super` module using `use super::*`

---
class: center, middle

### Integration tests

---
class: center, middle

Can test only public API

---
class: center, middle

## Doc tests

---
class: center, middle

Defined using `///`

---
class: center, middle

### Running tests with `cargo test`

---

- Parallel tests by default: `--test-threads=1`

- Output of passing test: `--show-output`

- Run subset by name: `cargo test <subset-name>`

- Run ignored tests: `--ignored`

---
class: center, middle

## Closures

---
class: center, middle

Rust "borrows" a lot of concepts from functional programming languages.

---

- Closures use ruby like syntax.

- In addition, can have types of input & return specified
  - Otherwise, inferred once

- Closures can be stored & passed around, using `Fn` traits!

---

- `Fn` traits are inferred by the compiler automatically

  - `FnOnce`: Moves & the closure can be invoked just once.

  - `FnMut`: Borrows mutably

  - `Fn`: Borrows immutably

---
class: center, middle

`move` keyword can be used explicitly

---
class: center, middle

## Iterators

---
class: center, middle

Custom types can implement `std::iter::Iterator` trait.

---

- Lazy evaluation by default

- Force it by calling `collect` result!

```rust
pub trait Iterator {
  type Item;
  fn Next(&mut self) -> Option<Self::Item>
}
```

- `Item` & `Self::Item` are associated types

- Methods that call `next` are "consuming adaptors"
  - eg: `sum`

- Other methods are known as "iterator adaptors"
  - eg: `map`, `filter`

---
class: center, middle

### Creating iterators

---

- `iter` method: immutable references to elements

- `into_iter` method: takes ownership of whole collection

- `iter_mut` method: mutable reference to elements

---
class: center, middle

## Performance of Iterators

---
class: center, middle

Iterators are Rust's "zero-cost abstraction"

---
class: center, middle

> In general, C++ implementations obey the zero-overhead principle: What you don't use, you don't pay for. And further: What you do use, you couldn't hand code any better.

.content-credits[As defined by Bjarne Stroustrup, creator of C++]

---

class: center, middle

Code
https://github.com/AgarwalConsulting/Rust-Training

Slides
https://rust-fundamentals.slides.algogrit.com
