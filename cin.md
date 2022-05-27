## What's News

Political pollsters have completed their root-cause analysis of why they mispredicted the results of recent electoral races -- all they had to do was get input from their constituents!

## Getting input

Like we are able to perform _character_ output (`std::cout`), we are able to perform _character_ input as well. In fact, the two operations look very similar. 

Remember what it looks like to print the contents of the variable `name` to the screen:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   std::string name{<font color=red>"Alexandra"</font>};
5   std::cout << name;
6   <font color=green>return</font> <font color=red>0</font>;
7 }

</pre></body></html>

The output is actually accomplished on line 5:

```
std::cout << name;
```

As you recall, the `<<` is called the stream insertion operator because we are telling the compiler to "insert" the value of `name` in to a "stream" of data that is destined for the console. Visually, the `<<` is intuitive because it looks like it is "funneling" the value from `name` to `std::cout` (which we can think of as the console).

What would happen if we flipped that around? Well, it would seem to follow that we could go in the opposite direction and _get_ values from the stream of data that is coming from the console! 

And, yes, we can! To read the user's name from their keyboard input, we use `std::cin` and `>>`, the _stream extraction operator_.

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   std::string name{<font color=red>""</font>};
5   std::cin >> name;
6   <font color=green>return</font> <font color=red>0</font>;
7 }

</pre></body></html>

`std::cin` is pretty powerful! It gives us the power to ask the user for data of a certain type and _enforce_ that condition! Let's see how that works. Say we want to ask the user to enter the number of tires on their car. Well, you can't have half a tire on your car, so the number that they enter must be a whole number -- an `int`. 

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   <font color=green>int</font> num_tires_on_car{<font color=red>0</font>};
5   std::cin >> num_tires_on_car;
6   <font color=green>return</font> <font color=red>0</font>;
7 }

</pre></body></html>

Because `num_tires_on_car` is an `int`, we can use the value that the user entered and is stored in `num_tires_on_car` in mathematical expressions where we would require an `int`. Pretty cool!

Later, we will learn how to check whether the user entered a value that is valid for the type that we want. That will give us even more power!

A word is an `std::string`. If we wanted to write a program that gave a user some positive feedback about their favorite word, it would look something like this:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   std::string favorite_word{<font color=red>""</font>};
5   std::cout << <font color=red>"Please type your favorite word: "</font>;
6   std::cin >> favorite_word;
7   std::cout << favorite_word << <font color=red>" is a great word!\n"</font>;
8   <font color=green>return</font> <font color=red>0</font>;
9 }

</pre></body></html>


On line 5 we are "prompting" the user for their input -- basically, we are asking them for some response. Because we want the prompt to appear on the screen, we are using `std::cout` and the stream insertion operator. On line 6 we are reading the response the user typed. We are using `std::cin` and the stream extraction operator. Finally, on line 7 we are giving the user praise for their choice. We are using the stream insertion operator and `std::cout` again to make a message appear on the screen!

```
Please type your favorite word: obscure
obscure is a great word!
```
