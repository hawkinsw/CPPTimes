## What's News

After making our TVs smart enough to watch any *type* of content, Reed Hastings and Ted Sarandos of Netflix are teaming up with Roku to make *smart* AM/FM radios and smart portable CD players. They promise worldwide advances in entertainment by making these two venerable media tools intelligent.

## Function Templates for Generic Programming

In our previous classes we learned about [class templates](./templates.md) as a way to use generic programming to write classes that are flexible and can be configured based on the users' choice of types. The type that the user chooses when they instantiate a class from our class template (what is called the template argument) will be substituted in a _copy_ of the class' declaration as if it were copied/pasted. Pretty cool!

But, it gets better -- C++ offers us the ability to write function templates. As the name implies, function templates are templates for declaring and defining functions. Like a class template allows us to write a class definition that is flexible enough to handle multiple different types and obviates a need to copy/paste code, function templates let us write a function definition that is flexible enough to handle different types!

The syntax for declaring and defining a function template is eerily similar to the code for declaring and definition a class template:

```C++
template <typename Type>
return_type function_name(<parameters>) {
  ...
  <statements>
  ...
}
```

Inside a function template declared as above, you can use `Type` anywhere that you would use the name of a C++ type.

An example might help make things more concrete. Let's say that we want to write a function that prints out a nice message telling the user the size of a vector. That's easy, right? Well, not so fast -- what kind of vectors can our function handle? `std::vector`s of integers? `std::vector`s of doubles? For each of the types of `std::vector`s we would have to write a different version of the function. 

In other words, we would have to define an overloaded function named `print_vector_size` for all those different types:

```C++
void print_vector_size(const std::vector<int> &v) {
  ...
}
void print_vector_size(const std::vector<double> &v) {
  ...
}
void print_vector_size(const std::vector<std::string> &v) {
  ...
}
...
```

Honestly, though, it doesn't really matter what is _in_ the vector, but _how many_ are in the vector. As we said before, changing the types without changing the algorithm means that we are using parametric polymorphism and function templates are a huge help here. With a function template we could write a single definition/declaration and be done with it:

```C++
template <typename TypeInVector>
void print_vector_size(const std::vector<TypeInVector> &v) {
  std::cout << "This vector has " << v.size() << " elements.\n";
  return;
}
```

With a function named `print_vector_size` so defined, we can write the following:

```C++
int main() {
  std::vector<int> integer_vector{};
  std::vector<std::string> string_vector{};

  print_vector_size(integer_vector);
  print_vector_size(string_vector);

  return 0;
}
```

Even though we only coded one body for the `print_vector_size` function, we got two generated! Thanks C++!

## Going Undercover with Alias Templates

Although not as often used (even though they should be), _aliases_ give a programmer the power to give another name to an existing type. By giving it another name, the programmer is able to give additional semantic meaning to the type in order to produce more self-documenting code.

For example, `double`s are the perfect type to hold the average of a per-event statistic. As an average, per-event statistics are usually not whole numbers (i.e., `int`s won't work). However, using the type name `double` might not help the next programmer who needs to modify the code understand what is going on. A `double` is a good type for storing the average number of touchdowns scored per game for a football in a season:

```C++
#include <iostream>

int main() {
    double bengals{5.1};
    std::cout << "Bengals touchdowns per game: " << bengals << "\n";
    return 0;
}
```

But, we can do _so_ much better. What we can do is define `TouchdownsPerGame` as an alias for a `double` using the `using` syntax. That ways, when someone reads the code, it just makes more sense:


```C++
using TouchdownsPerGame = double;
```

And how could we use it? Well, just like a `double`:

```C++
#include <iostream>

using TouchdownsPerGame = double;

int main() {
    TouchdownsPerGame bengals{5.1};
    std::cout << "Bengals touchdowns per game: " << bengals << "\n";
    return 0;
}

```

> NB: An alias _does not_ make a new type -- it is just another _name_ for the same type.

Well, what happens if we wanted to declare an alias for, say, `std::vector`s? A `std::vector` (and most other [_container types_](https://en.cppreference.com/w/cpp/container.html)) take a template parameter when we declare them:

```C++
std::vector<double> semesterGrades{};
```

So, if we wanted to use our trick from above, we will need to use an _alias template_:

```C++
template <typename GradeType>
using SemesterGrades = std::vector<GradeType>;
```

That will give us the power to make vectors of grades that are either floating point (double) or whole numbers (int) depending on how precise we want to be:

```C++
#include <vector>

template <typename GradeType>
using SemesterGrades = std::vector<GradeType>;

int main() {
    SemesterGrades<double> willsGradesIn2080C{};
    return 0;
}
```

Take a look at C++ Insights as a reminder of what the compiler is doing on your behalf behind the scenes when you use templates, and especially alias templates. Because the type of `willsGradesIn2080C` is an alias for a vector of `GradeType` (and the template argument is a `double`), we would expect the C++ makes the type a vector of floating point numbers.

If you [check out](https://cppinsights.io/s/58bb91ca) the results, you will see that is exactly what it does. The compiler takes our

```C++
    SemesterGrades<double> willsGradesIn2080C{};
```

and turns it into 

```C++
  std::vector<double, std::allocator<double> > willsGradesIn2080C = std::vector<double, std::allocator<double> >{};
```

Woah![^allocators]

[^allocators]: If you are curious, the second template argument to the `std::vector` is called an allocator and is the type of a class that will perform (memory) allocations on behalf of the implementation of `std::vector` when the vector needs more space to store additional items!
