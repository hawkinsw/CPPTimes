## What's News

After conquering the vagaries of quantum mechanics, physicists turned their attention to completely deciphering the behavior of mathematical operations in C++ programs. The years-long effort resulted in today's announcement from C++ Elucidation Research Network (CERN) that they have finally broken the code.

## Integer Division

Though computers are able to accomplish a tremendous amount of work in a short amount of time and regularly put their human computing counterparts to shame (after all, who can multiply 549872 times 86384572 in their head faster than a computer?), in some ways humans remain superior.

For instance, consider the mathematical equation

$ 5 / 2 $

The answer is $2.5$, obviously!

Let's see what that too-cool-for-school computer says:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4 
5   <font color=green>int</font> five{<font color=red>5</font>};
6   <font color=green>int</font> two{<font color=red>2</font>};
7   std::cout << <font color=red>"5/2: "</font> << (five / two) << <font color=red>"\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>

```
5/2: 2
```

Uhm, what?

Just when you thought that you could get away from clutches of types in programming, they pull you right back in! The answer to this mystery lies in types.

As you recall from previous discussions, _expressions_ are anything that has a value. More importantly (for this discussion), all values in C++ have a type! Never forget the definition of _type_: A range of valid values and the valid operations that you can perform on those values.

All that said, `(five/two)` is an expression and it generates a value. Therefore, that value has a type. C++ determines the type of a value based on the composition of the expression that generates it! It just so happens that C++ specifies that when you have an operator (remember the definition?) whose operands are `int`s, the type of the value when that expression is evaluated is also an `int`. 

Ah, so that's the rub!

Dividing two `int`s always yields an `int` and an `int` cannot hold numbers with decimal points! So, when a C++ program generates a value for an expression whose type is `int` but the value has numbers after the decimal point, what does it do? Does it round the number up? Nope! It simply lopes off that extra information in a process called _truncation_. 

So, how can you get around this obnoxious behavior and teach the computer to give the answer that you really want? 

## Operators With Operands of Mixed Types

C++ cannot evaluate an expression when there the two operands of an operator have different types. So, when C++ sees an operator whose operands have different types, it goes through a process of attempting to coerce the two operands to the same type!

The rules and mechanics of this process are fairly complex (and it is best to consult a [reference](https://en.cppreference.com/w/cpp/language/implicit_conversion) any time you rely on that behavior), but there is one that is easy to remember and very handy:

> When C++ sees an operator whose operands are an `int` and a `double`, the operator with the `int` operand is "upgraded" to a `double`.

With that upgrade, the two operands are both `double`s and as a result (obviously) the, er, result of that operation also has the type `double`. Exactly what we want.

So, all that's left is to convince C++ that either `five` or `two` is really a double. Because whole numbers (`int`s) are a subset of all numbers, we can assure C++ that it is okay to think about either `five` or `two` as a `double` without losing any information. In other words, if C++ considers either `five` or `two` as a double, none of the meaning of $5$ or $2$ will be modified! 

***N.B.***: This assumption does not hold in the opposite direction -- if you store $5.2$ in an `int` the meaning of $5.2$ will change! The variable will only store the $5$ -- where did the rest of our value go? Remember _truncation_?

C++ gives us an awesome tool known as the `static_cast` that we can use to assure it that we can treat of a variable of one type as a variable of a different type. Of course there are rules about when it is safe to do this type of operation. Obviously we don't want to be able to tell C++ that a `std::string` is a `bool`. 

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4 
5   <font color=green>int</font> five{<font color=red>5</font>};
6   <font color=green>int</font> two{<font color=red>2</font>};
7   std::cout << <font color=red>"5/2: "</font> << (static_cast&lt;double&gt;(five) / two) << <font color=red>"\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>

```
5/2: 2.5
```

Yes, problem solved!

In general, the `static_cast` looks like

```
static_cast<new type>(expression)
```

when you want to tell C++ to _interpret_ the type of `expression` as `new type`. Note how we used the word _interpret_. A `static_cast` is itself an expression -- in other words, it has a type and a value. The type of the value of that expression is `new type` and the value is the value of `expression` as that type. _A `static_cast` does not change anything about the value or type of `expression`._

One final important caveat about `static_cast`. The `expression` is evaluated _before_ the `static_cast` occurs. Why is that important? 

What is the output of the following program?

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4 
5   <font color=green>int</font> five{<font color=red>5</font>};
6   <font color=green>int</font> two{<font color=red>2</font>};
7   std::cout << <font color=red>"5/2: "</font> << static_cast<<font color=green>double</font>>(five / two) << <font color=red>"\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }
</pre></body></html>

```
5/2: 2
```

What? Think about it! The first thing that C++ does is evaluate the expression `5/2`. Those are two `int`s and, well, we are right back where we started. The value of that expression is $2$ and it's type is `int`! It is only then that the `static_cast` does it job and tell C++ to interpret the `int` $2$ as a `double`.

Be very careful!