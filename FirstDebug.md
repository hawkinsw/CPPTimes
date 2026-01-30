### What's News

SpaceX, Blue Origin and other space launch companies continue to look for new ways to power their rockets. The less volatile the fuel, the longer a ready-to-launch rocket can sit on the pad safely. Methane could be the answer.

### One of These Things is Not Like the Other

To begin understanding the C++ language, let's work backward from some broken source code, decipher its intent and then correct it so that it performs as intended. The programmer in our example is attempting to give us directions to some secret treasure that they bequeathed to us. They _want_ us to find what they left but it's hard to say whether they left the proper instructions:

```C++
#include <iostream>
/*
 * Follow the instructions printed on the screen to find the treasure.
 */
int main() {
  std::cout << "1. Orient your compass properly and count to 10";
  std::cout << "2. Next,\n";
  std::cout << "sail East toward the land";
  std::cout << "fill methane tank." << std::endl
  std::cout << "3. Lift off in the rocket ship.\n";
  return 0;
}
```

The first thing to notice is a missing `;` at the end of the _statement_ instructing us to fill a methane tank. Statements, complete instructions that define a computation, usually end in semicolons in C++.

The great thing about this error was that we could discover it with the help of the compiler. Any error that the compiler can detect is known as a _compile-time error_ because it is an error that happens, well, at compile time. Of course, this statement begs the question -- Just _what is_ compile time? Compile time is when the programmed is compiled. 

Here is the updated version of our code:

```C++
#include <iostream>
/*
 * Follow the instructions printed on the screen to find the treasure.
 */
int main() {
  std::cout << "1. Orient your compass properly and count to 10";
  std::cout << "2. Next,\n";
  std::cout << "sail East toward the land";
  std::cout << "fill methane tank." << std::endl;
  std::cout << "3. Lift off in the rocket ship.\n";
  return 0;
}
```

If there is a compile time, then there must be another time. That time is _run time_: the time when the program is executed. With the fix in (i.e., the addition of the missing `;`), our code now compiles. That's half the battle. Now we need to determine what happens when the code _runs_.

If we run the compiled code shown above, the output (perhaps unexpectedly) looks like

```
1. Orient your compass properly and count to 102. Next,
sail East toward the landfill methane tank.
3. Lift off in the rocket ship.
```

Only the author can say for sure whether or not that constitutes the proper path to the treasure. However, it certainly does not look like what they intended, especially given the way that the source code is formatted.

Although the code compiles and runs, it does not seem entirely correct. The mistake seems to appear when the program runs. We call these types of errors _run-time errors_.

It is my guess that the author _probably_ intended their message to read something more like:

```
1. Orient your compass properly and count to 10
2. Next,
sail East toward the land
fill methane tank.
3. Lift off in the rocket ship.
```

Let's see whether we can correct our bequeather's bumbles. We can add formatting information to the output so that the lines in the output break in the proper spots. There are two ways to include information in a C++ program about where to create new lines in the output: the newline _escape sequence_[^escape] (`\n`) and `std::endl`;

[^escape]: An escape sequence in a string is a pair characters: the `\` and another character. Even though an escape sequence is written with two characters, the system considers is a single character with a special meaning. The `\n` escape sequence is a newline. The `\t` is a tab character. There are [others](https://en.wikipedia.org/wiki/Bell_character), too!

To use a newline escape sequence, just embed it in the string _literal_ you are printing to the screen:

```C++
  std::cout << "1. Orient your compass properly and count to 10\n";
```

To use a `std::endl`, you will need to use another `<<`:

```C++
  std::cout << "sail East toward the land";
```

Notice how you can output something that is not a string literal, i.e. the `std::endl`, at the same time that you are outputting the string. We will rely on this functionality later when we output the contents of variables!

Here is our corrected code:

```C++
#include <iostream>

/*
 * Follow the instructions printed on the screen to find the treasure.
 */
int main() {
	std::cout << "1. Orient your compass properly and count to 10\n";
	std::cout << "2. Next,\n";
	std::cout << "sail East toward the land" << std::endl;
	std::cout << "fill methane tank." << std::endl;
	std::cout << "3. Lift off in the rocket ship.\n";
	return 0;
}
```

With the changes, let's confirm that we have fixed the author's runtime errors:

```
1. Orient your compass properly and count to 10
2. Next,
sail East toward the land
fill methane tank.
3. Lift off in the rocket ship.
```

Looks like the only thing left is to do as we were told and claim our reward!