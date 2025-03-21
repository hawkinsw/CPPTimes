### What's News

Despite advances in the understanding of quantum mechanics, researchers at the world's leading institutes for science have finally boiled down the essence of matter into one of two states. Read on to find out about their amazing discovery!

### Boolean

Booleans are a conceptual thing and a technical thing in C++. Named after their famous discover, George Boole, Booleans (and their C++ type `bool`) are either _true_ (`true`) or _false_ (`false`). Booleans remind me of the phrase Yoda made famous: "Do or do not. There is no try." Booleans are similar: True or False. There is no maybe.

Like all _types_ in C++, `bool`s have two characteristics:

1. A range of valid values; and
2. Valid operations on those values.

Like `int`s which can only hold whole numbers (positive and negative), `bool`s are limited in what they can hold. Valid values for `bool`s are `true` or `false`. That's it.

What is cool about George Boole's thinking was that he defined a set of operations that you can use to manipulate Booleans and get another one (we'll come back to those operations in a second). What's even more awesome is that those operations closely reflect how logic works in real life! 

> Note: Remember that in C++ we use _operators_ to specify an operation to perform.

So far we have learned that variables with the type `bool` can only hold the values `true` and `false`. We also know that we have operations that combine two values with Boolean type and get another. But, how do we get a value with a Boolean type in the first place?

There are a number of ways, but the best way[^1] is through _relational_ and _equality_ operators! An expression composed of operands and a relational or equality operator has a value (just like all expressions!). And, like every value in C++, it has a type! The value can be either `true` or `false` and the type is `bool`. I am sure that you aren't surprised!

[^1]: Just this person's opinion, of course.

Let's look at the relational and equality operators in "C++ World" and how they compare to the ways that we write comparisons when we are in "Math World."

| Meaning | Math World | C++ World |
| --- | --- | --- |
| Less than | $\lt$ | `<` |
| Less than or equal to | $\leq$ | `<=` | 
| Greater than | $\gt$ | `>` |
| Greater than or equal to | $\geq$ | `>=` |
| Equal to | $=$ | `==` |
| Not equal to | $\ne$ | `!=` |

The first four are the _relational_ operators and the final two are the _equality_ operators.

Let's look at a few examples to make it clear:

```C++
int main() {
  bool result{5 < 10}; 
  return 0;
}
```

`result` is `true`!

```C++
int main() {
  bool result{10 <= 10};
  return 0;
}
```

`result` is `true`!

```C++
int main() {
  bool result{10 >= 10};
  return 0;
}
```

`result` is `true`!

```C++
int main() {
  bool result{10 == 10};
  return 0;
}
```
`result` is `true`!

```C++
int main() {
  bool result{10 != 10};
  return 0;
}
```

`result` is `false`!


