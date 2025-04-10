## What's News

After conquering the vagaries of quantum mechanics, physicists turned their attention to completely deciphering the behavior of mathematical operations in C++ programs. The years-long effort resulted in today's announcement from the C++ Elucidation Research Network (CERN) that they have finally broken the code.

## Integer Division

Though computers are able to accomplish a tremendous amount of work in a short amount of time and regularly put their human computing counterparts to shame (after all, can you multiply 549872 times 86384572 in your head faster than a computer?), in some ways humans remain superior.

For instance, consider the mathematical equation

$$ 5 / 2 $$

The answer is $2.5$, obviously!

Let's see what that too-cool-for-school computer says:

```C++
#include <iostream>

int main() {

  int five{5};
  int two{2};
  std::cout << "5/2: " << (five / two) << "\n";
  return 0;
}
```

will print

```
5/2: 2
```

Uhm, what?

Just when you thought that you could escape the clutches of types in programming, they pull you right back in! The answer to this mystery lies in types.

Remember that _expressions_ are anything that has a value. More importantly (for this discussion), all values in C++ have a type! Never forget the definition of _type_: A range of valid values and the valid operations that you can perform on those values.

Put that knowledge together and you can reason that `(five/two)` is ... 

- an expression, it generates a value and,
- therefore, has a type.

C++ determines the type of a value based on the composition of the expression that generates it! It just so happens that C++ specifies that when you have an operator (remember the definition of operator?) whose operands are `int`s, the type of the value when that expression is evaluated is also an `int`. 

Ah, so that's the rub!

Dividing two `int`s always yields an `int` and an `int` cannot hold numbers with decimal points! So, when a C++ program generates a value for an expression whose type is `int` but the value has numbers after the decimal point, what does it do? Does it round the number up? Nope! It simply lopes off that extra information in a process called _truncation_. 

So, how can you get around this obnoxious behavior and teach the computer to give the answer that you really want? 

## Operators With Operands of Mixed Types

C++ cannot evaluate an expression when the two operands of an operator have different types. So, when C++ sees an operator whose operands have different types, it goes through a process of attempting to coerce the two operands to the same type!

The rules and mechanics of this process are fairly complex (and it is best to consult a [reference](https://en.cppreference.com/w/cpp/language/implicit_conversion) any time you rely on that behavior), but there is one that is easy to remember and very handy:

> When C++ sees an operator whose operands are an `int` and a `double`, the `int` operand is "upgraded" to a `double`.

With that upgrade, the two operands are both `double`s and as a result (obviously) the, er, result of that operation also has the type `double`. Exactly what we want.

So, all that's left is to convince C++ that either `five` or `two` is really a `double`. Because whole numbers (`int`s) are a subset of all numbers, we can assure C++ that it is okay to think about either `five` or `two` as a `double` without losing any information. In other words, if C++ considers either `five` or `two` as a double, none of the meaning of $5$ or $2$ will be lost! 

***N.B.***: This is not always 100% guaranteed to be the case. There is the possibility that you could store a very, very large whole number and fail to be able to convert it to a double-precision number. But, most of the time it works just fine!

***N.B. (2)***: This assumption _definitely_ does not hold in the opposite direction -- if you think about $5.2$ as an `int`, it will lose some of its essence. You will only be able to think about $5$ -- where did the rest of our value go? Remember _truncation_?

Okay, now for the arm twisting: C++ gives us an awesome tool known as the `static_cast` that we can use to assure the compiler that we can treat a variable of one type as a variable of a different type. Of course there are rules about when it is safe to do this type of operation -- we don't want really want C++ to allow us to tell it that a `std::string` is a `bool`. 

```C++
#include <iostream>

int main() {

  int five{5};
  int two{2};
  std::cout << "5/2: " << (static_cast<double>(five) / two) << "\n";
  return 0;
}
```

will print

```
5/2: 2.5
```

Yes, problem solved!

In general, the syntax for a `static_cast` looks like

```C++
static_cast<new_type>(expression)
```

when you want to tell C++ to _interpret_ the type of `expression` as `new_type`. Note how we used the word _interpret_. A `static_cast` is itself an expression -- in other words, it has a type and a value. The type of the value of that expression is `new_type` and the value is the value of `expression` as that type. _A `static_cast` does not change anything about the value or type of `expression`._

One final important caveat about `static_cast`. The `expression` is evaluated _before_ the `static_cast` occurs. Why is that important? 

Because we have to be _very_ careful about how we apply the `static_cast` if we want to get the proper behavior!

Consider this fun(ny) example: What is the output of the following program?

```C++
#include <iostream>

int main() {

  int five{5};
  int two{2};
  std::cout << "5/2: " << static_cast<double>(five / two) << "\n";
  return 0;
}
```

```
5/2: 2
```

What? Think about it! The first thing that C++ does is evaluate the expression `5/2`. Those are two `int`s and, well, we are right back where we started. The value of that expression is $2$ and it's type is `int`! It is only then (after all the evaluation of that expression is completed) that the `static_cast` does its job and tells C++ to interpret the `int` $2$ as a `double`.

Be very careful!