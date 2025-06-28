## What's News
First-time homeowners and retirees historically believed that purchasing a condo or town house was a financially responsible decision. For first-time homeowners, it was considered a way to start up the real-estate latter. For retirees, it was a way to save money when extra space was no longer needed. But, these days, condo and homeowners fees and [_initiation_ dues](https://www.hoaleader.com/public/HOA-Wants-Charge-Initiation-Fee-This-Semantics-or-New-Fee.cfm) are making these purchases incredibly [costly](https://www.bloomberg.com/news/articles/2021-09-25/u-s-condo-fees-surge-19-as-energy-wage-bills-pile-up).

## Starting Out On The Right Foot

They[^who] say that first impressions have a long-lasting affect on what you think about a person. They also say you never get a second chance to make a good first impression! As in life, so it is with C++.

As we discussed [previously](./variables-intro-naming.md), you introduce[^pun] variables (any _name_, really!) into your C++ programs with a declaration. In fact, that's all a declaration is: a way to introduce a name into [a part](./ScopeSupplement.md) of a program. Programmers love to name things and we will see in this course all the different things in our programs that we get to christen. 

We discussed how declarations and definitions _can_ be separate. But, almost as soon as we learned that there is the possibility of declaring and defining variables in separate places in our programs, we realized that _variable declaration and variable definition usually_ happen at the same time.

Once we have declared and defined a variable, the C++ compiler knows that variable has certain properties (among them, the _type_ of the values that it can hold). The compiler uses those properties when converting your high-level source code into low-level code to detect errors and build an executable program that is as efficient as possible. As an example, consider the following declaration/definition of a meteorological variable `sunny_outside`:

```C++
bool sunny_outside;
```

The compiler's job is never done, though -- especially when it comes to handling the variables that you have declared/defined. As we just described, the compiler uses declarations/definitions when it is building your program to check for _type_(and other compile-time errors) _errors_. 

But, it does something else, too! Think of the compiler as like a really, really loyal dog: They do what you ask _and_ so much else besides! To keep up the terrible analogy a little longer, when it comes to how it simplifies our use of variables, _A Programmer's Best Friend_ is a lifesaver: The compiler generates code on our behalf that will make sure that there is space in the computer's memory for the value of our variable for as long as it is necessary! And then, when we are done with that variable, the compiler will generate code that cleans up that space _automatic_ally.[^almost] That deserves a big Scooby Snack!

## Neither a Magician Nor a Mindreader

Even though the compiler is pretty awesome, it's not David Copperfield or Harry Houdini. In particular, it can't decide for _you_ the value the variable stores. _You_ are responsible for the values that variables store.

It goes without saying that the assignment operator is one of the best tools for living up to our responsibility:

```C++
sunny_outside = false;
```

The statement above sets the value of the variable named `sunny_outside` to `false`. And, because variables are, well, variable, we can change the value of variables (as long as they are not [constant variables](./variables-intro-naming.md)) using the assignment operator.

Normally, when we live up to _our_ responsibility to tend to the values that are stored in variables, then reading the value of a variable (by using the name of the variable -- that is an [_expression_](./expressions-types.md), after all!) works without a problem:

```C++
int days_until_vacation;
days_until_vacation = 2;
std::cout << "There are " << days_until_vacation << " days until we leave for our vacation!\n";
```

will print

```console
There are 2 days until we leave for our vacation!\n";
```

But what would happen if our travel agent was a little, well, delayed:

```C++
int days_until_vacation;
std::cout << "There are " << days_until_vacation << " days until we leave for our vacation!\n";
days_until_vacation = 2;
```

See what I see? We are reading the value of a variable after the declaration/definition (that's a good start -- the compiler will make sure that the variable has space to store the value when the program is running) but _before_ we have given it any value! When the `std::cout` statement executes, the flight attendant has not yet updated our ETA in Bermuda. 

In such a situation, the value of the variable has an _indeterminate_ value. That means you cannot make any guarantees about it. In fact, if you write an expression that evaluates to an indeterminate value (e.g., use a variable with an indeterminate value in an expression), then you have just introduced undefined behavior into your program. 

Depending on the compiler (and the way that it was written), the code generated for this poorly written program means that the application could produce different output every time that it is executed[^indeterminate]. Don't believe me? Play with it on [Godbolt](https://godbolt.org/z/PTK98Efzr).

[^indeterminate]: Because compilers are notoriously good at being overly cautious and clearing the indeterminate values if you don't, you may not get a different value on every execution of a program that uses an indeterminate value. But, there are downsides to the compilers zealous protection of your code. Can you think of any? 

So, to be clear: Reading the value of a variable that has _not_ been given a value is an error (although one that the compiler cannot very easily catch).

## What's a Compiler To Do?

Although the compiler is _not_ a magician and it couldn't read our mind and determine that we actually expected our vacation start in a few days, maybe it just assumed that the declaration/definition

```C++
int days_until_vacation;
```

meant we wanted the default value for `days_until_vacation`, which is `-1` for an `int`.[^not] Oh, you _don't_ think that `-1` is a good default value for `int`-typed variables? As you grow as computer scientists, you will realize that there is one thing that we hate more than anything: surprises. 

[^not]: `-1` is _absolutely not_ the default value for an `int`-typed variable. I was just trying to make a point!

There is _no way_ that the people who design C++ could come to a consensus on non-zero default values that will _not_ be a surprise to _some_ programmer! On the other hand, there _are_ good _zero_ default values -- more on that in a second.

So, when the compiler sees a declaration/definition like

```C++
double wind_speed;
```

it really is in a no-win situation. Choose a default and _someone_ will be mad. Don't do anything and the memory will have some indeterminate value that will be funny when it is read.

The compiler needs our help ... and fast. How can we rescue it? Easy: Always _initialize_ variables when you declare/define them.

> Note: Yes, of course there are situations where you want to declare/define a variable without giving it an initial value. And, when that is the case, you will _know_ it. Until then, ...

For fundamental types (and it's even a good idea for compound types, too), there's [_uniform initialization_](https://isocpp.org/wiki/faq/cpp11-language#uniform-init) which requires the use of `{}` after the name of the variable in the declaration/definition. For example, if you wanted to make sure the day was calm, 

```C++
double wind_speed{};
```

would declare, define and give an initial value to the `wind_speed` variable (meaning it is safe to read the variable's value immediately after declaration definition).

How could we fix our absent-minded AAA agent from earlier:

```C++
int days_until_vacation;
std::cout << "There are " << days_until_vacation << " days until we leave for our vacation!\n";
days_until_vacation = 2;
```

We could add uniform initialization:
```C++
int days_until_vacation{};
std::cout << "There are " << days_until_vacation << " days until we leave for our vacation!\n";
days_until_vacation = 2;
```

while the output won't be correct, it will be _reasonably_ wrong and not random.

When using uniform initialization like we did above, we are asking the compiler to generate code that will guarantee that the variable has _zero value_ of it's type as it's initial value. Here are some examples of the _zero value_ of common fundamental types:

| Type | Zero Value |
| -- | -- |
| `bool` | `false` |
| `int` | `0` |
| `double` | `0.0` |
| `char` | [`'\0'`](https://eel.is/c++draft/conv#integral-3) |

What's even better than the ability to use uniform initialization to fix bugs like the ones we saw above is that we can use the same syntax to give a variable an initial value of our choosing. For instance, if we wanted to age in reverse we would need to start in old age:

```C++
int age{92};
```

declares/defines an `int`-typed variable named `age` that has the initial value of `92`. How cool! 

But wait, there's more: Uniform initialization helps alert you to the fact that you might have given a variable an initial value that loses "information". Wait, what? Remember the issue that we discussed with [integer division](./integerdivision.md)? When we thought there should be fractions in a result of an operation but C++ treats the result as a whole number, we "lose" that fraction part (aka "information").

If you wanted to track that $\frac{1}{2}$ cup of water that you drank, you might have written:

```C++
int water_cups_per_day = 13.5;
```

But, if you looked at the value of `water_cups_per_day` after that declaration/definition/initialization, you would see that it's value is `13`! Wouldn't it have been nice if the compiler alerted us to this mistake?! If we rewrote that code using uniform initialization:

```C++
int water_cups_per_day{13.5};
```

the compiler would raise an error and tell us that we are trying to put a value with one type into a variable that expects to store values of a different type and that there might data lost in the process.

Pretty powerful!


[^who]: Who are "they", really?
[^pun]: Get it?
[^almost]: There are some nuances to rather broad statement that I just made -- we will learn about those [later in the course](./variables-memory.md).