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

These type of "anomalies" are hard to detect and end up being the root cause of many, many security vulnerabilities and hard-to-spot bugs. Be sure to always be on the lookout for what might happen if your program has to deal with data that stretch the limits of what your variable's types are designed to hold!