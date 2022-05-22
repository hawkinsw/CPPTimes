And just like that the third edition of the _C++ Times_ is at your doorstep! Enjoy!


### One of These Things is Not Like the Other

We started off with the first Daily Debug of the semester. Our program to debug looked like this:

<html>
<pre><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>

<font color="#34E2E2">/*</font>
<font color="#34E2E2"> * This program will play the Two Truths and a Lie game</font>
<font color="#34E2E2"> * on behalf of the University of Cincinnati.</font>
<font color="#34E2E2"> */</font>
<font color="#87FFAF">int</font> main() {
  <font color="#34E2E2">// Describe to the user the game that they are playing.</font>
  std::cout &lt;&lt; <font color="#AD7FA8">&quot;One of these statements is not true. Can you guess which one?&quot;</font>;
  <font color="#34E2E2">// Display the choices.</font>
  std::cout &lt;&lt; <font color="#AD7FA8">&quot;1. George Rieveschl discovered Benadryl.&quot;</font>
  std::cout &lt;&lt; <font color="#AD7FA8">&quot;2. Mick and Oscar, stone lions, guard McMicken Hall.&quot;</font>;
  std::cout &lt;&lt; <font color="#AD7FA8">&quot;3. UC alum Vinod Dham created the Intel Pentium CPU.&quot;</font>;
  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
}</pre>
</html>

The first thing we noticed was a missing `;` at the end of _statement_ that printed the trivia fact about George Rieveschl. In C++, all statements, complete instructions that define a computation, must end in a semicolon. 

The great thing about this error was that we could discover it with the help of the compiler. Any error that the compiler can detect is known as a _compile-time error_ because it is an error that happens, well, at compile time. Of course, this statement begs the question -- Just _what is_ compile time? Compile time is when the programmed is compiled. 

