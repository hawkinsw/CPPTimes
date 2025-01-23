### What's News

Scientists have made a transformative discovery in their historic quest for the Fountain of Youth. While searching in Florida they discovered the bones of Ponce De Leon which had been turned in to gold. After analysis, they discovered the means of this transformation and believe they have unlocked the secret to a means of not only reversing the aging process but also of turning any material into valuable bullion. 

### Compilation

Like alchemy, *compilation* is the process of turning one type of material into another. For many alchemists, the goal is to turn a common element (say, dirt) into a rare element (gold!). The premise is that the common element is cheap but the rare element is expensive -- essentially, to these wizards the goal is create value out of nothing! We are a different type of alchemist, but our goal is the same and the compiler helps us transform material. The compiler transforms the source code (that we write in a *high-level language* like C++) into code that the computer can execute (named machine code, or low-level code). Compilation consists of three different phases:

1. Preprocessing
2. Compilation
3. Linking

Because the entire process is called compilation _and_ one of the three steps is named compilation, some people refer to the overall process as _building_. Either is acceptable in this class.

#### Preprocessing

In the preprocessing phase, a tool called the preprocessor searches your source code for lines starting with a `#` (pronounced "pound sign"). These lines are known as _preprocessor directives_ and tell the preprocessor to perform a certain action. The most common processor directive you will see is the `#include` preprocessor directive. This directive instructs the preprocessor to replace that line of code with the entire contents of another file (the one named within the `<` and `>`). The result of the preprocessor step is more C++ code. That modified C++ code serves as the input to the _compilation_ step.

#### Compilation
In the compilation step, our code (after it has been preprocessed) is converted into code that the computer can execute natively (known as _object code_). Object code is stored in _object files_. Even though object code is in a language that the computer can execute, it is missing some very important supporting information. That missing information is itself code (known as runtime libraries) that the machine can execute and provides operations for doing low-level operations like accessing/storing files, communicating over the Internet, etc.

#### Linking
In the linking step, the machine code generated from our preprocessed source code is combined with runtime libraries. _Runtime_ libraries are support libraries provided by a high-level programming language or the operating system that support platform-specific functions that vary from hardware-vendor to hardware-vendor or operating system to operating system. For example, accessing a hard drive is something that varies depending on the operating system and the hardware vendor so there are runtime libraries that make it easier. If your program uses that functionality, it will need to be linked to those libraries.

### Painting the Compilation Canvas

Compilation (aka building) is a pipeline where the output of one phase becomes the input to the next phase. Here is a visual description of the compilation process:

![](./graphics/C%2B%2B%20Compilation%20Process.png)
