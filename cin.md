## What's News

Political pollsters have completed their root-cause analysis of why they misjudged the results of recent electoral races -- all they had to do was get input from their constituents!

## Getting input

Like we are able to perform _character_ output (`std::cout`), we are able to perform _character_ input as well. In fact, the two operations look very similar. 

Remember what it looks like to print the contents of the variable `name` to the screen:

```C++
#include <iostream>

int main() {
  std::string name{"Alexandra"};
  std::cout << name;
  return 0;
}
```

The output is actually accomplished on line 5:

```C++
std::cout << name;
```

As you recall, the `<<` is called the stream insertion operator because we are telling the compiler to "insert" the value of `name` in to a "stream" of data that is destined for the character output device (usually the console!). Visually, the `<<` is intuitive because it looks like it is "funneling" the value from `name` to `std::cout` (which we can think of as the console).

What would happen if we flipped that around? Well, it would seem to follow that we could go in the opposite direction and _get_ values from the stream of data that is coming from the console! 

And, yes, we can! To read the user's name from their keyboard input, we use `std::cin` and `>>`, the _stream extraction operator_.

```C++
#include <iostream>

int main() {
  std::string name{""};
  std::cin >> name;
  return 0;
}
```

`std::cin` is pretty powerful! It gives us the power to ask the user for data of a certain type and _enforce_ that condition! Let's see how that works. Say we want to ask the user to enter the number of tires on their car. Well, you can't have half a tire on your car, so the number that they enter must be a whole number -- an `int`. 

```C++
#include <iostream>

int main() {
  int num_tires_on_car{0};
  std::cin >> num_tires_on_car;
  return 0;
}
```

Because `num_tires_on_car` is an `int`, we can use the value that the user entered (which we subsequently stored in `num_tires_on_car`) in mathematical expressions wherever we would require an `int`. Pretty cool!

Later, we will learn how to check whether the user entered a value that is valid for the type that we want. That will give us even more power!

A word is a `std::string`. If we wanted to write a program that gave a user some positive feedback about their favorite word, it would look something like this:

```C++
#include <iostream>

int main() {
  std::string favorite_word{""};
  // Prompt the user for their input.
  std::cout << "Please type your favorite word: ";
  // Read in their input.
  std::cin >> favorite_word;
  std::cout << favorite_word << " is a great word!\n";
  return 0;
}
```

First, we are "prompting" the user for their input -- basically, we are asking them for some response. Because we want the prompt to appear on the screen, we are using `std::cout` and the stream insertion operator. Then we are reading the response that they typed. We are using `std::cin` and the stream extraction operator. Finally, we are giving the user praise for their choice. We are using the stream insertion operator and `std::cout` again to make a message appear on the screen!

Here's how it might look if we ran that program (user input in **bold**).

<pre>
Please type your favorite word: <b>obscure</b>
obscure is a great word!
</pre>