If there is a compile time, then there must be another time ... the time when the program is executed. We call that the _run time_. With the fix in, we executed our code on [Compiler Explorer](https://godbolt.org/z/b473rrj1b).

Our output unexpectedly looked like

`One of these statements is not true. Can you guess which one?1. George Rieveschl discovered Benadryl.2. Mick and Oscar, stone lions, guard McMicken Hall.3. UC alum Vinod Dham created the Intel Pentium CPU.`

Definitely not what we wanted! This error occurred at run time and, therefore, we call it a _run-time error_. We cleaned up our run-time error by adding formatting information to the output. There are two ways to include information about where to create new lines: the newline escape sequence (`\n`) and `std::endl`;

To use a newline escape sequence, just embed it in the string you are printing to the screen:

<html>
<pre>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;2. Mick and Oscar, stone lions, guard McMicken Hall.\n&quot;</font>;
</pre>
</html>

To use a `std::endl`, you will need to use another `<<`:

<html>
<pre>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;2. Mick and Oscar, stone lions, guard McMicken Hall.&quot; << std::endl</font>;
</pre>
</html>

Notice how you can output something that is not a string literal (more on that later), i.e. the `std::endl`, at the same time that you are outputting the string. We will rely on this functionality later when we output the contents of variables!

Wohoo! With the additional formatting and the added `;`, our program does exactly what it is supposed to do:

<html>
<pre>
One of these statements is not true. Can you guess which one?
1. George Rieveschl discovered Benadryl.
2. Mick and Oscar, stone lions, guard McMicken Hall.
3. UC alum Vinod Dham created the Intel Pentium CPU.
</pre>
</html>
   
### Go Directly to the CPU, Do Not Pass Go

Next, we reviewed the compilation process one final time! See [last Thursday's C++ Times](https://uc.instructure.com/courses/1533374/pages/the-c++-times-for-5-slash-12-slash-2022) for a full report.

### Bjarne's Anatomy

In order to move forward in the class we will have to all speak the same language about C++. We are going to spend a tremendous amount of time during the semester talking to one another about code -- what it does, why it does it and how to write it. If we do not have a shared vocabulary to talk about the components of a C++ program, that communication is going to be very difficult! 

Here is an annotated C++ program with different parts and pieces labeled:

<html>
<pre><font color="#FCE94F">  1 </font><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>
<font color="#FCE94F">  2 </font><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;string&gt;</font>
<font color="#FCE94F">  3 </font>
<font color="#FCE94F">  4 </font><font color="#34E2E2">/*</font>
<font color="#FCE94F">  5 </font><font color="#34E2E2"> * This program will print Hello, World. A &quot;Hello, World&quot;</font>
<font color="#FCE94F">  6 </font><font color="#34E2E2"> * program is the first program that a developer writes in</font>
<font color="#FCE94F">  7 </font><font color="#34E2E2"> * a language they are learning.</font>
<font color="#FCE94F">  8 </font><font color="#34E2E2"> */</font>
<font color="#FCE94F">  9 </font><font color="#87FFAF">int</font> main() {
<font color="#FCE94F"> 10 </font>  <font color="#34E2E2">// Declare some variables.</font>
<font color="#FCE94F"> 11 </font>  std::string hello{<font color="#AD7FA8">&quot;Hello&quot;</font>};
<font color="#FCE94F"> 12 </font>  std::string world{<font color="#AD7FA8">&quot;World&quot;</font>};
<font color="#FCE94F"> 13 </font>  <font color="#34E2E2">// Print the output to screen.</font>
<font color="#FCE94F"> 14 </font>  std::cout &lt;&lt; hello &lt;&lt; <font color="#AD7FA8">&quot;, &quot;</font> &lt;&lt; world &lt;&lt; <font color="#AD7FA8">&quot;!</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F"> 15 </font>  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
<font color="#FCE94F"> 16 </font>}
</pre>
</html>


| | Entity | Definition | Pronunciation |
| -- | -- | -- | -- | 
|1| `#include` | preprocessor directive | pound include |
|2| `#include` | preprocessor directive | pound include |
|3| `int`, `return` | keyword | |
|4| `main` | programmer-defined identifier | |
|5| `(` | | open parenthesis | 
|6| `)` | | close parenthesis |
|7| `{` | | open curly brace |
|8| `std::string hello = "Hello";` | statement | |
|9| `hello` | variable | |
|11| `std::string hello = "Hello";` | variable definition | |
|12| `std::string hello = "Hello";` | variable declaration | |
|13| `std::string hello = "World";` | statement | |
|14| `world` | variable | |
|15| `std::string hello = "World";` | variable declaration | |
|16| `std::string hello = "World";` | variable definition | |
|17| `“Hello”` | string literal | | 
|18| `“World”` | string literal | | 
|19| `std::cout << hello << ", " << world << "!\\n";` | statement | |
|20| `", "` | string literal | |
|21| `"!\n"` | string literal | |
|22| `\n` | escape sequence | backslash n |
|23| `<<` | (stream insertion) operator | shovel shovel |
|24| hello | operand (of `<<` operator) || 
|25| `", "` | operand (of `<<` operator) || 
|26| `world` | operand (of `<<` operator) || 
|27| `"!\n"` | operand (of `<<` operator) || 
|28| `std::cout` | programmer-defined identifier | stood see out |
|29| `return 0;` | statement | |
|30| `0` | literal | | 
|31| `return` | reserved word | |
|32| `}` | | close curly brace |

We have explicitly discussed the utility of most of those statements, but not *every* one. For instance, we have not learned everything about variable declarations and definitions (9, 11, 12, 15, 16) yet. 

#### Notes: 

Lines 1 and 2 are preprocessor directives and are handled by the preprocessor during the first phase of compilation (remember what that phase is called?). Any line that starts with a `#` is a preprocessor directive and the preprocessor handles everything between that `#` and the end of the line. The fact that a preprocessor directive extends to the _end of the line_ and not to the next `;` makes it unique in the language. 

The preprocessor directive `include` tells the preprocessor to substitute the contents of the file named between the `<` and the `>` with the line of the line of the preprocessor! While _any_ file could be given between the *shovels*, the names of files containing C++ code that defines additional, useful functionality are most likely given. 

The `string` and `iostream` files define functionality for using string variables (see below) and producing output for the screen, respectively. We will see other files `#include`d during the semester.

The `\n` (22, used on line 14) is just one type of _escape sequence_, any two-character combination of `\` and any other character. There are several pre-defined escape sequences that are commonly used: `\n`, `\t`, `\r`, etc. The `\n` is such a common escape sequence that it has its own name: *the newline escape sequence*. Even though `\n` looks like two characters, the compiler wears a very odd pair of glasses. Through those glasses, that compiler sees the two characters that comprise an escape sequence as a single character. 

If you are interested in *why* escape sequences exist, please ask! I'd love to tell you the story!

The stream insertion operator (23, line 14) is a particular type of a general class of "things" in C++ known as _operators_. We will learn more about operators next class, I promise.

### C++ By the Slice

We concluded class by beginning to write the *PizzaParty* application. This is an application that we are going to continue to develop next class, too. 

The first version of the program simply generated output that described a pizza party!

**Pizza Party 1.0**
<html>
<pre><font color="#FCE94F">  1 </font><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>
<font color="#FCE94F">  2 </font>
<font color="#FCE94F">  3 </font><font color="#34E2E2">/*</font>
<font color="#FCE94F">  4 </font><font color="#34E2E2"> * This program describes Will&apos;s pizza party with his (only)</font>
<font color="#FCE94F">  5 </font><font color="#34E2E2"> * friend Kevin.</font>
<font color="#FCE94F">  6 </font><font color="#34E2E2"> */</font>
<font color="#FCE94F">  7 </font><font color="#87FFAF">int</font> main() {
<font color="#FCE94F">  8 </font>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will and his friend Kevin are sharing a large pizza.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F">  9 </font>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will eats 4 slices and Kevin eats 2.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F"> 10 </font>  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
<font color="#FCE94F"> 11 </font>}
</pre>
</html>

This is a "fine" program, as far as it goes. However, it lacks a certain something ...

**Pizza Party 2.0**

What would be really nice is if we were able to change the name of my friend relatively easily. In order to do that, we need functionality that allows for some amount of variability. For variability we will use an entity known as, not surprisingly, a _variable_. 

A _variable_ contains a value that can change throughout the life of a program. In C++ (unlike other popular languages), every variable must have a _type_ (we will define that term in the next class). We cannot simply use a variable -- we must declare it first. Ideally, before we access the value of a variable, we assign it some value. It is *very important* and *best practice* (but not strictly required) to assign some initial value to the variable at the time that you declare it. A variable declaration consists of the variables type, the variable's name and (optionally) its initial value. 

The naming of a variable is (mostly) an art. We want to name our variables so that their contents are obvious! For instance, we don't want to name a variable `distance` if that that variable is going to hold the time of day. `friend` seems like a good name for a variable that holds the name of our friend. We want the initial value of that variable to contain `Kevin`. To declare a variable named `friend` that holds strings and has the initial value `Kevin` we would write:

<html>
<pre>  std::string <font color="#FCE94F">friend</font> {<font color="#AD7FA8">&quot;Kevin&quot;</font>};
</pre>
</html>

Not so fast! There's a problem! In C++ we are allowed a significant amount of latitude in how we named _programmer-defined identifiers_. So far the only programmer-defined identifier that we have seen are variables. In the future, we will see others. However, we cannot name them anything! There are a few restrictions:
1. The name cannot be a reserved word (a.k.a., key word).
2. The name cannot start with a number.
3. The name can only contain numbers (subject to (2)), letters (upper or lower case) and the `_` (pronounced "underscore").

Is `friend` a valid programmer-defined identifier? Unfortunately, no, it is not. `friend` is a reserved word in the C++ language and we will learn its meaning in the future. We have to choose another name. We could choose `wills1Friend`, `wills_friend`, etc. Programmers usually vary the capitalization in programmer-defined identifiers or insert `_`s to indicate the separation of words (the former is called *camel case* style). You can use either one -- they are just conventions -- but try to be consistent throughout your code.

I think that we will name the variable `wills_friend`. 

Rewriting Pizza Party 1.0 to use our variable, we have 

<html>
<pre><font color="#FCE94F">  1 </font><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>
<font color="#FCE94F">  2 </font>
<font color="#FCE94F">  3 </font><font color="#34E2E2">/*</font>
<font color="#FCE94F">  4 </font><font color="#34E2E2"> * This program describes Will&apos;s pizza party with his (only)</font>
<font color="#FCE94F">  5 </font><font color="#34E2E2"> * friend Kevin.</font>
<font color="#FCE94F">  6 </font><font color="#34E2E2"> */</font>
<font color="#FCE94F">  7 </font><font color="#87FFAF">int</font> main() {
<font color="#FCE94F">  8 </font>  std::string wills_friend{<font color="#AD7FA8">&quot;Kevin&quot;</font>};
<font color="#FCE94F">  9 </font>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will and his friend &quot;</font> &lt;&lt; wills_friend
<font color="#FCE94F"> 10 </font>            &lt;&lt; <font color="#AD7FA8">&quot; are sharing a large pizza.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F"> 11 </font>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will eats 4 slices and &quot;</font> &lt;&lt; wills_friend &lt;&lt; <font color="#AD7FA8">&quot; eats 2.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F"> 12 </font>  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
<font color="#FCE94F"> 13 </font>}
</pre>
</html>

Notice how we are able to use the `wills_friend` variable along with the stream insertion operator (`<<`) in a way that we earlier used `std::endl` with the stream insertion operator to print the value of the `wills_friend` variable next to a string literal. Our power to intersperse variables with literals using the `<<` is really amazing!

Also, notice how we can separate a single statement on multiple lines (9, 10). Remember: In C++, spacing matters (almost) not at all!

In the next lecture we will continue to talk about the Pizza Party, go in to a further discussion of types, operators and operands!

### Definitions

Besides the definitions included above, we also defined:

1.  _syntax_: The rules that define what it means to be a valid (not correct!) program. The compiler checks a program's syntax. 
2.  _syntax errors_: An error in the validity of your program according to C++'s syntax. Syntax errors are detected when the compiler executes. They are _compile-time_ errors.