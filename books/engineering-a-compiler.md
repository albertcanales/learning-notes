Engineering a Compiler - Keith D. Cooper & Linda Torczon

# Overview of Compilation

## Introduction

Given a source program, a compiler generates a target program.

Compilers source programs are written in a programming language that, unlike a natural language, is structured and well-defined.

A compiler is called a *source-to-source translator* when the target program is written also in a programming language.

An interpreter, unlike a compiler, recieves a source code and generates the results of the execution. This makes some parts of compiler and interpreter construction very similar (identifying errors, allocating memory...), but others really different.

## Why Study Compiler Construction

- Absurdly complex and resource-rich task
- Heavy use of algorithms

## Fundamental Principles of Compilation

- Must preserve the meaning of the program being compiled
- Must improve the input program in some distintive way

## Compiler Structure

A compiler can be simplified into two blocks:

- Front End: Understanding the source program
- Back End: Mapping functionality to the target machine

Within each block, the *Intermediate Representation* (IR) is passed, which is the structured representation that represents the program in a specific time of the compilation process.

The IR passed between independent phases of compilation (such as Frontend and Backend) is called the *Definitive IR*. Many passes of the IR through phase may occur before the Definive IR is obtained.

This separation of phases provides also some modularity for target machines or source languages.

More commonly, the compilation process is divided into three phases:

- Front End
- Optimiser: IR-to-IR transformer
- Back End

Each independent phase can be considered a compiler itself due to the definition above.

The Optimiser can not produce optimal code, but it tries to get close (more on this later).

