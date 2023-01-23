### What's News

Scientists have made a transformative discovery in their historic quest for the Fountain of Youth. While searching in Florida they discovered the bones of Ponce De Leon which had been turned in to gold. After analysis, they discovered the means of this transformation and believe they have unlocked the secret of turning any material into valuable bullion. 

### Compilation

Like alchemy, *compilation* is the process of turning the source code (that we write in a *high-level language* like C++) into code that the computer can execute. It consists of three different phases:

1. Preprocessing
2. Compilation
3. Linking

Because the entire process is called compilation but one of the three steps is named compilation, too, some people refer to the overall process as *building*. Either is acceptable in this class.

#### Preprocessing

In the preprocessing phase, your source code is searched for lines of code starting with a #. These lines are known as preprocessor directives and tell the preprocessor to perform a certain action. The most common processor directive you will see is the #include preprocessor directive. This directive instructs the preprocessor to replace that line of code with the contents of another file (the one named within the < and >). We saw what that looks like during our demonstration.

#### Compilation
In the compilation step, our code (after it has been preprocessed) is converted into code that the compiler can execute natively (known as object code and stored in object files). Unfortunately, however, that code is missing some very important supporting information. That missing information is itself code (known as runtime libraries) that the compiler can execute and provides operations for doing low-level operations like accessing/storing files, communicating over the Internet, etc.


#### Linking
In the linking step, the native code generated from our preprocessed source code is combined with the runtime libraries. *Runtime* libraries are support libraries provided by a high-level programming language or the operating system that support platform-specific functions that vary from hardware-vendor to hardware-vendor or operating-system to operating system. For example, accessing a hard drive is something that varies depending on the operating system and the hardware-vendor so there are runtime libraries that make it easier. If you program uses that functionality, it will need to be linked to those libraries.

### Painting the Compilation Canvas

Compilation (aka building) is a pipeline where the output of one phase becomes the input to the next phase. Here is a visual description of the compilation process:

![](./graphics/C%2B%2B%20Compilation%20Process.png)


### Definitions

0. programming language: a special language used to write computer programs.
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