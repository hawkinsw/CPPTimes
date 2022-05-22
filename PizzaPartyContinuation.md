Look no further than your front yard for the next issue of the _C++ Times_. Our hard-working journalists go to the ends of the earth to bring you all the latest headlines from the world of CS1021C/EECE1080C.

## Pizza Party 3.0

Recall where we last left off when designing and implementing the Pizza Party application:

<html>
<pre><font color="#FCE94F">  1 </font><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>
<font color="#FCE94F">  2 </font>
<font color="#FCE94F">  3 </font><font color="#34E2E2">/*</font>
<font color="#FCE94F">  4 </font><font color="#34E2E2"> * This program describes Will&apos;s pizza party with his (only)</font>
<font color="#FCE94F">  5 </font><font color="#34E2E2"> * friend Kevin.</font>
<font color="#FCE94F">  6 </font><font color="#34E2E2"> */</font>
<font color="#FCE94F">  7 </font><font color="#87FFAF">int</font> main() {
<font color="#FCE94F">  8 </font>  std::string wills_friend{<font color="#AD7FA8">&quot;Kevin&quot;</font>};
<font color="#FCE94F">  9 </font>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will and his friend &quot;</font> &lt;&lt; wills_friend
<font color="#FCE94F"> 10 </font>            &lt;&lt; <font color="#AD7FA8">&quot; are sharing a large pizza.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F"> 11 </font>  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will eats 4 slices and &quot;</font> &lt;&lt; wills_friend &lt;&lt; <font color="#AD7FA8">&quot; eats 2.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
<font color="#FCE94F"> 12 </font>  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
<font color="#FCE94F"> 13 </font>}
</pre>
</html>

We called that version _2.0_ and it was a significant upgrade over version _1.0_. However, there are still many things that can be improved.

Most importantly, our string _literals_ continue to have a hard-coded number of slices that each person eats. If I wanted to change the number of slices that I eat, it would require me to change the the number 4 anywhere I see it in the code. This is certainly not good! 

So, let's fall back on the trick we learned earlier and replace those values with variables! 

We learned in the previous lecture that every variable in C++ belongs to a particular category. Depending on the category of the variable, we can tell what kinds of values that variable can hold. In Pizza Party 2.0, our `wills_friend` variable held only strings which put it in a particular category. 

Let's say that the number of slices that each person eats must be a whole number -- no one can eat just half a slice, after all! Pizza is just too tasty to stop halfway through! We will need, then, a category of variable designed to hold whole numbers! We will call them _integers_ and their _type_ is `int`. 

### A Type of Digression

**Wait, _type_? What's that??** The _type_ of a variable defines the
1.  range of valid values of a variable and
2.  valid operations (e.g., addition, subtraction, comparison, extracting a substring, etc) for a variable;

In this class we will learn the five most important types in C++:

| Name | Type | Example |
| -- | -- | -- |
| `int` | Whole numbers | `1`, `10`, `1024` |
| `double` | Fractional numbers | `1.5`, `71.0` |
| `char` | Single character | `'c'`, `'\n'` |
| `bool` | True or false | `true`, `false` |
| `std::string` | Strings | `"Testing"`, `"c"` |

There are a few _very_ important things to recognize in this table:

1. `'c'` is a character and `"c"` is a string, even though they both contain a single `c`. The `'` is used to enclose a `char` and `"` is used to enclose a `std::string`.
2. `'\n'` is a character, even though it looks like it contains two characters. Think about why and recall _escape sequences_.
3. `double`s can hold whole numbers (i.e., `int`s are a subset of `double`s) but `int`s cannot hold fractional numbers.

Types play an important role in programming languages. Because they signal to the programmer (and the compiler!) the valid values that a variable can hold, they help prevent very [terrible](https://nebelwelt.net/publications/files/16CCS2.pdf) _run-time_ errors. NASA famously lost a very expensive rocket in a very public, humiliating fashion because their software attempted to stuff a big number into a variable whose type limited it to hold only small numbers! Read more about that [here](https://www.esa.int/Newsroom/Press_Releases/Ariane_501_-_Presentation_of_Inquiry_Board_report).

If the compiler can alert the programmer to the fact that they are confusing types, their software can be safer: There is no chance that they assign, say, `"My name is ..."` to a variable designed to hold a number. Therefore, the software is always guaranteed to safely perform operations on variables (it would be very bad to attempt to divide `"Please enter your password"` by `5`, say, because the result is nonsensical).

A _type error_ occurs when a programmer attempts to assign a value of one type to a variable designated to hold a different, incompatible, type. For instance, attempting to store `'c'` to a variable whose type is `bool` would make no sense (although C++ tries to be "helpful" in cases like this -- we will come back to that later!). A language that can always detect type errors (whether those errors are detected when the program is compiled or run) is known as a _strongly typed_ programming language. A language that is not strongly typed is known as a _weakly typed_ programming language. Obviously, it would be better for the language to detect type errors when the program is compiled (that makes them easier to fix), but even detecting type errors at runtime is better than nothing!

### Back to Our Party

So, where were we? We were about to use variables of `int` type to hold the number of slices that my friend and I ate. We want these variables to hold `2` and `4` respectively. So, when we declare/define them (recall the definition of a declaration from last class), we will also want to _initialize_ them (again, recall initialization from last class). As we discussed previously, although it is not strictly required, it is best practice to initialize variables when you declare/define them. You will see why in future lectures.

In order to accomplish what we just described, we can write

<html>
<pre>  <font color="#87FFAF">int</font> will_slices{<font color="#AD7FA8">2</font>};
  <font color="#87FFAF">int</font> friend_slices{<font color="#AD7FA8">4</font>};
</pre>
</html>

and then introduce those variables in the appropriate places in the `std::cout` statements.

## Pizza Party 4.0

Here's what our code looks like now:

<html>
<pre><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>

<font color="#87FFAF">int</font> main() {
  std::string wills_friend{<font color="#AD7FA8">&quot;Kevin&quot;</font>};

  <font color="#87FFAF">int</font> will_slices{<font color="#AD7FA8">2</font>};
  <font color="#87FFAF">int</font> friend_slices{<font color="#AD7FA8">4</font>};

  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will and his friend &quot;</font> &lt;&lt; wills_friend
            &lt;&lt; <font color="#AD7FA8">&quot; are sharing a large pizza.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will eats &quot;</font> &lt;&lt; will_slices &lt;&lt; <font color="#AD7FA8">&quot; slices and &quot;</font> &lt;&lt; wills_friend
            &lt;&lt; <font color="#AD7FA8">&quot; eats &quot;</font> &lt;&lt; friend_slices &lt;&lt; <font color="#AD7FA8">&quot;.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
}
</pre>
</html>

It would be great if our program were able to keep track of the number of the slices of pizza left to be eaten at any given time. Let's assume that a large pizza contains 10 slices of pizza to start (which was the class' consensus, despite some argument!).

