The Rust Programming Language, 2nd Edition

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

*Shadowing* is the effect of giving a variable the same same as a prevoius one. The effect will be that the new one will replace the old.

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

For readability, the *_* sign can be used, for example `1_000_000`, which is the same as `1000000`.

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

The keywords `break` and `continue` apply to the innermost loop. They can refer to previous ones by using *loop labels*. For example:

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
