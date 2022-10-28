[Why would a Python programmer learn Rust when there are no jobs in it](https://youtu.be/IYLf8lUqR40)


General talk that has a misleading title. Nearly no mentions to usecases and so, it is more about of an introduction to the Rust environment.

## Python

*Duck typing*. Interesting characteristic that I did not know the name of, good explanation in [Wikipedia](https://es.wikipedia.org/wiki/Duck_typing)

Multithreading is not as useful in Python because of the increasing interpreter log.

## Rust

### Design decisions

- Created in Mozilla with practical purposes in mind, to replace C and C++ codebase.
- Zero abstraction overhead
- Multithread support embedded into type system, such that unsafe code will not compile
- Immutable variables by default

In general, one *fights* with the compiler until it compiles, but when it does so, it rarely fails.

### Tools and features

Some tools:

- Rustup: To get development tools
- Cargo: To manage dependencies

The compiler is **very** slow, but has amazing error messages.

*Error Handling* does not work as in Python. In Rust a returned enum type is used to manage the handling, with values like: OK, etc.

There is no *Object Oriented* as in Python (no inheritance), may seem as it reinvents the wheel, but has very little downsides.

The *Borrow Checker* behaves really good with multithreading, and sometimes seems at it behaves as garbage collector, but in real time. In short, the compiler imposes best practices.

It can be embedded into Python for better performance.

Its main disadvantages are:

- Not as easy to learn as Python
- Cargo, by default, requires downloading packages from the internet

In general, Rust is a very promising language for its age.
