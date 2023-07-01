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

	const PI: f64 = 3.141592;

### Shadowing

*Shadowing* is the effect of giving a variable the same same as a prevoius one. The effect will be that the new one will replace the old.

It differs from using `mut` because, at is is creating a new variable, it also allows changing the type and takes scopes into account. For the former, consider the examples:

	let mut x = 5;
    {
        x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }
    println!("The value of x is: {x}");

and:

	let x = 5;
    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }
    println!("The value of x is: {x}");

