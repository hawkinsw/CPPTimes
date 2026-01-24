### What's News
Activist investors have made a record number of attempts this fiscal quarter to encourage conglomerates to spin off subsidiaries to unlock value. The investing instigators claim that these companies overall are underpriced in the market relative to the value of their individual businesses. To help with this analysis they are hiring a record number of people who can identify and name the valuable pieces of businesses. 

### Bjarne's Anatomy - The Parts

In order to move forward in our C++ education, we will all have to speak the same language. We are going to spend a tremendous amount of time in our careers talking to fellow programmers about code -- what it does, why it does it and how to write it. If we do not have a shared vocabulary to talk about the components of a C++ program, that communication is going to be very difficult! 

Here is an annotated C++ program with different parts and pieces labeled:

```C++
#include <iostream>
#include <string>
/*
 * This program will print Hello, World. A "Hello, World"
 * program is the first program that a developer writes in
 * a language they are learning.
 */
int main() {
    // Declare some variables.
    std::string hello{"Hello"};
    std::string world{"World"};
    // Print the output to screen.
    std::cout << hello << ", " << world << "!\n";
    return 0;
}
```
| | Entity | Definition | Pronunciation |
| -- | -- | -- | -- | 
|| `#include` | preprocessor directive | pound include |
|| `#include` | preprocessor directive | pound include |
|| `int`, `return` | keyword | |
|| `main` | programmer-defined identifier | |
|| `(` | | open parenthesis | 
|| `)` | | close parenthesis |
|| `{` | | open curly brace |
|| `std::string hello{"Hello"};` | statement | |
|| `hello` | variable | |
|| `std::string hello{"Hello"};` | variable definition | |
|| `std::string hello{"Hello"};` | variable declaration | |
|| `std::string hello{"World"};` | statement | |
|| `world` | variable | |
|| `std::string world{"World"};` | variable declaration | |
|| `std::string world{"World"};` | variable definition | |
|| `"Hello"` | string literal | | 
|| `"World"` | string literal | | 
|| `std::cout << hello << ", " << world << "!\\n";` | statement | |
|| `", "` | string literal | |
|| `"!\n"` | string literal | |
|| `\n` | escape sequence | backslash n |
|| `<<` | (stream insertion) operator | shovel shovel |
|| hello | operand (of `<<` operator) || 
|| `", "` | operand (of `<<` operator) || 
|| `world` | operand (of `<<` operator) || 
|| `"!\n"` | operand (of `<<` operator) || 
|| `std::cout` | programmer-defined identifier | stood see out |
|| `return 0;` | statement | |
|| `0` | literal | | 
|| `return` | reserved word | |
|| `}` | | close curly brace |

### Say It Like You Mean It

It's one thing to be able to say what each component of a program is, but it's another to be able to describe what that component means.

A *literal* is, well, *literally* what is written. If it is a string literal (e.g., `"Hello"`) then it is just a string (the sequence of characters between the `"`s). If it is an `int`eger literal (e.g., `5`), then it is just a number. And so on. These literals' values cannot change. Oh, what's that I just said? Yes, they have a value! 

_Expressions_ also have values. In fact, that's the very definition of _expression_: anything with a value. In the program above, `0` in `return 0;`, `"Hello"` in `std::string hello{"Hello"};` and `hello` in `std::cout << hello << ", " << world << "!\n"` are expressions. Those aren't the only ones, though.

_Operators_ are a symbol that represents some operation. For example, `*` is an operator in C++ that represents the multiplication operation (among other things [more on that later]). In the program above, `<<` is an operator. What good is an operation without some data on which to operate? The data upon which operators operate are known as _operands_. In the program above, `hello` and `"!\n"` are operands (among others). 

There are a few words in C++ that are off limits for programmers to use when they name their functions and variables and whose meaning is fixed by the language. Those words are known as _key words_ (or _reserved words_). The key words in the program above are `int` and `return`. 

> Note: You can see the entire list of reserved words online at [cppreference](https://en.cppreference.com/w/cpp/keyword).

Finally, there are variables. _Variables_ are named locations for storage in memory. The variables in the program above are `hello` and `world`.

### Follow The Rules

A programming language specifies a set of rules that you (the programmer) must follow to construct a valid program. The rules lay out the valid ways that you can arrange operators, key words, operands, expressions, literals, etc. and still make a valid program. These rules are known as _syntax_. If you write a program that violates any of these rules, your program contains a _syntax error_.

Notice that we are using a very particular word: "valid". Just because a program is valid does not mean that it is correct. It is very hard to specify what constitutes a "correct" solution for a given problem let alone assess the veracity of a claim that a certain program offers a solution. In this class (and most others) you will learn how to write _valid_ programs and learn how you can write tests to demonstrate that you've also written _correct_ programs (and only occasionally will you touch on proving that the programs are _actually_ correct).