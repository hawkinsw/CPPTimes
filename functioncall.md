## What's News

Union organizers at the Federation of Aligned C++ Programmers (FOACP), have demanded that more than of their statements work together in solidarity. The C++ language designers agreed to their negotiating demands and added a feature to the language.

## Functions

When statements in C++ program work together to accomplish a particular task that none of them could accomplish on their own, it makes sense to group those statements together in a unit and give them a name. We call the unit a _function_ and the name, if it's a good one, usually describes the action or task that those statements are performing.

Precisely, then, a _function_ is a combination of

1.  A set of statements
2.  A name for those statements.

Names of functions must follow the same rules as the names for variables.

Just like you have to declare a variable before you use it, programmers have to declare a function before they can use it. A function _declaration_ is a way to communicate to other programmers how to use a function.

Some functions calculate a _result_, some functions perform side effects (see below) and some do a combination of both. The use of a function that calculates a result (we use a function by _calling_ it) is like any ordinary expression.

One of the really cool things about functions is that we can use them and take advantage of their calculations _without having to know how they do the work_! For instance, although we have no idea how to perform number theoretic calculations, if another programmer defined a function that performed factorization of a very large number into two primes we could still use their work! (*PS*: If a programmer _did_ do that, they would be worth a ton of money!).

Working with something (like a function) without worrying about its details is called _abstraction_. To reiterate, abstraction is just a fancy term for hiding details. The person who wrote the function knows the details of how the function does its work but we, the users, do not. We (again, the users) only care about _what_ the function does and not _how_ it does it.

#### Functional Side Effects

Medicines have side effects that are physical effects on your body that are unrelated to the medicine's purpose. For instance, I may take medicine to control my headaches but it may occasionally make me dizzy. Function's can have side effects, too.

To understand side effects, we have to recognize that programs' have _state_ when they are executing. A program's _state_ at a certain point of execution consists of the value of all the variables in memory at the time. In other words, a program's state is a combination of
1. a point of execution in a program, and
2. the contents of the program's variables at that point.

Any change to the state of the program that may affect the user of the function is known as a _side effect_. When we learn about global variables and defining functions, we will see an actual example of a function that has a side effect -- it will be perfectly clear then.

In the meantime, know that side effects are bad!

#### Reading Function Declarations/Definitions

In "Math World" we could define a function that squares a number. We would write that like ![LaTeX: f\left(x\right)\:=\:x^2](https://uc.instructure.com/equation_images/f%255Cleft(x%255Cright)%255C%253A%253D%255C%253Ax%255E2 "f\left(x\right)\:=\:x^2"). The function _evaluates_ to the square of the value of x, _a parameter_. A _parameter_ is a variable defined by the function for use in the implementation. A parameter goes with the function declaration/definition.

Like ![LaTeX: f](https://uc.instructure.com/equation_images/f "f") (above) evaluates to the square of its parameter ![LaTeX: x](https://uc.instructure.com/equation_images/x "x"), functions in C++ evaluate to values, too.

We could declare/define the function ![LaTeX: f](https://uc.instructure.com/equation_images/f "f") in C++ like

<html><head></head><body><pre>
<font color=green>int</font> square(<font color=green>int</font> x);
</pre></body></html>


The function's name is `square`. Inside the `(` and `)` are the function's parameters. In this case, there is one parameter named `x` and its type is `int`. The `square` function _returns_ a value whose type is `int` (we can tell because of the `int` to the left of the function name). Programmers usually say this in slang like "The `square` function returns an `int`."

Here's another example:

<html><head></head><body><pre>
<font color=green>double</font> pow(<font color=green>double</font> base, <font color=green>double</font> exp);
</pre></body></html>

The function's name is `pow`. This function has two parameters and they are both `double`s. The `pow` function returns a value whose type is `double` (or "The `pow` function returns a `double`.").

#### Using Functions

In order to use a function, we have to _call_ it. We can call a function that returns a value anywhere that we could write an expression. Wait, that's cool: a function that returns a value is an expression just like an other expression! For example, we could assign the result of a call to `square` to a variable:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> square(<font color=green>int</font> x);
4 
5 <font color=green>int</font> main() {
6   <font color=green>int</font> two{<font color=red>2</font>};
7   <font color=green>int</font> square_of_two = square(two);
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>

In this call, we are passing an _argument_ of `two`. Arguments go with the caller of the function and pair up with parameters. In this case we are pairing the argument `two` with the parameter `x`. _Notice that the type of the argument and the type of the parameters have to match. This is very important!_ After this line of code executes, the variable `square_of_two` will hold the value `4`.

Or,

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>double</font> pow(<font color=green>double</font> base, <font color=green>double</font> exp);
4 
5 <font color=green>int</font> main() {
6   <font color=green>double</font> three{<font color=red>3</font>};
7   <font color=green>double</font> three_cubed = pow(three, three);
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>
In this call we are passing arguments of `three` and `three`. The first `three` is paired with the `base` parameter and the second `three` is paired with the `exp` parameter. After this line of code executes, the variable `three_cubed` will hold the value 27. But wait, 3 and 3 are `int`s and `base` and `exp` are `double`s. Didn't I just say that the types of the arguments must match the type of the parameters?! Yes, I did. In this case, C++ coerces the `int`s to the higher rank of `double` (just the way that it would when it coerces `2` to `2.0` in `3.0/2` -- what we learned last class!).

But, we can do more with function calls than just assign their results to values. We can use them as part of an expression that is assigned to another variable:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>double</font> pow(<font color=green>double</font> base, <font color=green>double</font> exp);
4 
5 <font color=green>int</font> main() {
6   <font color=green>double</font> three{<font color=red>3</font>};
7   <font color=green>double</font> v = <font color=red>8.0</font> + pow(three, three);
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>
After line 7 executes, the variable `v` will hold the value `35`. We can also use them in `std::cout` statements:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>double</font> pow(<font color=green>double</font> base, <font color=green>double</font> exp);
4 
5 <font color=green>int</font> main() {
6   <font color=green>double</font> three{<font color=red>3</font>};
7   std::cout << <font color=red>"Three cubed: "</font> << pow(three, three) << <font color=red>"\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>

which prints

```
Three cubed: 27
```

[Solidarity](https://en.wikipedia.org/wiki/Solidarity_(Polish_trade_union).