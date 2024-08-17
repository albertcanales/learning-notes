The C Programming Language, Second Edition

> Disclaimer! I read this book being already familiar with C. I only took notes of some more detailed aspects of the language. It has been reordered, so it does not follow the book exactly.

## Keywords for objects

Important difference between definition and declaration of variables:

- Definition: Space assigned to a variable (and maybe value). Just done once.
- Declaration: Assigns a variable name and type to that variable. Can be done multiple times.

A variable can be:

- Internal: Available only in a certain scope (defined in a function)
- External: Available during all the program (defined outside any function)

Some aspects about external objects:

- Use the keyword `extern` to use external variables from other source files.
- All functions are external.
- External objects with the same name reference always to the same object.

The keyword `static` has two meanings depending if an object is:

- External object: Limits the object's scope to that source file (similar to private objects on OOP). Is is a matter of **visibility**
- Internal object: Makes the variable (and its value) persist between function calls, while maintaining the limited scope. It is a matter of **duration**.

All static and external variables are assigned to `0`.

The keyword `register` suggests the compiler to place the variable in a register, but it can ignore the suggestion. We cannot take its pointer.

## Chars and strings

List of escape sequences [here](https://en.wikipedia.org/wiki/Escape_sequences_in_C#Escape_sequences).

Strings can be concated by using a space: `"Hello" " " "World!"`.

## Enums syntax

Basic syntax:

	enum seasons {WINTER = 1, SPRING, SUMMER, AUTUMN}

The unspecified values increment the previous one (`SPRING = 2`, etc).

## Blocks

Brackets ({}) denote a block (or compound statement). Syntactically, t is treated as a single statement. This leads to some strange behaviour (example from page 56):

```c
if (n >= 0)
	for (i = 0; i < n; i++) {
		if (s[i] > 0) {
			printf("...");
			return i;
		}
else	/* WRONG */
	printf("error -- n is negative\n")
	}
```

## C Preprocessor

Useful use of conditionals on the preprocessor (including the appropiate header file for the OS based on some value):

```c
#if SYSTEM == SYSV
	#define HDR "sysv.h"
#elif SYSTEM == BSD 
	#define HDR "bsd.h"
#else
	#define HDR "default.h"
#endif
#include HDR
```

We can check for defined macros with `ifdef` and `ifndef`.

#### Libc Header files

- `limits.h`: Limits for each type
- `ctype.h`: Char manipulation (upper, lower, etc)
