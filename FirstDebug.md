### What's News

TODO

### One of These Things is Not Like the Other

To begin understanding the C++ language, let's work backward from some broken source code, decipher its intent and then correct it so that it performs as intended: 

```C++
#include <iostream>
/*
 * This program will play the Two Truths and a Lie game
 * on behalf of the University of Cincinnati.
 */
int main() {
    // Describe to the user the game that they are playing.
    std::cout << "One of these statements is not true. Can you guess which one?";
    // Display the choices.
    std::cout << "1. George Rieveschl discovered Benadryl." 
    std::cout << "2. Mick and Oscar, stone lions, guard McMicken Hall.";
    std::cout << "3. UC alum Vinod Dham created the Intel Pentium CPU.";
    return 0;
}
```

The first thing we noticed was a missing `;` at the end of the _statement_ that printed the trivia fact about George Rieveschl. In C++, all statements, complete instructions that define a computation, must end in a semicolon. 

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