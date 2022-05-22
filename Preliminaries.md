## The C++ Times for 1/13/2022

And just like that the second edition of the _C++ Times_ is at your doorstep! Enjoy!

### Recap

In our second class, we

- Defined and discussed the Von Neumann Architecture and talked about the fundamental components of every modern computer.
- Note that every byte is accessible by its address.
- Defined the Fetch, Decode, Execute cycle of the Von Neumann Architecture and talked about how the computer's operation is simply a never-ending loop of these three operations.
- Discussed how programmers prefer to write programs in languages that look like natural language (high-level programming languages), but came to the conclusion that the processor can only execute very basic operations that must be specified using cryptic commands (low-level programming languages, machine code) that are limited to simple arithmetic and logic.
- Emphasized that there are two (2) audiences for a computer program: 1) the computer and (more importantly) 2) fellow programmers.
- Talked about how programmers use a compiler to bridge the gap between high-level and low-level languages.
- Simplified every computer program is written in source code and performs three fundamental operations: accept input, process/calculate, generate output.

One of the programs that we talked about during the lecture was the compiler itself. There is nothing special about a compiler. It's just a program like any other program. How do we know? Well, it performs the same three operations that every program performs:
1. It takes input. The compiler's input is your source code! What you write!
2. The processes/calculates. The compiler processes your input and turns it into code that the compiler can execute natively on the CPU.
3. It generates output. The compiler generates an executable file!

Now, if that's not cool, I don't know what is!

### Compilation

We saw a demo that showed the compilation process in detail and quickly summarized the key steps of compilation. To be specific, compilation happens in three steps:

1.  preprocessing (the output of this step is a preprocessed file and is the input to Step 2): In the preprocessing phase, your source code is searched for lines of code starting with a `#`. These lines are known as preprocessor directives and tell the preprocessor to perform a certain action. The most common processor directive you will see is the `#include` preprocessor directive. This directive instructs the preprocessor to replace that line of code with the contents of another file (the one named within the `<` and `>`). We saw what that looks like during our demonstration.
2.  compilation (the output of this step is object code and is the input to Step 3): In the compilation step, our code (after it has been preprocessed) is converted into code that the compiler can execute natively (known as object code and stored in object files). Unfortunately, however, that code is missing some very important supporting information. That missing information is itself code (known as runtime libraries) that the compiler can execute and provides operations for doing low-level operations like accessing/storing files, communicating over the Internet, etc.
3.  linking (the output of this step is an executable program): In the linking step, the native code generated from our preprocessed source code is combined with the runtime libraries.

Because, confusingly, the entire process is called compilation at the same time that one of the three steps is called compilation, some people prefer to call the entire process the build process. If that vocabulary helps you, feel free to use it. Otherwise, feel free to simply disambiguate based on context.

There is a (I think) helpful figure at the bottom of this edition of the C++ Times that gives a visual representation of the compilation process.

### Definitions

11. programming language: a special language used to write computer programs.
1. computer program: A set of instructions a computer follows in order to perform a task.
2. compiler: The tool that converts from high- to low-level languages.
3. compiling: The process of invoking the compiler to translate between high- and low-level programming languages.
4. object code: instructions executable by the CPU.
5. object file: a file containing object code.
6. preprocessing: A phase of the compilation process that modifies the program's source code in accordance with preprocessor directives.
5. compiling (as a step of the compilation process): A phase of the compilation process that translates preprocessed source code into object code.
6. linking: A phase of the compilation process that combines object code (produced by the compiler and from the system) into an executable.
7. binary: a way to represent numbers using only 1s and 0s.
8. bit: the smallest denomination of information -- either a 1 or a 0; it is a portmanteau of binary and digit.
9. byte: a grouping of 8 bits.
10. pipeline: the connection between the CPU and the memory.
11. runtime library: A library of prewritten code for performing low-level, hardware-specific tasks or for performing common, but sometimes intricate, calculations (e.g., mathematical operations, sorting operations, etc.).

### Compilation -- A Warhol Painting

![The C++ compilation process, visualized.](./graphics/C++%20Compilation%20Process.png)