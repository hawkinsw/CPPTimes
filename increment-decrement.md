## What's News

With Hall of Fame voters split, one more (or fewer) vote for a player on this year's ballot could make the difference between their enshrinement in sports history or their agonizing through another round of debate over their stats.

## Just The Facts.

By now we are very familiar with the assignment statement:[^expression]

[^expression]: Or _expression_, depending upon your perspective.

```C++
int main() {
    int favorite_number{5};
    favorite_number = 6;
    return 0;
}
```

It is the tool that we can always use to change the value of a variable, no matter how we want to modify the variable's contents.

But, there are often times when we don't want to arbitrarily reset the value of a variable with a new value. We want to take the existing value of the variable, modify it by some amount, and then store it back into the variable.

For instance, depending on the configuration of a user interface, we may need to write code that converts between kilobytes and megabytes:[^bibibytes]

[^bibibytes]: Yes, we could have [divided](https://simple.wikipedia.org/wiki/Mebibyte) by 1024.


```C++
disk_usage = disk_usage / 1000;
```

But, that's lots of typing. What's more, it's a pattern that we see over and over again! So, C++ gives us the [_compound assignment operators_](./compoundassignment.md) to make our lives easier:

```C++
disk_usage /= 1000;
```

> If you would like to relive the excitement of learning about compound assignment operators, check out the back issue of the C++ Times on the [_compound assignment operator_](./compoundassignment.md)!

While we are thankful for C++ giving us such syntax so that we can keep our typing to a minimum, compound assignment operators are still too powerful. 

There are certain operations that are so simple that we almost take them for granted. They occur so frequently in our programs that it seems unnecessary to have to rely on such a configurable piece of syntax. 

We are talking about adding (or subtracting) one from a variable. Thank goodness that C++ gives us a cool shorthand.

## One (More) If By Land, One (Fewer) If By Sea

Adding one to (or subtracting one from) a variable is a _very_ common operation in programming. As an example, think about the code that you would have to write on a user's birthday:

```C++
age = age + 1;
```

That's lots of code for such a simple operation. We _could_ use compound assignment operators:

```C++
age += 1;
```

That is definitely better. But, C++ gives us yet another tool to make this frequently performed operation that much easier to type: the increment operator!

```C++
age++;
```

How cool is that? All of

```C++
age = age + 1;
```

```C++
age += 1;
```

```C++
age++;
```

have the same _side effect_.

For Benjamin Button, we may have to do a different operation on their birthday (a decrement):

```C++
age--;
```

How cool is that? All of

```C++
age = age - 1;
```

```C++
age -= 1;
```

```C++
age--;
```

have the same _side effect_.

## (Ex)Press Your Luck

> A side effect is an action that occurs over/above the operation specifically requested by the programming.

Why do we consider the fact that the value of `age` is updated when writing

```C++
age++;
```

or 


```C++
age--;
```

to be a side effect? Aren't we specifically requesting that update happen?

Yes, I'll admit, it does take a bit of sideways looking, but it will make more sense when you appreciate that those are _expressions_! An increment expression produces a value and has the side effect of updating the variable's value. We have seen how the side effect works for the increment and decrement operators, but what about the value?

Just what is the value of the expression

```C++
age++;
```

Let's assume that `age` has the value $43$ before the code is executed that the compiler generated for 

```C++
age++;
```

In that case, the value of the expression `age++` is $43$ -- the value of the variable `age` _before_ it was incremented! If the meaning of this expression depends so heavily on the fact that the increment comes _after_ the expression's value is calculated based on the variable's value, then you are probably guessing that there is a version that performs in the opposite order. You would be right!

```C++
++age;
```

has the same side effect as `age++;` but the value of the expression is the value of the variable `age` _after_ it was been incremented. 

In other words, 

| Operation | Order | Value | 
| -- | -- | -- |
| `age++` | `age` is updated _after_ expression's value is calculated | `age` |
| `++age` | `age` is updated _before_ expression's value is calculated | `age + 1` |

Notice the _before_ and _after_ and you can make out why the `++` _after_ the name of the variable is known as the _postfix_ increment (or decrement) operator. And, on the other side, the `++` before the name of the variable is known as the _prefix_ increment operator.

## Perfect Practice Makes Perfect

What does the following program print?

```C++
#include <iostream>

int main() {
    int a{5};
    std::cout << "a++: " << a++  << "\n";
    std::cout << "a: " << a  << "\n";
    return 0;
}
```

We know that the no matter whether you write a pre- or post-increment operator, the _next_ time you read the variable's value it will have increased by one. So, the second line of output is absolutely

```
a: 6
```

But, what about the first line? See that the expression is a post-increment operator? That means that the value is incremented _after_ the value of the expression is calculated. So, that means that the value of that expression is `5` and the program prints:


```
a++: 5
a: 6
```

You can play with that code online at [Godbolt](https://godbolt.org/z/EdKrrzvje).

What about the other direction?

```C++
#include <iostream>

int main() {
    int a{5};
    std::cout << "--a: " << --a  << "\n";
    std::cout << "a: " << a  << "\n";
    return 0;
}
```

Again, we know that the no matter whether you write a pre- or post-decrement operator, the _next_ time you read the variable's value it will have decreased by one. So, the second line of output is absolutely

```
a: 4
```

But, what about the first line? See that the expression is a pre-decrement operator? That means that the value is decremented _before_ the value of the expression is calculated. So, that means that the value of that expression is `4` and the program prints:

```
a++: 4
a: 4
```

You can play with that code online at [Godbolt](https://godbolt.org/z/3ve3a1vo9).

## With Great Power

Yes, the pre- and post-increment and decrement operators provide you with lots of power and give you the opportunity to write code that is tremendously succinct. However, these are also very easy to overuse. If you find yourself writing code that "cleverly" uses the difference between pre- and post-increment/decrement operators to perform an operation correctly, consider rewriting. It will be very difficult for you when you (or your colleague) comes back to the code a few days later and tries to figure out the meaning.

One of the most important things to remember when programming: Make it easy on your future self.



