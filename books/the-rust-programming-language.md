<!-- FRONT
title = "The Rust Programming Language (2nd Ed.)"
description = "Steve Klabnik, Carol Nichols, & Chris Krycho"
-->

Latest release available here: <https://doc.rust-lang.org/stable/book/>.

# Getting Started

## Hello Cargo!

To sum up:

- `cargo new PROJECT` creates a new project
- `cargo check` builds without producing executables (much faster)
- `cargo build` compiles the developer binaries (no optimisations)
- `cargo build --release` compiles the release binaries (with optimisations)
- `cargo run` runs the project

In addition (not in the book):

- `cargo fmt` formats the project code

# Programming a Guessing Game

> The chapter itself is really well summarized, I recommend going through it entirely.

All the creates are downloaded from: <https://crates.io/>.

The command `cargo doc --open` will automally generate a local documentation for the project's dependencies.

# Common Programming Concepts

## Variables and Mutability

### Varibles

Variables are immutable by default, this behaviour can be changed with `mut`. For example:

let mut x = 5;

### Constants

Very similar to other languages. Must have its type annotated. For example:

```rust
const PI: f64 = 3.141592;
```

### Shadowing

_Shadowing_ is the effect of giving a variable the same same as a prevoius one. The effect will be that the new one will replace the old.

It differs from using `mut` because, at is is creating a new variable, it also allows changing the type and takes scopes into account. For the former, consider the examples:

```rust
let mut x = 5;
{
    x = x * 2;
    println!("The value of x in the inner scope is: {x}");
}
println!("The value of x is: {x}");
```

and:

```rust
let x = 5;
{
    let x = x * 2;
    println!("The value of x in the inner scope is: {x}");
}
println!("The value of x is: {x}");
```

## Data Types

### Scalar types

The `arch` type's size depends on the architecture of the machine running the program.

For readability, the \_\_\_ sign can be used, for example `1_000_000`, which is the same as `1000000`.

Some methods are provided to avoid integer overflow [here](https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#integer-overflow).

The default types for integers and floating-point are `i32` and `f64` respectively.

A character occupies 4 bytes (unlike in C). It can store accented values, emojis, etc.

### Compound Types

#### Tuples

A tuple can hold multiple types. Its values can be extrated by index or pattern matching, for example:

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (_, y, _) = tup;
println!("The value of x and y are {tup.0} and {y} respectively");
```

#### Arrays

Fixed size and single type (like in C). Arrays can be initialised as so:

```rust
    let a = [1, 2, 3, 4, 5];    // Clear enough
    let a: [i32; 5] = [1, 2, 3, 4, 5];  // Specifying type and size
    let a = [3; 5];     // Specifying initial value and size
```

Element access on invalid indexes are checked and result in a panic. This prevents invalid memory access.

## Functions

Note the difference between statements and expressions:

- **Statements** are instructions that perform some action and do not return a value.
- **Expressions** evaluate to a resultant value. Let’s look at some examples.

Have a look at this vaid example, which is useful to understand function syntax:

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

Following this principe, this function as is would be valid, but if a semicolor is added after 1, it would not:

```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}
```

The keyword `return` must be used if the expression that is going to be returned is not the last one in the function code.

## Comments

There is only single line comment, using `//`.

## Control Flow

### If expressions

They work as one would expect. No parenthesis for the condition are needed, but braces are compulsory.

Having this and the expression reasoning from before into account, the following code is possible:

```rust
let number = if condition { 5 } else { 6 };
```

### Repetition with Loops

There are three loops: `loop`, `while` and `for`.

#### Loop

Runs indefinetely. Control flow can be managed through `break` and `continue`.

It can become an expression when an expression is placed next to `break`. For example:

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

The keywords `break` and `continue` apply to the innermost loop. They can refer to previous ones by using _loop labels_. For example:

```rust
fn main() {
    let count = 4;
    'first_loop: loop {
        loop {
            if count == 4 {
                break 'first_loop;
            }
        }
    }
    println!("Program ended");
}
```

#### While and For

Really similar to other languages.

# Understanding Ownership

## What is Ownership?

Rust's ownership tackle's this basic C problem:

```c
void fun(int* p) {
    ...
}

int main() {
    int* p = malloc(...);

    f(p)
    // Now, who is responsible of freeing p?
}
```

To clarify, we must free the memory when unused to avoid memory leaks. On the other hand, freeing the memory twice or prematurely will result in an unrecoverable error.

To solve this, some languages make use of a garbage collector, which can greatly damage performance. Rust uses the concept of _ownership_. That means:

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

We have to note that data types with known size (at compile time) can be stored in the stack, and thus do not need the concept of _ownership_.

Based on these rules, there is no _shallow copy_ (i.e. ahving two pointer variables pointing at a same value). Instead we have a _move_. Deep copying is still allowed.

### Copy and Drop traits

How does Rust know that a data type have fixed size (and thus can be stored in the stack)? They implement the _Copy_ trait. This means that on variables will not be moved, but deep copied.

To indicate that moving must be used, we implement the _Drop_ trait. A data type cannot implement _Copy_ if any of its parts implements _Drop_.

For example the tuple `(i32, i32)` implements _Copy_, but `(i32, String)` does not.

### Ownership, functions and return values

Passing a variable through a function's parameters gives the called function ownership over the variable. Similarly, returning a value gives the caller ownership over that value.

Suppose this simple case:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = func(s1);
    // Here we would like ownership over s1 and s2
    // But we only have ownership over s2
}

fn func(s: String) {
    String::from("bye");
}
```

To solve this one might return a tuple with the parameter. This may work but it is really inconvinient. Here we introduce the following chapter.

## References and Borrowing

A reference allows to pass a value without giving it's ownership. It differs from a pointer in the sense that it is guarantieed to point to a valid value and known type.

_Borrowing_ is the act of creating a reference.

You cannot modify a reference unless you explicitly make it mutable.

It is also forbidden to have more than one mutable reference to the same value in use. This prevents data races at compile time. Having a mutable reference among some immutable is also forbidden.

Rust can also warn and prevent dangling references such as:

```rust
fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger!
```