Again, the number of slices is always going to be a whole number. Therefore, if we want to store this value in a variable, we would give that variable the `int` type. It seems like a reasonable name for a variable that holds the number of slices remaining to be eaten would be `slices_remaining`. It would start out containing the value 10.

<html>
<pre>  <font color="#87FFAF">int</font> slices_remaining{<font color="#AD7FA8">10</font>};
</pre>
</html>

After my friend and I eat some of the pizza, that number of slices remaining is going to change! Good thing that variables are designed to do exactly that -- vary!

In order to update the value of a variable, we can use the _assignment_ statement/expression. Generally, the format of the assignment statement/expression is

<html>
<pre>  variable = &lt;New Value&gt;;
</pre>
</html>

Notice that the variable you are updating goes on the _left_ side of the `=`s sign. This arrangement is not obvious when you first see such a construction. As long as the `New Value` has a type that is compatible with the `variable` type, it can be anything you like: a literal, a variable, or the result of an operation!

The only requirement is that it have a value. In C++, an _expression_ is anything that has a value! So far, we have seen two things that have values:

1. Variables (their contents)
2. Literals (well, _exactly_ what is written)

There are others, though. In particular, the result of mathematical operations generate values and are, therefore, considered expressions. Let's use an expression to calculate a new value for the `slices_remaining` variable after my friend and I chow down.

Reminiscent of a word problem from elementary school math class, let's answer the following question: If there were 10 slices of pizza to start and Will ate 4 and his friend at 2, how many slices are left?

```
Number of slices that we started with - (number of slices that Will ate + the number of slices that his friend ate)
```

Awesome answer. Let's just translate that into C++:

<html>
<pre>  slices_remaining - (will_slices + friend_slices)
</pre>
</html>

And we want to _assign_ the result of that expression (!!) to `slices_remaining` so that it's value is updated:

<html>
<pre>  slices_remaining = slices_remaining - (will_slices + friend_slices);
</pre>
</html>

Note a few things:

1. We can use `slices_remaining` on _both_ sides of the `=` without a problem. `slices_remaining` is only changed as a result of the assignment _after_ the calculation on the right-hand side of the `=` is evaluated. 
2. There is a `;` at the end because that entire "thing" represents a complete instruction to perform some action (the very definition of a statement) and we know that all statements in C++ end in a `;`.

I was sneaky! I slipped in another new term: _evaluate_. When you evaluate an expression, you calculate it's value. For instance, if you evaluate the expression `wills_friend`, you get `"Kevin"` (remember, variables are expressions) and if you evaluate `5 + 3` you get `8`. Pretty cool.

Here's the final version of Pizza Party 4.0:

<html>
<pre><font color="#5FD7FF">#include </font><font color="#AD7FA8">&lt;iostream&gt;</font>

<font color="#87FFAF">int</font> main() {
  std::string wills_friend{<font color="#AD7FA8">&quot;Kevin&quot;</font>};

  <font color="#87FFAF">int</font> will_slices{<font color="#AD7FA8">2</font>};
  <font color="#87FFAF">int</font> friend_slices{<font color="#AD7FA8">4</font>};

  <font color="#87FFAF">int</font> slices_remaining{<font color="#AD7FA8">10</font>};

  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will and his friend &quot;</font> &lt;&lt; wills_friend
            &lt;&lt; <font color="#AD7FA8">&quot; are sharing a large pizza.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;
  std::cout &lt;&lt; <font color="#AD7FA8">&quot;Will eats &quot;</font> &lt;&lt; will_slices &lt;&lt; <font color="#AD7FA8">&quot; slices and &quot;</font> &lt;&lt; wills_friend
            &lt;&lt; <font color="#AD7FA8">&quot; eats &quot;</font> &lt;&lt; friend_slices &lt;&lt; <font color="#AD7FA8">&quot;.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;

  slices_remaining = slices_remaining - (will_slices + friend_slices);

  std::cout &lt;&lt; <font color="#AD7FA8">&quot;After eating their fill, the pizza has &quot;</font> &lt;&lt; slices_remaining
            &lt;&lt; <font color="#AD7FA8">&quot; slices remaining.</font><font color="#FFD7D7">\n</font><font color="#AD7FA8">&quot;</font>;

  <font color="#FCE94F">return</font> <font color="#AD7FA8">0</font>;
}
</pre>
</html>


## New Terms Defined in This Edition

1. _type_
1. _type error_
2. _strongly type language_
2. _weakly type language_
3. _assignment statement/expression_
4. _expression_
5. _evaluate_ 