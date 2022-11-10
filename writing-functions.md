
## What's News
Inflation in the market for programming talent has hit an all-time record. More users are trading down to store-brand code in order to save money. There have even been *arguments* between employees and customers about *return*ing goods over buyer's remorse. Ingenious researchers everywhere are trying to learn how to do more with less.

### The Curse of Copy And Paste

Let's say that I am writing a program that is going to compare the contents of two files. The first thing that I will have to do is to, well, open those two files, right? Let's write some code to do that!

```C++
#include <iostream>
#include <fstream>

int main() {

  std::ifstream file_a, file_b;

  file_a.open("file_a.txt");
  if (!file_a.is_open()) {
    std::cout << "Could not open file_a.txt.n";
    return 1;
  }

  file_b.open("file_b.txt");
  if (!file_a.is_open()) {
    std::cout << "Could not open file_a.txt.n";
    return 1;
  }

  /*
   * We can now work with files A and B!
   */

  file_a.close();
  file_b.close();
}
```

That was pretty easy, right?

WRONG.

If you look closely at the code above, you will notice that there are several errors! In particular, the code refers to `file_a` in places where it should refer to `file_b`. Why did that happen? It happened because I was sloppy when I was copying and pasting the code. Because the code to open the two files was nearly identical I got carried away and didn't pay close attention!

But there are more problems with the code above than just that. As we've said before, programmers are lazy. It took the programmer "twice" as long to code the program above because they had to retype the same code twice. More importantly, what if the user requests changes to the names of the files that the program manipulates? The programmer will have to track down all those instances (and make sure not to miss any) and make the appropriate changes. Finally, consider what happens if the programmer finds a bug in some code that they have copied and pasted? They will have to remember all the places where they duplicated the code and fix the error in every. single. place. Not good at all!

The good news is that we can solve all this with ...

## Functions

We have already learned how to _call_ functions but we now want to know how to _write our own_ functions. Before we do that, let's remind ourselves about the purpose of functions: A function is a means to collect "statements that perform[] a specific task" in order to "break down a problem into small, manageable pieces."  A function can generate a value, perform some process or both generate a value and perform some process. The _call_ to a function that generates a value is just like any other expression and can be used (with some caveats) anywhere any expression can be used!

Every function has 4 components:

1.  A return type
2.  A name
3.  A list of parameters
4.  A body

The _return type_ defines the _type_ of value that the function generates when it is called. The _name_ defines the way that the programmer refers to the function when calling it. A function name must meet the same criteria as names for variables. The _list of parameters_ defines a set of variables the caller and the function implementation can use to "communicate". The body contains the code that performs the process, generates a value or both. 

In C++ it is possible to separate the _declaration_ of a function from the _definition_ of a function. The _declaration_ of the function is sometimes known as the function's _prototype_. The declaration specifies the function's return type, name and list of parameters _but not its body_. In other words, with just the declaration C++ does not have enough information about the function for a programmer to _call_ it. When a function declaration appears separately from the definition, you must still define the function. And when you do that, you still have to write all that information again -- the return type, name, list of parameters (yes, again!) _and_ _then_ write the body.

Why does C++ include this functionality? Because in order to _call_ a function in C++ the compiler must "know about" the function. If there were only one way to tell the compiler about the function (by defining it), then you would always have to define a function before calling it. What if the body of function A calls function B and the body of function B calls function A? You cannot very well define both functions before themselves!! So, C++ allows you to declare a function before its definition just for this case!

The syntax for defining a function is this:

```C++
return_type function_name(parameter1_type parameter1_name, parameter2_type parameter2_name) {
  statement1;
  statement2;
}  
```

The syntax for declaring a function is this:

```C++
return_type function_name(parameter1_type parameter1_name, parameter2_type parameter2_name);
```

TL;DR: in a function declaration, the `;` replaces the body of the function.

In the slot of the `return_type`, you can place any valid C++ type. In the slot of the `function_name` you can write any name (again, though, it must meet the same criteria for variable names). Each item in the parameter list is separated by _commas_. The items are pairs of variable types and variable names.

## Return Statement to Generate a Function's Value

