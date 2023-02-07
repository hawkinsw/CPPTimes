### What's News

Despite advances in the understanding of quantum mechanics, researchers at the world's leading institutes for science have finally boiled down the essence of matter into one of two states. Read on to find out about their amazing discovery!

### Boolean

Booleans are a concept and a type in C++. Named after their famous discover, George Boole, Booleans (and their C++ type `bool`) are either _true_ (`true`) or _false_ (`false`). Booleans remind me of the phrase Yoda made famous: "Do or do not. There is no try." Booleans are similar: True or False. There is no maybe.

Like all _types_ in C++ we know, `bool`s have two characteristics:

1. A range of valid values; and
2. Valid operations on those values.

Like `int`s which can only hold whole numbers (positive and negative), `bool`s are limited in what they can hold. Valid values for `bool`s are `true` or `false`. That's it.

What is cool about George Boole's thinking was that he defined a set of operations that you can use to manipulate Booleans and get another one (we'll come back to those operators in a second). What's even more awesome is that those operators closely reflect how logic works in real life! 

So far we have learned that `bool`s can only hold `true` and `false`. We know that we have operators to combine two Booleans and get another. But, how do we get a Boolean value in the first place?

The answer: _relational_ and _equality_ operators! An expression composed of operands and a relational or equality operator has a value (just like all expressions!). And, like every value in C++, it has a type! The value can be either `true` or `false` and the type is `bool`. I am sure that you aren't surprised!

Let's look at the relational and equality operators in "C++ World" and how they compare to the ways that we write comparisons when we are in "Math World."

| Meaning | Math World | C++ World |
| --- | --- | --- |
| Less than | ![](https://uc.instructure.com/equation_images/%253C) | `<` |
| Less than or equal to | ![](https://uc.instructure.com/equation_images/%255Cle) | `<=` | 
| Greater than | ![](https://uc.instructure.com/equation_images/%253E) | `>` |
| Greater than or equal to | ![](https://uc.instructure.com/equation_images/%255Cge) | `>=` |
| Equal to | ![](https://uc.instructure.com/equation_images/%253D) | `==` |
| Not equal to | ![](https://uc.instructure.com/equation_images/%255Cne) | `!=` |

The first four are the _relational_ operators and the final two are the _equality_ operators.

Let's look at a few examples to make it clear:

```C++
#include <iostream>

int main() {
  bool result{5 < 10}; 
  return 0;
}
```

`result` is `true`!

```C++
#include <iostream>

int main() {
  bool result{10 <= 10};
  return 0;
}
```

`result` is `true`!

```C++
#include <iostream>

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


Now, there's something really philosophical about `bool` in C++. Like C++ is able to _coerce_ an `int` into a `double` (or vice versa), C++ can coerce values that are not `bool` into a `bool`. That's really powerful (as we'll see later!). Although the specifics are sometimes odd, the basic idea is that if you assign a value (from a type such as an `int` or a `double` or `char`) to a `bool` and that value is `0` (`0.0` or `'\0'`), then the `bool` is `false`. For _any other value_, the `bool` is `true`. For instance, 

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

Now, let's get back to how we can combine Boolean values and get new Boolean values. Like `+`, `-`, `*`, `/`, etc, there are a few primitive operators for `bool`s: `&&` (which means _and_), `||` (which means _or) and `!` (which means _not_). 

The first two (`&&` and `||`) are binary operators (which mean that they two two operands, like `+`, and `-` do). The `!` is a unary operator, meaning that it only takes a single operand.

Think about statements like:

> It is raining and it is snowing.

and

> It is raining or it is snowing.

Assume that we are in Cincinnati in June and I look outside. I see that it is raining, but there's no way that it's snowing. So, yes, it is _true_ that it is raining (we'll call that `raining` and say it is `true`) but, no, it is not snowing (we'll call that `snowing` and say that it is `false`).

The first statement is logically not true! So, we would say that it is `false`. The second statement, however, is `true`. So, in 

```C++
#include <iostream>

int main() {
  bool is_raining_and_snowing{raining && snowing};
  bool is_raining_or_snowing{raining || snowing};
}
```

`is_raining_and_snowing` is `false` and `is_raining_or_snowing` is `true`. 

In an `||` expression, if either operand is `true`, then the expression is `true`.  In an `&&` expression, if both operands are `true`, then the expression is `true`. Otherwise, the expressions are `false`. 

The `!` simply reverses the value of its (single) operand. For example,

There is a cool chart that we can write that describes completely the values for combinations of Boolean values using the _and_, _or_ and _not_ operators. The chart is called a truth table. Here is the truth table for the _and_ operator:

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

So, we can calculate `true` and `false` values, but what good are they? Read on to find out!

### It Takes `bool` to Tango: The `if` Statement

So far our programs have been pretty boring. They may have calculated different values based on user input, but they performed the same set of operations no matter what. In other words, until now our programs have operated _sequentially_. With an `if` statement we can get our programs to operate _selectively_. In other words, depending on a runtime calculation, our program can perform different operations! Although conceptually very simple, the power of this selective computation is infinite! The general form of an `if` statement is

<html>
<pre><font color="#FCE94F">if</font> (boolean expression) {
  some statements;
}   </pre>
</html>

When _boolean expression_ is `true`, then `some statements` execute. When _boolean expression_ is `false`, _some statements_ do not execute. _some statements_ between `{}` are called a _block_. You can nest blocks of code inside one another without limit (some caveats apply to that statement)! C++ programmers say that a block of code is conditionally executed by an `if` statement. C++ programmers also often say that a block of code is guarded by a conditional.

```C++
#include <iostream>

int main() {
  int five = 5;
  int six = 6;

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

Because `5` is less than `6`, the `std::cout` statement on line 8 executes and `Five is less than six!` is printed to the screen. However, the value of `five` does not equal the value of `six` so line 12 does not execute.

#### Be very careful: = and == are different!

C++ tries to be very accommodating and will attempt to convert a value of any type to a `bool` when it is used in the place of _boolean expression_ in an `if` statement. Remember, from earlier, how we talked about the ways that conversions happen?

More importantly, remember how we talked about the fact that many things in C++ are expressions that you would not immediately realize? 

Well, one of those hidden expressions in C++ is the `=` (assignment) operator! The value of expression `a = b` is the value of `b`! In other words,

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

If you look closely, you will see that where I put _boolean expression_ in the `if` statement there is a `=` and not a `==`. The expression evaluates to the value in `six`. Remember earlier how we learned that C++ interprets anything that is not a `bool` and `false` and is not another type whose value is something like `0` (etc) as true? Well, in the case shown above, C++ evaluates whether the value of the expression `five = six` is 0 (it's not, it's the value in `six`), determines that the value equals `true` and chooses to execute the block!

Be very careful! _You will make this mistake!_  _I promise!_