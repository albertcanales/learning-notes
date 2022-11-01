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


## High-Level View of Translation

### Understanding the Input

#### Syntax

The *Grammar* is the set of rules that are obeyed by all valid programs. Thus, it is the one defining the concept of valid.

The task of the Parser and the Scanner is to check efficiently if a given program obeys the language's grammar.

A simplified process for syntax validation would be (applied to English as an example):

1. Check if all words exists
2. Replace its word by its more abstract type (syntactic category)
3. Test if the abstracted version of the phrase matches the patterns described in the defined rules

The last phase is done using *derivation*, that is, combining simple patters to match more complex phrases. Each step in the process of derivation consists in decomposing by one single rule. If the process completes (it arrives to a decomposition matchiing the input pattern), it is valid.

The two phases of checking syntax can be summarised in:

1. Scanning: Abstracting the syntactic category of each word
2. Parsing: Finding a derivation using gramatical rules

#### Meaning

Understanding meaning goes beyond checking a well-formed expression, as it requires understanding of context. Some examples would be undefined variables, type errors, etc. as the correctness of this does not depend on grammar, but on the context given by the previous sentences.

