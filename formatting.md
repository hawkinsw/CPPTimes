### What's News

Archaelogists have uncovered ancient markings that indicate a picture really is not worth one thousand words. The prehistoric writing indicates, however, that formatting does matter.


### Formatting

C++ has a bevy of functions available to make it easy to generate nicely formatted output. You will rarely use them in industry, but it is good to practice using them and _know_ that they exist.
To repeat, it is important that you _know_ that these functions exist, but it is _not_ important that you memorize their semantics. These are functions whose documentation is easily accessible for situations where you need to use them. To use these functions, you will need to include the `iomanip` header file.

#### Set the (minimum) width

There are situations where you want to print the value of a variable but you want to _pad_ that output so that it is at least a certain width. `setw` is the function that will do that. `setw` takes a single parameter -- the minimum width of the next item output. A picture is worth a thousand words:

<html><head></head><body><pre>
1 #include &lt;iomanip&gt;
2 #include &lt;iostream&gt;
3 #include &lt;string&gt;
4 
5 <font color=green>int</font> main() {
6   <font color=green>int</font> one{<font color=red>1</font>};
7   std::cout << <font color=red>"-"</font> << std::setw(25) << one << <font color=red>"-\\n"</font>;
8 }

</pre></body></html>


will print

```
-                      1-
```

Because `1` takes a single character to print, there are 24 empty spaces printed so that the minimum width (25) is achieved. Note that this is the minimum -- if the contents of the variable one had taken more than 25 characters to print, the value would not have been _truncated_ (or clipped).

The value set in a call to `setw` persists only for the very next output. As soon as a value is output, the minimum width goes back to the default. In other words, the value set in a call to `setw` does not _stick_.

#### Make `setw` Perform Tricks

In the previous example, the extra space required to meet the minimum width was filled with spaces. We can change that and choose any character to fill those extra spaces. Use the `setfill` function to do so.

<html><head></head><body><pre>
 1 #include &lt;iomanip&gt;
 2 #include &lt;iostream&gt;
 3 #include &lt;string&gt;
 4 
 5 <font color=green>int</font> main() {
 6   <font color=green>int</font> one{<font color=red>1</font>};
 7 
 8   std::cout << std::setfill('@');
 9   std::cout << <font color=red>"-"</font> << std::setw(25) << one << <font color=red>"-\n"</font>;
10 }

</pre></body></html>

will print

```
-@@@@@@@@@@@@@@@@@@@@@@@@1-
```

The fill value set with a call to `setfill` does stick.

#### Everything you own in a box to the left

So far all we've been able to do is print the content on the right-hand side. But, you can left-justify the content as well! Just use the `left` function.

<html><head></head><body><pre>
 1 #include &lt;iomanip&gt;
 2 #include &lt;iostream&gt;
 3 #include &lt;string&gt;
 4 
 5 <font color=green>int</font> main() {
 6   <font color=green>int</font> one{<font color=red>1</font>};
 7 
 8   std::cout << std::setfill('@') << std::left;
 9   std::cout << <font color=red>"-"</font> << std::setw(25) << one << <font color=red>"-\n"</font>;
10 }

</pre></body></html>

prints

```
-1@@@@@@@@@@@@@@@@@@@@@@@@-
```

The fill value set using `setfill` sticks.

#### Set the number of significant digits

Use `setprecision` to set the number of significant digits when printing numbers.

<html><head></head><body><pre>
1 #include &lt;iomanip&gt;
2 #include &lt;iostream&gt;
3 #include &lt;string&gt;
4 
5 <font color=green>int</font> main() {
6   <font color=green>double</font> dbl = <font color=red>6.45</font>;
7   std::cout << <font color=red>"dbl: "</font> << std::setprecision(2) << dbl << <font color=red>"\n"</font>;
8 }

</pre></body></html>

will print

```
dbl: 6.5
```
Note that `setprecision` will cause rounding!

#### 1 or 1.0?

We have seen that `std::cout` helpfully refuses to print the .0 when a floating point number has no fractional part. We can change this behavior using the `showpoint` function.

<html><head></head><body><pre>
1 #include &lt;iomanip&gt;
2 #include &lt;iostream&gt;
3 #include &lt;string&gt;
4 
5 <font color=green>int</font> main() {
6   <font color=green>double</font> dbl = <font color=red>6.0</font>;
7   std::cout << <font color=red>"dbl: "</font> << std::showpoint << dbl << <font color=red>"\n"</font>;
8 }

</pre></body></html>

will print

```
6.00000
```

#### Setprecision is Finicky

`setprecision` will not always do what you want. If you have a very large number it will sometimes convert it to scientific notation. Again, don't worry about the specifics -- you can always look up the behavior in the documentation when you need it! However, we can combine `setprecision` with `fixed` to get more reliable behavior. When combined with `fixed`, `setprecision` can be used to specify the number of significant digits printed after the `.`.

<html><head></head><body><pre>
1 #include &lt;iomanip&gt;
2 #include &lt;iostream&gt;
3 #include &lt;string&gt;
4 
5 <font color=green>int</font> main() {
6   <font color=green>double</font> dbl = <font color=red>6.123456</font>;
7   std::cout << <font color=red>"dbl: "</font> << std::fixed << std::setprecision(4) << dbl << <font color=red>"\n"</font>;
8 }

</pre></body></html>
will print

```
dbl: 6.1235
```

Again, note the rounding! Using `fixed` in combination with `setprecision` is a good way to print dollars and cents.