### What's News

Scientists have made a transformative discovery in their historic quest for the Fountain of Youth. While searching in Florida, they discovered the bones of Ponce De Leon which had been turned in to gold. After analysis, they discovered the means of this transformation and believe they have unlocked the secret to not only reversing the aging process but also of turning any material into valuable bullion. 

### Compilation

Like alchemy, _compilation_ is the process of turning one type of material into another. For many alchemists, the goal is to turn a common element (say, dirt) into a rare element (gold!). The premise is that the common element is cheap but the rare element is expensive -- essentially, to these wizards the goal is create value out of nothing! We are a different type of alchemist, but our goal is the same and the compiler helps us transform material. The compiler transforms the source code (that we write in a _high-level language_ like C++) into code that the computer can execute (named machine code, or low-level code). Compilation consists of three different phases:

1. Preprocessing
2. Compilation
3. Linking

Because the entire process is called compilation _and_ one of the three steps is named compilation, some people refer to the overall process as _building_. Either is acceptable in this class.

#### Preprocessing

In the preprocessing phase, a tool called the preprocessor searches your source code for lines starting with a `#` (pronounced "pound sign"). These lines are known as _preprocessor directives_ and they tell the preprocessor to perform a certain action. The most common preprocessor directive you will see is the `#include` preprocessor directive. This directive instructs the preprocessor to replace that line of code with C++ code from another file (the one named within the `<` and `>`[^1]). The result of the preprocessor step is more C++ code. That modified C++ code serves as the input to the _compilation_ step.

[^1]: Note that you can also put the name of the file subject to a `#include` between `"`s, too. The meaning is _slightly_ different, though.

#### Compilation
In the compilation step, your code (after it has been preprocessed) is converted into code that the computer can execute natively (known as _object code_). Object code is stored in _object files_. Even though object code is in a language that the computer can execute, it is (often) missing some very important supporting information. That missing information is itself code (known as runtime libraries) that the machine can execute and provides operations for doing low-level operations like accessing/storing files, communicating over the Internet, etc.

#### Linking
In the linking step, the machine code generated from your preprocessed source code is combined with runtime libraries. _Runtime_ libraries are support libraries provided by a high-level programming language or the operating system that support platform-specific functions that vary from hardware-vendor to hardware-vendor or operating system to operating system. For example, accessing a hard drive is something that varies depending on the operating system and the hardware vendor so the operating system and runtime libraries are available to make it easier. If your program uses that functionality, it will need to be linked to those libraries.

### Painting the Compilation Canvas

Compilation (aka building) is a pipeline where the output of one phase becomes the input to the next phase. Here is a visual description of the compilation process:

![](./graphics/C%2B%2B%20Compilation%20Process.png)

### Sidebar: The Relationship Between `#include` And Linking

It is easy to be confused about the difference between `#include`ing and linking. Based on the discussion above, it seems that the utility of both is to use code that you did not develop. That is absolutely the case! And, in fact, it is _usually_ the case that you will have to use a `#include` and link in a runtime library to use code that you did not develop. Just what does that mean?

In the very near future we will discuss the difference between declaration and definition. A declaration, simply, is a way to introduce a particular name into your program. A declaration is the minimum amount of information the compiler needs to analyze code that uses what is declared.

For instance, if your code used some external functionality that had the name `calculate_next_move`, after processing only the declaration of `calculate_next_move`, the compiler could compile your code if it used that functionality. On the other hand, if you had code that used `calculate_next_move` but did not include a declaration for it, the compiler would generate an error!

However, ultimately, your program needs to do more than just know the name of that `calculate_next_move` that you want to use. It actually needs to know what that `calculate_next_move` actually, well, calculates. In the linking phase of compilation, _that_ code -- the code that contains the _definition_ of `calculate_next_move` -- is what is _linked_ into the object file that the compiler generates from the high-level C++ code that you wrote.

If that discussion of the difference between declaring something and defining something is helpful, then great. Otherwise, forget I said anything! When we come to the topic in class, it will all make perfect sense!
