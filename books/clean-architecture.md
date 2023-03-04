Clean Architecture - Robert C. Martin

## Foreword

Software is fractal, as its foundation blocks are software themselves.


# Introduction

## What is Design and Architecture?

Design and architecture are the same, but looked from different perspectives. People talk about design and architecture from a low-level and high-level perspective, respectively.

A good architecture is that one that minimises human resources to be built and mantained.

It is a common mistake to start a project with a bad architecture (because it is overlooked) with intent to improve it afterwards, but this is rarely possible due to pressure and compatibility.

One can proof that developing a good architecture is faster in both the short and the long run.

## A tale of two values

Software has to values: behaviour and structure.

- Behaviour: To make programs under some requirements
- Structure: To make programs easy to change

It is commonly thought that behaviour is more important than structure, but in reality is the opposite, as a program is of no use if it is impractical to be adapted to new requirements.

It is the job of the Software Architect to ensure that structure is valued over behaviour, as by doing so, future changes will be applied much more easily.

## Paradigm Overview

Each paradigm restricts the programmer in a certain way, to prevent bad practices. Some paradigms:

- Structured Programming: Restricts *direct transfer of control*. No gotos, only use of conditionals or loops to control the program flow
- Object-Oriented Programming: Restricts *indirect transfer of control*. Reduces functionality on function pointers, improving use of polymorphism.
- Functional programming: Restricts *assigment*. Forces immutability of symbols

## Structured Programming

