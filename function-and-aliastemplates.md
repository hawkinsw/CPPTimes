## What's News

After making our TVs smart enough to watch any *type* of content, Reed Hastings and Ted Sarandos of Netflix are teaming up with Roku to make *smart* AM/FM radios and smart portable CD players. They promise worldwide advances in entertainment by making these two venerable media tools intelligent.

## Function Templates for Generic Programming

In our previous classes we learned about [class templates](./templates.md) as a way to use generic programming to write classes that are flexible and can be configured based on the users' choice of types. The type that the user chooses when they instantiate an instantiated class from our class template can be used in place of the name of an actual type throughout the class. Pretty cool!

But, it gets better -- C++ offers us the ability to write function templates. As the name implies, function templates are templates for declaring and defining functions. Like a class template allows us to write a class definition that is flexible enough to handle multiple different types without and obviates a need to copy/paste code, function templates let us write a function definition that is flexible enough to handle different types!

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

An example might help make things more concrete. Let's say that we want to write a function that prints out a nice message telling the user the size of a vector. That's easy, right? Well, not so fast -- what kind of vectors can our function handle? `std::vector`s of integers? `std::vector`s of doubles? For each of the types of `std::vector`s we would have to write a different version of the function. With a function template we could write a single definition/declaration and be done with it:

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

Although not as often used (even though they should be), *aliases* give a progarmmmer the power to give another name to an existing type. By giving it another name, the programmer is able to give additional semantic meaning to the type in order to produce more self-documenting code.

For example, `double`s are the perfect type to contain a per-event value. As an average, per-event values are usually not whole notes (i.e., `int`s won't work). However, using the type name `double` might not help the next programmer who needs to modify the code understand what is going on. A `double` is a good type for storing the average number of touchdowns scored per game for a football in a season:

```C++
#include <iostream>

int main() {
    double bengals{5.1};
    std::cout << "Bengals touchdowns per game: " << bengals << "\n";
    return 0;
}
```

But, we can do *so* much better. What we can do is define `TouchdownsPerGame` as an alias for a `double` using the `using` syntax. That ways, when someone reads the code, it just makes more sense:


Thank you for your question about alias templates!

In C++ there is the ability to name "aliases" for types. Let's say that we want to make a special name for double'egers so that we can give them some extra meanings to fellow programmers:

```C++
#include <iostream>

using TouchdownsPerGame = double;

int main() {
    TouchdownsPerGame bengals{5.1};
    std::cout << "Bengals touchdowns per game: " << bengals << "\n";
    return 0;
}

```

> NB: An alias *does not* make a new type -- it is just another *name* for the same type.

Well, what happens if we wanted to declare an alias for, say, `std::vector`s? A `std::vector` (and most other *container types*) take a template parameter when we declare them:

std::vector<double> semesterGrades{};

So, if we wanted to use our trick from above, we will need to use an *alias template*:

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
