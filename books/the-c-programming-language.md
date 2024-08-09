The C Programming Language, Second Edition

> Disclaimer! I read this book being already familiar with C. I only took notes of some more detailed aspects of the language. It has been reordered, so it does not follow the book exactly.

# A Tutorial Introduction

Important difference between definition and declaration of variables:

- Definition: Space assigned to a variable (and maybe value)
- Declaration: Assigns a variable name and type to that variable

A variable can be:

- Internal: Available only in a certain scope
- External: Available during all the program

Use the keyword `extern` to use external variables from other source files.

Static and External variables are assigned to `0`.

All functions are external.

External objects with the same name reference always to the same object.

List of escape sequences [here](https://en.wikipedia.org/wiki/Escape_sequences_in_C#Escape_sequences).

Strings can be concated by using a space: `"Hello" " " "World!"`.

#### Enums

Basic syntax:

	enum seasons {WINTER = 1, SPRING, SUMMER, AUTUMN}

The unspecified values increment the previous one (`SPRING = 2`, etc).

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


#### Libc Header files

- `limits.h`: Limits for each type
- `ctype.h`: Char manipulation (upper, lower, etc)