Now, there's something really philosophical about `bool` in C++. Like C++ is able to _coerce_ an `int` into a `double` (or vice versa -- what C++ calls an [_implicit conversion_](https://en.cppreference.com/w/cpp/language/implicit_conversion)), C++ can coerce values that are not `bool` into a `bool`. That's really powerful (as we'll see later!). Although the specifics are sometimes odd, the basic idea is that if you assign a value (from a "source" with type such as an `int` or a `double` or `char`) to something that can store a value of type `bool` (the "destination") and the source value is `0` (`0.0` or `'\0'`), then the value of destination  `bool` is `false`. For _any other value_ of the source, the value of the destination is `true`. For instance, 

```C++
#include <iostream>

int main() {
  int zero{0};
  double zero_point_zero{0.0};
  char null_character{'\0'};
  char letter_a{'a'};
  int five_hundred_one{501};
  double pi{3.14};

  bool zero_bool{zero};
  bool zero_point_zero_bool{zero_point_zero};
  bool null_character_bool{null_character};
  bool letter_a_bool{letter_a};
  bool five_hundred_one_bool{five_hundred_one};
  bool pi_bool{pi};

  return 0;
}
```

`zero_bool`, `zero_point_zero_bool`, and `null_character_bool` are all `false`! The others are `true`.

#### Crossing the Streams

Now, let's get back to how we can combine values with Boolean types and get new values with Boolean types. Like `+`, `-`, `*`, `/`, etc. are primitive operations for numbers (`int`egers and `double`s), there are a few primitive operators for `bool`s: `&&` (which means _and_), `||` (which means _or_) and `!` (which means _not_). 

The first two (`&&` and `||`) are binary operators (which mean that they take two operands, like `+`, and `-` do). The `!` is a unary operator, meaning that it only takes a single operand.

Think about statements like:

> It is raining and it is snowing.

and

> It is raining or it is snowing.

Assume that we are in Cincinnati in June and I look outside. I see that it is raining, but there's no way that it's snowing. So, yes, it is _true_ that it is raining (we'll call that `raining` and say it is `true`) but, no, it is not snowing (we'll call that `snowing` and say that it is `false`).

The first statement is logically not true -- it is not both raining and snowing! So, we would say that it is `false`. On the other hand, the second statement, according to the Cincinnati Weather scenario defined above, is `true`. The same is true for the program below that uses C++ to encode the same scenario:

```C++
int main() {
  bool raining{true};
  bool snowing{false};
  bool is_raining_and_snowing{raining && snowing};
  bool is_raining_or_snowing{raining || snowing};
}
```

`is_raining_and_snowing` is `false` and `is_raining_or_snowing` is `true`. 

In an `||` expression, if the Boolean value of either operand is `true` (after being coerced [a.k.a., implicitly converted]), then the Boolean value of the expression result is `true`. In an `||` expression, if both of the operands are `false` (again, after implicit conversion), then the value of the expression is `false`. In an `&&` expression, if both operands are `true`, then the expression is `true`. Otherwise, the expressions are `false`. 

The `!` simply reverses the value of its (single) operand.

There is a cool chart that we can write that describes completely the values for combinations of Boolean values using the _and_, _or_ and _not_ operators. The chart is called a _truth table_. Here is the truth table for the _and_ operator:

| _a_ | _b_ | _a_ `&&` _b_ |
| -- | -- | -- |
| `true` | `true` | `true` |
| `true` | `false` | `false` |
| `false` | `true` | `false` |
| `false` | `false` | `false` |

Notice that the only combination that is `true` is when both _a_ and _b_ are `true`. Interesting!

Now, here is the truth table for the _or_ operator:

| _a_ | _b_ | _a_ `\|\|` _b_ |
| -- | -- | -- |
| `true` | `true` | `true` |
| `true` | `false` | `true` |
| `false` | `true` | `true` |
| `false` | `false` | `false` |

Notice that the only combination that is `true` is when one of _a_ and _b_ are `true`. Doubly interesting!

The _not_ operator is different than the _and_ and the _or_ operators. The _not_ operator needs only one operand (a so-called _unary_ operator). Why is that? Well, the _not_ operator simply reverses the value of a Boolean. So, it would make sense that it would only work on one operator. Here's the truth table for the _not_ operator:

| _a_ | `!` _a_ |
| -- | -- |
| `true` | `false` |
| `false` | `true` |

Let's play computer and try to figure out the value of `no` in the following C++ code:

```C++
#include <iostream>

int main() {
  bool yes{true};
  bool no{!yes};
  return 0;
}
```

`no` is `false`. To compute that, just replace `yes` with its value (`bool no{!true}`), flip `true` to `false` because of the `!` and you get `bool no{false};`. Pretty cool!

So, we can calculate `true` and `false` values, but what good are they? _If_ you want to know the answer, read on!

### It Takes `bool` to Tango: The `if` Statement

To date, the programs that we have written in class and in lab have been pretty straightforward, as in their execution proceeds _straight_ from one statement to the next. They may have calculated different values each time they executed based on user input, but they performed the same statements in the same order no matter what. In other words, until now our programs have operated _sequentially_. With an `if` statement we can get our programs to operate _selectively_. In other words, depending on a runtime calculation, our program can execute different statements! Although conceptually very simple, the power of this selective execution is infinite! The general form of an `if` statement is

```C++
if (expression) {
  some statements;
} 
```
When the value of _expression_ is `true` (again, after conversion to `bool`), then `some statements` execute. When the value of _expression_ is `false`, _some statements_ do not execute. _some statements_ between `{}` are called a _block_. You can nest blocks of code inside one another without limit (some caveats apply to that statement)! C++ programmers say that a block of code is "conditionally executed" by an `if` statement. C++ programmers also often say that a block of code is "guarded by a conditional".

```C++
#include <iostream>

int main() {
  int five{5};
  int six{6};

  if (5 < 6) {
    std::cout << "Five is less than 6!\n";
  }

  if (five == six) {
    std::cout << "Huh, five is equal to 6!\n";
  }
  return 0;
}
```
prints

```
Five is less than 6!
```

Because `5` is less than `6`, the `std::cout` statement inside the block associated with the `5 < 6` condition executes and `Five is less than six!` is printed to the screen. However, the value of `five` does not equal the value of `six` so the `std::cout` inside the block associated with the `five == six` condition does not execute.

![Which blocks of code execute depends on the value of the expressions that _guard_ those blocks.](./graphics/FiveEqualsSixSelectiveExecutionLabeled.png)

#### Be very careful: = and == are different!

C++ tries to be very accommodating and will attempt to convert a value of any type to a `bool` when it is used in the place of _boolean expression_ in an `if` statement. Remember, from earlier, how we talked about the ways that conversions happen?

More importantly, remember how we talked about the fact that many things in C++ are expressions that you would not immediately realize? 

Well, one of those hidden expressions in C++ is the `=` (assignment) operator! Yes, an assignment statement like `a = b` is, in fact, an expression and its value is the value of `b`! In other words,

```C++
#include <iostream>

int main() {
  int a{5};
  int b{6};

  std::cout << "a = b: " << (a = b) << "\n";
  return 0;
}
```

prints

```
a = b: 6
```

I know, right?! These semantics make it possible to do things like

`a = b = c = d = 5;`

to set `a`, `b`, `c` and `d` all to the value `5`.

Now, think closely and figure out why

```C++
#include <iostream>

int main() {
  int five{5};
  int six{6};

  if (five = six) {
    std::cout << "Huh, five is equal to 6!\n";
  }

  return 0;
}
```

prints

```
Huh, five is equal to 6!
```

If you look closely, you will see that the expression whose value guards the `if` statement contains a `=` and not a `==`. Because of that _minor_ typo, the expression evaluates to the value in `six`. Remember earlier how we learned that C++ interprets anything that is not a `bool` and is not another type whose value is something like `0` (etc.) as `true`? Well, in the case shown above, C++ evaluates whether the value of the expression `five = six` is 0 (it's not, it's the value in `six`), determines that the value equals `true` and chooses to execute the block!

Be very careful! _You will make this mistake!_  _I promise!_