The `return_type` of the function is the type of the value generated by the function. It is the primary means by which an invoked _function_ communicates with its _caller_.

In the body of the function, the programmer writes a _return statement_ to specify the value generated by the function. Let's write a function that takes no parameters and simply generates the number 2:

```C++
int two() {
  return 2;
} 
```

What is the function's return type? What is the function's name? What are the types and names of the function's parameters? (The last one is a trick question!) You can see that the value "returned" (this is how professional programmers pronounce that statement) by the function (the literal 2) is the same as the return type. This is an absolute requirement. What if we wanted to write the same function, but return the string "two" instead?

```C++
std::string two() {
  return std::string{"two"};
}
```

Again, the value returned by the function matches the return type. To reiterate, the return value is the primary means of communication between the function and the caller. We will see later that there are other ways for the function to communicate with the caller, but you should primarily use the return value for this purpose.

What if the function that you are writing does not need to generate a value? For instance, let's write a function that just prints `Hello, World.` to the screen:

```C++
void print_hello_world() {
  std::cout << "Hello, World.\n";
}
```

These functions simply perform a process but generate no value. In some sense, they are not like functions at all -- at least the ones that we learned about in math class. A function that performs a process but generates no value is a a so-called _void function_ (because their return type is _void_). You can optionally include a `return` statement in a void function, but it's not necessary:

```C++
void print_hello_world() {
  std::cout << "Hello, World.\\n";
  return;
}
```

When you learn later (below) how to use the `return` statement for program control, you will see that sometimes in a void function you will want to use a return statement.

## Parameters

Like we said above, parameters are the way that implementers of functions take input from the functions' caller. (Remember that the function's return value is the primary means for communicating in the opposite direction.) The programmer calling the function provides values for parameters (so-called arguments). The values that the caller gives for the function's parameters (again, known as the arguments) can 

1. provide data for the function to manipulate,
2. tell the function how to handle special cases, etc.

Let's write a function that adds two numbers:

```C++
int add(int a, int b) {
  return a + b;
}
```

Break it down: the `add` function takes two `int` parameters (named `a` and `b`) and returns an `int` (their sum). Again, notice that the return type matches the type of the value being returned. The body of the function can use the values of the parameters by their names. The parameters are only in scope in the body of the function -- they cannot be used anywhere else!

From just where do the values for those two parameters come? They come from the values that the user gives when they call the function `add` (one more time, the arguments!).

```C++
int l = 5;
int r = 6;
int sum = add(l, r);
```

When a caller invokes `add`, the compiler makes space in memory for two new variables: the parameters of the `add` function. It _copies_ the value of `l` and `r` to the memory for the parameters `a` and `b`, respectively. After that, the compiler directs the computer to start executing the code in the body of the function. 

That the argument values are _copied_ to the parameters has tremendous consequences.

```C++
#include <iostream>

void add_one(int a) {
  a = a + 1;
  std::cout << "In function add_one, a is " << a << "\n";
}

int main() {
  int a = 1;
  add_one(a);
  std::cout << "In function main, a is " << a << "\\n";
}
```

The code above will print

```
In function add_one, a is 2
In function main, a is 1
```

Why? I mean, `add_one` modifies the value of `a` (by adding 1) and it even has the same name as the variable in `main`? So, what gives? The key is that the `a` in `add_one` (the *parameter* `a`) is a _copy_ of the value of `a` (in `main`, the argument) when `add_one` is invoked.

Copying the value of the argument into the value of the parameters (and keeping them separate -- even if they share the same name) is called passing arguments _by value_. By default, arguments in C++ are passed _by value_. Again, this is incredibly important.

## Return Statement for Control Flow

We hinted earlier that the return statement can also be used for control flow inside a function! What exactly does that mean? Well, it's simple: the first `return` statement that the function executes is the last statement executed by the function! It's definitely easier to realize the consequences of this statement if we look at an example.

```C++
int hello_goodbye(bool hello) {
  if (hello) {
    std::cout << "Hello!\n";
    return 5;
  }
  std::cout << "Goodbye!\n";
  return 4;
}

int main() {
  int hg = hello_goodbye(true);
  std::cout << "hg: " << hg << "\\n";
}
```

