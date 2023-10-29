# MINT Language

MINT is a minimalist character-based interpreter but one which aims at fast performance, readability and ease of use. It is written for the Z80 microprocessor and is 2K.

## Table of Contents


## Reverse Polish Notation (RPN)

RPN is a [concatenative](https://concatenative.org/wiki/view/Concatenative%20language)
way of writing expressions in which the operators come after their operands.
This makes it very easy to evaluate expressions, since the operands are already on the stack.

Here is an example of a simple MINT program that uses RPN:

```
10 20 + .
```

This program pushes the numbers `10` and `20` are operands which are followed by an
operator `+` which adds the two operands together. The result becomes operand for
the `.` operator which prints the sum.

## Numbers in MINT

MINT on the Z80 uses 16-bit integers to represent numbers. A valid (but not very
interesting) MINT program can be simply a sequence of numbers. Nothing will happen
to them though until the program encounters an operator.

There are two main types of numbers in MINT: decimal numbers and hexadecimal numbers.

### Decimal numbers

Decimal numbers are represented in MINT in the same way that they are represented
in most other programming languages. For example, the number `12345` is represented
as `12345`. A negative number is preceded by a `-` as in `-786`.

### Hexadecimal numbers

Hexadecimal numbers are represented in MINT using the uppercase letters `A` to `F`
to represent the digits `10` to `15`. Hexadecimal numbers are prefixed with a `$`.
So for example, the hexadecimal number `1F3A` is represented as `$1F3A`.
Unlike decimal numbers, hexadecimal numbers are assumed to be positive in MINT.

### Formatting numbers

MINT provides commands for formatting hexadecimal and decimal numbers. The print
operator `.` prints numbers in the current base. To switch the base to hexadecimal
use the command `\\H` before using the `.` operator. To switch back to formatting
in decimal use the command `\\D`.

## Basic arithmetic operations

```
5 4 * .
```

In this program the numbers `5` and `4` are operands to the operator `*` which
multiplies them together. The `.` operator prints the result of the
multiplication.

```
10 20 - .
```

This program subtracts `20` from `10` which results in the negative value `-10`
The `.` operator prints the difference.

```
5 4 / .
```

This program divides 5 with 4 prints their quotient. MINT for the Z80 uses
16-bit integers. The remainder of the last division operation can accessed using
the `\r` system variable.

```
\r .
```

## Variables and Variable Assignment

Variables are named locations in memory that can store data. MINT has a limited
number of global variables which have single letter names. In MINT a variable can
be referred to by a singer letter from `a` to `z` so there are 52
globals in MINT. Global variables can be used to store numbers, strings, arrays, blocks, functions etc.

To assign the value `10` to the global variable `x` use the `!` operator.

```
10 x !
```

In this example, the number `3` is assigned to the variable `x`

To access a value in a variable `x`, simply refer to it.
This code adds `3` to the value stored in variable `x` and then prints it.

```
3 x@ + .
```

The following code assigns the hexadecimal number `$3FFF` to variable `a`
The second line fetches the value stored in `a` and prints it.

```
$3FFF a !
a .
```

In this longer example, the number `10` is stored in `a` and the number `20` is
stored in `b`. The values in these two variables are then added together and the answer
`30` is stored in `z`. Finally `z` is printed.

```
10 a !
20 b !
a@ b@ + z !
z@ .
```

## Variable operators

## Strings

MINT allows null-terminated strings to be defined by surrounding the string with `'` characters.

```
'hello there!' s !
```

Strings can be prints with the `\P` operator

```
s \.
```

prints out `hello there!`

### Printing values

MINT has a number of ways of printing to the output.

`<value> .` prints a value as a number. This command is affected by \H /dc /byt /wrd  
`<value> \C` prints a value as an ASCII character
`<value> \P` prints a value as a pointer to a null terminated string

Additionally MINT allows the user to easily print literal text by using \` quotes.

For example

```
100 x !
`The value of x is ` x .
```

prints `The value of x is 100`

## Logical operators

MINT uses numbers to define boolean values.

- false is represent by the number `0`
- true is any non-zero value, however the most useful representation is `1`.

```
3 0 = .
```

prints `0`

```
0 0 = .
```

prints `1`

MINT has a set of bitwise logical operators that can be used to manipulate bits. These operators are:

`&` performs a bitwise AND operation on the two operands.
`|` performs a bitwise OR operation on the two operands.
`^` performs a bitwise XOR operation on the two operands.
`{` shifts the bits of the operand to the left by the specified number of positions.
`}` shifts the bits of the operand to the right by the specified number of positions.

The bitwise logical operators can be used to perform a variety of operations on bits, such as:

- Checking if a bit is set or unset.
- Setting or clearing a bit.
- Flipping a bit.
- Counting the number of set bits in a number.

Here is an example of how to use the bitwise logical operators in MINT:

Check if the first bit of the number 10 is set

```
10 & 1 .
```

this will print `1`

Set the fourth bit of the number 10

```
1 }}} 1 | \H .
```

prints $0009

Flip the third bit of the number 10

```
1 {{ $0F \X \H .
```

prints $000B

## Conditional code

Code blocks are useful when it comes to conditional code in MINT.

The syntax for a MINT IF-THEN-ELSE or "if...else" operator in MINT is:

```
condition code-block-then code-block-else ?
```

If the condition is true, then code-block-then is evaluated and its value is returned.
Otherwise, code-block-else is evaluated and its value is returned.

Here is an example of a "if...else" operator in MINT:

```
10 x !
20 y !

x@ y@ > ( \'x is greater than y' )( \'y is greater than x' ) z !

z \P
```

In this example, the variable x is assigned the value 10 and the variable y is assigned the value 20. The "if...else" operator then checks to see if x is greater than y. If it is, then the string "x is greater than y" is returned. Otherwise, the string "y is greater than x" is returned. The value of the "if...else" operator is then assigned to the variable z. Finally, the value of z is printed to the console.

Here is another example of the "if...else" operator in MINT. This time, instead of creating a string just to print it, the following
code conditionally prints text straight to the console.

```
18 a !

`This person` a 18 > (`can`)(`cannot`) `vote`
```

In this example, the variable a is assigned the value 18. The "if...else" operator
then checks to see if age is greater than or equal to the voting age of 18. If it is,
then the text "can" is printed to the console. Otherwise, the string "cannot" is printed to the console.

## Functions in MINT

You can put any code inside `:` and `;` block which tells MINT to "execute this later".

Functions are stored variables with uppercase letters.

Storing a code block in the variable `Z`.

```
:Z `hello` 1. 2. 3. ;
```

Running the code block by stored in uppercase `Z` by referring to it

```
Z
```

will print out.

```
hello 1 2 3
```

A basic function with a single argument is represented as follows:

```
:F a ! a@ . ;
```

This function takes a single argument `a` and prints its value using the `.` operator.

Example: a function to square a value a

```
:F a ! a@ a@ * ;
```

### Function with Multiple Arguments

You can also define functions with multiple arguments. For example:

```
:F b ! a ! a@ b@ + . ;
```

This function takes two arguments `a` and `b`, adds them together using the `+` operator,
and then prints the result using `.`.

### Calling functions

Functions are called by referring to them

```
:F b ! a ! a@ b@ * ;
30 20 F .
```

This code passes the numbers `30` and `20` to a function which multiplies them and returns
the result which is then printed.

### Assigning Functions to Variables

In MINT, you can assign functions to variables just like any other value.
Variables in MINT are limited to a single uppercase or lowercase letter. To
assign a function to a variable, use the `=` operator.

Let's see some examples:

Here's a function to print a number between after a `$` symbol and storing t in variable `A`

```
:A a ! `$` a@ . ;
```

And calling it:

```
100 A
```

The `100` is passed to the function as argument `a`. The function first prints `$` followed by `1001

Here's a function to square two numbers. The function is stored in variable S

```
:S a ! a@ a@ * ;
```

Calling it:

```
4 S .
```

The number `4` is passed to the function S which squares the value and then prints it.

```
:T b ! a ! a@ b@ + ;
```

### Using Functions

Once you've assigned functions to variables, you can use them in your MINT code.

Example:

```
10 A       // prints 10
3 7 B      // prints 10, the sum of 3 and 7
```

In the first line, we execute the function stored in variable `A` with the argument `10`,
which prints `10`. In the second line, we execute the function stored in variable `B` with
arguments `3` and `7`, which results in `10` being printed (the sum of the two arguments).

### SYSTEM VARIABLES

System variables contain values which MINT uses internally but are available for programmatic use. These are the lowercase letters preceded by a \ e.g. \a, \b, \c etc. However MINT only uses a few of these variables so the user may use the other ones as they like.

### Using MINT on the TEC-1

MINT was designed for for small Z80 based systems but specifically with the small memory configuration of the TEC-1 single board computer. It is only 2K to work with the original TEC-1 and interfaces to the serial interface via a simple adapter.

On initialisation it will present a user prompt ">" followed by a CR and LF. It is now ready to accept commands from the keyboard.

### Loops

0(this code will not be executed but skipped)
1(this code will be execute once)
10(this code will execute 10 times)

You can use the comparison operators < = and > to compare 2 values and conditionally execute the code between the brackets.

## Arrays

MINT arrays are a type of data structure that can be used to store a collection of elements. Arrays are indexed, which means that each element in the array has a unique number associated with it. This number is called the index of the element.
In MINT, array indexes start at 0

To create a MINT array, you can use the following syntax:

_[ element1 element2 ... ]_

for example

```
[ 1 2 3 ]
```

Arrays can be assigned to variables just like number values

```
[ 1 2 3 ] a !
```

An array of 16-bit numbers can be defined by enclosing them within square brackets:

[1 2 3 4 5 6 7 8 9 0]

Defining an array puts its start address and length onto the stack

These can then be allocated to a variable, which acts as a pointer to the array in memory

[1 2 3 4 5 6 7 8 9 0] $ a!

The swap $ is used to get the starting address onto the top of the stack and then store that into the variable a.

To fetch the Nth member of the array, we can create a colon definition N

:N @ $ {+ @. ;

### List of operators

MINT is a bytecode interpreter - this means that all of its instructions are 1 byte long. However, the choice of instruction uses printable ASCII characters, as a human readable alternative to assembly language. The interpreter handles 16-bit integers and addresses which is sufficient for small applications running on an 8-bit cpu.

### Maths Operators

| Symbol | Description                               | Effect   |
| ------ | ----------------------------------------- | -------- |
| -      | 16-bit integer subtraction SUB            | a b -- c |
| /      | 16-bit by 8-bit division DIV              | a b -- c |
| +      | 16-bit integer addition ADD               | a b -- c |
| \*     | 8-bit by 8-bit integer multiplication MUL | a b -- c |
| \>     | 16-bit comparison GT                      | a b -- c |
| <      | 16-bit comparison LT                      | a b -- c |
| =      | 16 bit comparison EQ                      | a b -- c |
| {      | shift left                                | --       |
| }      | shift right                               | --       |

### Logical Operators

| Symbol | Description        | Effect   |
| ------ | ------------------ | -------- |
| \|     | 16-bit bitwise OR  | a b -- c |
| &      | 16-bit bitwise AND | a b -- c |
| \\X    | xor                | a b -- c |

Note: logical NOT can be achieved with 0=

### Stack Operations

| Symbol | Description                                                          | Effect         |
| ------ | -------------------------------------------------------------------- | -------------- |
| '      | drop the top member of the stack DROP                                | a a -- a       |
| "      | duplicate the top member of the stack DUP                            | a -- a a       |
| ~      | rotate the top 3 members of the stack ROT                            | a b c -- b c a |
| %      | over - take the 2nd member of the stack and copy to top of the stack | a b -- a b a   |
| $      | swap the top 2 members of the stack SWAP                             | a b -- b a     |

### Input & Output Operations

| Symbol | Description                                               | Effect      |
| ------ | --------------------------------------------------------- | ----------- |
| ?      | read a char from input                                    | -- val      |
| .      | print the top member of the stack as a decimal number DOT | a --        |
| ,      | print the number on the stack as a hexadecimal            | a --        |
| \`     | print the literal string between \` and \`                | --          |
| \\.    | print a null terminated string                            | adr --      |
| \\,    | prints a character to output                              | val --      |
| \\$    | prints a CRLF to output                                   | --          |
| \\>    | output to an I/O port                                     | val port -- |
| \\<    | input from a I/O port                                     | port -- val |
| #      | the following number is in hexadecimal                    | a --        |

| Symbol  | Description                     | Effect   |
| ------- | ------------------------------- | -------- |
| ;       | end of user definition END      |          |
| :<CHAR> | define a new command DEF        |          |
| \\:     | define an anonynous command DEF |          |
| \\^     | execute mint code at address    | adr -- ? |
| \\\_    | conditional early return        | b --     |

NOTE:
<CHAR> is an uppercase letter immediately following operation which is the name of the definition
<NUM> is the namespace number. There are currently 5 namespaces numbered 0 - 4

### Loops and conditional execution

| Symbol | Description                            | Effect |
| ------ | -------------------------------------- | ------ |
| (      | BEGIN a loop which will repeat n times | n --   |
| )      | END a loop code block                  | --     |
| \\\~   | if false break out of loop             | b --   |

NOTE 1: a loop with a boolean value for a loop limit (i.e. 0 or 1) is a conditionally executed block of code

e.g. 0(`will not execute`)
1(`will execute`)

NOTE 2: if you _immediately_ follow a code block with another code block, this second code block will execute
if the condition is 0 (i.e. it is an ELSE clause)

e.g. 0(`will not execute`)(`will execute`)
1(`will execute`)(`will not execute`)

### Memory and Variable Operations

| Symbol | Description                   | Effect        |
| ------ | ----------------------------- | ------------- |
| !      | STORE a value to memory       | val adr --    |
| [      | begin an array definition     | --            |
| ]      | end an array definition       | -- adr nwords |
| @      | FETCH a value from memory     | -- val        |
| \\!    | STORE a byte to memory        | val adr --    |
| \\[    | begin a byte array definition | --            |
| \\`    | begin a string definition     | -- adr        |
| \\@    | FETCH a byte from memory      | -- val        |

### System Variables

| Symbol | Description                        | Effect |
| ------ | ---------------------------------- | ------ |
| \\a    | data stack start variable          | -- adr |
| \\b    | base16 flag variable               | -- b   |
| \\c    | carry flag variable                | -- adr |
| \\d    | start of user definitions          | -- adr |
| \\h    | heap pointer variable              | -- adr |
| \\i    | loop counter variable              | -- adr |
| \\j    | outer loop counter variable        | -- adr |
| \\t    | text input buffer pointer variable | -- adr |

### Miscellaneous

| Symbol | Description                                   | Effect |
| ------ | --------------------------------------------- | ------ |
| \\\\   | comment text, skips reading until end of line | --     |

### Utility commands

| Symbol | Description                     | Effect   |
| ------ | ------------------------------- | -------- |
| \\#0   | execute machine code at address | adr -- ? |
| \\#1   |                                 | --       |
| \\#2   |                                 | -- val   |
| \\#3   | stack depth                     | -- val   |
| \\#4   | print stack                     | --       |
| \\#5   | print prompt                    | --       |
| \\#6   | edit command                    | val --   |

### Control keys

| Symbol | Description                     |
| ------ | ------------------------------- |
| ^B     | toggle base decimal/hexadecimal |
| ^E     | edit a definition               |
| ^H     | backspace                       |
| ^J     | re-edit                         |
| ^L     | list definitions                |
| ^P     | print stack                     |
