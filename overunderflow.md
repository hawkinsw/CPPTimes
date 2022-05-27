## What's News

In today's news, researchers uncover what happens when people take the advice to "Go big or go home" too seriously. 

## Overflow and Underflow

Remember the definition of _type:_ the range of valid values of a variable and the valid operations on that variable. For variables that represent numbers, what happens when you store a number into a variable that it cannot represent? In cases where the value is too big, you get _overflow_. In cases where the value is too small, you get _underflow_. For example, what is the output of this program?

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   <font color=green>int</font> integer_variable{<font color=red>2147483647</font>};
5   std::cout << <font color=red>"integer_variable: "</font> << integer_variable << <font color=red>"\n"</font>;
6   integer_variable += <font color=red>1</font>;
7   std::cout << <font color=red>"integer_variable: "</font> << integer_variable << <font color=red>"\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>

```
integer_variable: 2147483647
integer_variable: -2147483648
```

It just so happens that on the computer where we ran this program, 2147483647 is the largest value that can be stored in an `int`. Therefore, attempting to store a number bigger than that (+1) results in overflow and the value wraps around to become a negative number. You can try this program live at [Godbolt](https://godbolt.org/z/asMEr4h8W).

This can also happen in the opposite direction. What is the output of this program?

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   <font color=green>int</font> integer_variable{-2147483648};
5   std::cout << <font color=red>"integer_variable: "</font> << integer_variable << <font color=red>"\n"</font>;
6   integer_variable -= <font color=red>1</font>;
7   std::cout << <font color=red>"integer_variable: "</font> << integer_variable << <font color=red>"\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>

```
integer_variable: -2147483648
integer_variable: 2147483647
```

You can try this program live at [Godbolt](https://godbolt.org/z/YMqMezE6b).

These type of "anomalies" are hard to detect and end up being the root cause of many, many security vulnerabilities and hard-to-spot bugs. Be sure to always be on the lookout for what might happen if if you program has to deal with data that stretch the limits of what your variable's types are designed to hold!

### Functions

A _function_ is a combination of

1.  A set of statements
2.  A name for those statements.

In a good function, the statements cooperate to accomplish a particular task and the name evokes that task. For instance, a function named `add_five` would refer to a set of statements that work together to "add 5" and not "add six".

Names of functions must follow the same rules as the names for variables.

Just like you declare a variable before you use it, programmers have to declare a function before they can use it. A function _declaration_ is a way to communicate to other programmers how to use a function.

Some functions calculate a _result_, some functions perform side effects (see below) and some do a combination of both. When a function calculates a result, the use of that function is like any ordinary expression.

A function is a form of _abstraction_, a way of hiding details. The person writing the function knows the details of how the function does its work. The person using the function does not. The person using the function cares about _what_ the function does and not _how_ it does it.

#### Functional Side Effects

Medicines have side effects that are physical effects on your body that are unrelated to the medicine's purpose. For instance, I may take medicine to control my headaches but it may occassionally make me dizzy. Function's can have side effects, too.

To understand side effects, we have to recognize that programs' have _state_ when they are executing. A program's _state_ at a certain point of execution consists of the value of all the variables in memory at the time. In other words, a program's state is a combination of a point of execution in a program and the contents of the variables at that point.

Any change to the state of the program that the user of the function can see as a result of using the function is known as a side effect. When we learn about global variables and defining functions, we will see an actual example of a function that has a side effect -- it will be perfectly clear then.

In the meantime, know that side effects are bad!

#### Reading Function Declarations/Definitions

In "Math World" we could define a function that squares a number. We would write that like ![LaTeX: f\left(x\right)\:=\:x^2](https://uc.instructure.com/equation_images/f%255Cleft(x%255Cright)%255C%253A%253D%255C%253Ax%255E2 "f\left(x\right)\:=\:x^2"). The function _evaluates_ to the square of the value of x, _a parameter_. A _parameter_ is a variable defined by the function for use in the implementation. A parameter goes with the function declaration/definition.

Like ![LaTeX: f](https://uc.instructure.com/equation_images/f "f") (above) evaluates to the square of its parameter ![LaTeX: x](https://uc.instructure.com/equation_images/x "x"), functions in C++ evaluate to values, too.

We could declare/define the function ![LaTeX: f](https://uc.instructure.com/equation_images/f "f") in C++ like

int square(int x);

The function's name is `square`. Inside the ()s are the function's parameters. In this case, there is one parameter named x and its type is int. The square function _returns_ a value whose type is int (we can tell because of the int to the left of the function name). Programmers usually say this in slang like "The square function returns an int."

Here's another example:

double pow(double base, double exp);

The function's name is `pow`. This function has two parameters and they are both `double`s. The `pow` function returns a value whose type is `double` (or "The square function returns a double").

#### Using Functions

In order to use a function, we have to _call_ it. We can call a function that returns a value anywhere that we could write an expression. Again, a function that returns a value is an expression! For example, we could assign the result of a call to `square` to a variable:

int two\_squared = square(2);

In this call, we are passing an _argument_ of `2`. Arguments go with the caller of the function and pair up with parameters. In this case we are pairing the argument `2` with the parameter `x`. _Notice that the type of the argument and the type of the parameters have to match. This is very important!_ After this line of code executes, the variable `two_squared` will hold the value `4`.

Or,

double three\_cubed = pow(3, 3);

In this call we are passing arguments of `3` and `3`. The first `3` is paired with the `base` parameter and the second `3` is paired with the `exp` parameter. After this line of code executes, the variable `three_cubed` will hold the value 27. But wait, 3 and 3 are `int`s and `base` and `exp` are `double`s. Didn't I just say that the types of the arguments must match the type of the parameters?! Yes, I did. In this case, C++ coerces the `int`s to the higher rank of `double` (just the way that it would when it coerces `2` to `2.0` in `3.0/2` -- what we learned last class!).

But, we can do more with function calls than just assign their results to values. We can use them as part of an expression that is assigned to another variable:

double v = 8.0 + pow(3, 3);

After this line of code executes, the variable v will hold the value `35`. We can also use them in `std::cout` statements:

std::cout << "Result: " << pow(3,3) << "\\n";

which prints

Result: 27