Because `hello_goodbye` is called with `true` as an argument, the `hello` parameter in the body of the `hello_goodbye` function is `true`. Therefore, the *then* branch of the `if` statement executes. That means that the function `hello_goodbye` will print `Hello!` to the screen and then return `5`. At that point, the function executes no further instructions! The value `5` is returned to the caller and `5` is assigned to the value `hg`. The value that is returned from a function is the value of the function call. 

The output is

```
Hello!
hg: 5
```

That's a really important point that we don't want to skip over! Anything that generates a value is known as an expression, remember? Well, in all functions that do *not* return `void`, the `return` statement in the function specifies to what the value of the function-call expression will evaluate.

If we call the function with the argument `false`, the output is

```
Goodbye!
hg: 4
```

What if we wanted to write the same function and get the same conditional output without returning a value? Can we use the return statement in a void function? Why yes we can (as we said above)!

```C++
void hello_goodbye(bool hello) {
  if (hello) {
    std::cout << "Hello!\n";
    return;
  }
  std::cout << "Goodbye!\n";
  return;
}

int main() {
  hello_goodbye(true);
}
```

This program will print:

```
Hello!

Very cool!
```

Using the `return` statement for control flow is powerful! Consider an example where you could have multiple exceptional conditions and only want to perform the action when they are all true.


```C++
void compare_file(std::string file1_name, std::string file2_name) {
  if (can_open(file1_name) && can_open(file2_name)) {
    _do work on files here_
    return;
  }
  std::cout << "Either file1 or file2 could not be opened!n";
}

int main() {
  std::string file_a_name, file_b_name;
  compare_file(file_a_name, file_b_name);
}
```

Our customer may add a further requirement that the failure to open either file be logged individually! Now we have to write this:

```C++
void compare_file(std::string file1_name, std::string file2_name) {
  if (can_open(file1_name)) { 
    if (can_open(file2_name)) {
      _do work on files here_
      return;
    } else {
      std::cout << "Could not open file2!n";
      return;
    }
  }
  std::cout << "Could not open file 1!n";
}

int main() {
  std::string file_a_name, file_b_name;
  compare_file(file_a_name, file_b_name);
}
```

Look at how we are doing the "real work" of the function nested all the way down in those `if` statements. That makes it harder for the reader of the function to determine where the real work is taking place. Let's use the `return` statement to make it more obvious what the real work of the function is:

```C++
void compare_file(std::string file1_name, std::string file2_name) {
  if (!can_open(file1_name)) {
    std::cout << "Could not open file 1!n";
    return;
  }
  if (!can_open(file2_name)) {
    std::cout << "Could not open file 2n";
    return;
  }
  // Do real work here
}
```

This is cool because we handle the possible exceptional cases at the top, once and for all. After we've dispatched with the exceptional conditions, we are free to do the real work of the function knowing that everything is normal! This is a very common pattern among professional developers and one that you should emulate!

## Local Variables

Like you can declare/define variables in the bodies of loops and conditional statements in C++, you can also declare/define variables in the bodies of functions! The simple rule that we used to determine when a scope start/stops is applicable to functions -- anywhere there is a `{`, a scope starts; anywhere there is a `}`, a scope ends. Because function bodies are enclosed in `{}`, the body of a function is its own scope.

Remember the term that we used to describe variables that are declared/defined in the same scope where they are used? That's right, _local_ variables. Functions have local variables.

Every time that execution of a program "enters" a scope, space in memory for local variables is created (allocated). Every time that execution of a program "exits" a scope, space in memory for local variables is destroyed. For this reason, the value of a local variable is reset every time the function executes. This is very important:

```C++
void increment() {
  int i = 0;
  i = i + 1;
  std::cout << "i: " << i << "\\n";
}

int main() {
  for (int i = 0; i<10; i++) {
    increment();
  }
}
```

will print

```
i: 1
i: 1
i: 1
i: 1
i: 1
i: 1
i: 1
i: 1
i: 1
i: 1
```

and _not_

```
i: 1
i: 2
i: 3
i: 4
i: 5
i: 6
i: 7
i: 8
i: 9
i: 10
```