# Object-Oriented Programming Introduction - Abstract Data Types

## What's News
Overnight there was a tremendous leak of highly confidential material from programmers around the world who relied on a network of informants to keep their dirtiest secrets safe. The Parameter Papers have exposed global variables, poorly named functions and a disastrous lack of comments.

## A Data Abstraction, Indeed

In the past several class sessions, we have learned a new type of abstraction, *data abstraction*. To date we have seen the power of procedural abstraction using functions. Remember that in procedural abstraction we are "hiding" the "how" from users and presenting them with a simplified interface for accomplishing a certain behavior.

Data abstraction, similarly, hides something, too. It hides information from the user in order to present them with a simplified interface. Remember our pithy definition of abstraction?

> Remembering what's important and forgetting what's not, depending on the context.

Let's think closely about how we would abstract the data about a date. Like the user of a function that we write (procedural abstraction) does not care how we accomplish an operation as long as it works correctly, a user of our date data abstraction (tongue twister!) does not particularly care about how we represent that date behind the scenes as long as it represents all the data relevant to a date.

Like we have to give a function safe, proper values for its parameters when we call it in order to be able to rely on its correct operation, a data abstraction is only  guaranteed to work correctly when we give it compatible data.

Okay, where have we heard something like that before? It sounds, to me, like the first half of the definition of a *type*: A range of valid values. Great! Now keep that in mind -- we'll come back to part two of that definition in a minute.

Because it's a data abstraction, the user only cares that it can be used anywhere a date can reasonably be used. It should be possible to compare two dates to see which one came first. It should be possible to compare two dates to determine the time elapsed between them. It should be possible to print a date to the screen for display on a calendar. The list goes on.

What do all those things sound like? That's right, they sound like examples of *valid operations* that can be performed on a date! Wait, *that* sounds exactly like part two of the definition of type! 

That's exactly right! In most cases, software developers combine data abstractions (what defines the range of valid values for non-fundamental types -- or *compound types* in C++) with procedural abstractions (what defines the valid operations for compound types) to get something call an *abstract data type* (ADT).

Enough theory, how do we implement an ADT in C++? Let's find out!

## Load-Bearing Walls

The most straightforward way to identify a date is to store: the day, the month and the year. At their most basic, we can represent each of these as integers. In C++, the `struct` is one of two ways to combine one or more variables of fundamental (or even other compound types) to create a *compound* type. The `struct` declaring a new `Date` *type* would look like:

```C++
struct Date {
  int month;
  int day;
  int year;
};
```

The `int`eger variables `month`, `day` and `year` are known as the *member variables* of the `Date` type.

That's pretty simple and concise. Now, instead of using three separate variables (one for a day, one for a month and one for a year) to describe, say, my dad's birthday, I can use a single one with a type of `Date`. Let's write a program that wishes my dad a happy birthday:

```C++
#include <iostream>

struct Date {
  int month;
  int day;
  int year;
};

int main() {
  Date dads_birthday{7, 18, 1949};
  std::cout << "On " << dads_birthday.month << "/" 
            << dads_birthday.day 
            << ", I will wish him a happy birthday!\n";
  return 0;
}
```

In the C++ program above, `dads_birthday` has the type of `Date`. That is just like

```C++
int age{45};
```

where `age` has the type `int`. Yes, we have the power to make our own types and then declare and define variables that have that type! How cool! When we write

```C++
Date dads_birthday{7, 18, 1948};
```

we are *instantiating* an *instance* (yes, I know it's repetitive) of a `Date` type with the name `dads_birthday`. Alternately, we can call `dads_birthday` an *object* whose type is `Date`. Any developer who instantiates an instance of a class that implements an ADT is called a _user_ of that class and/or ADT.

> Note: It is hard to overstate the importance of understanding the concept of _instantiation_ and recognizing the difference between a _type_ and an _instance_ (or _object_).

When I build and run the program above, it will produce the following output:

```
On 7/18, I will wish him a happy birthday!
```

Exactly what we wanted.

Let's look at an alternate syntax for instantiating an object of type `Date` and updating the values of its member variables:

```C++
struct Date {
  int month;
  int day;
  int year;
};

int main() {
  Date d;
  d.month = 2;
  d.day = 29;
  d.year = 2021;
  return 0;
}
```

In this case, we have set the date to be February 29, 2021. Can you spot the problem? 2021 is _not_ a leap year -- February of 2021 only had 28 days. In other words, the date held in `d` is invalid!

Using an ADT was supposed to help us prevent this very situation! The reason we are still susceptible to bad users is that we have not defined the `Date` ADT in a way that limits access to its member variables from the outside world. Users of the ADT Date (as written above) may pull back the curtain and assign values to its member variables arbitrarily. Think about the worst-case scenario: an ADT designed to hold the current speed of a vehicle where the programmer using that struct incorrectly sets a very high number.

Fundamentally, the problem is that an ADT written like the one above provides the power to build data abstractions but does not give programmers the power to perform _data hiding_.

### Data Hiding

Hiding the member variables of an ADT is very important, as we just saw. Ideally, when we write an ADT we want to make sure that its users can only set the member variables to reasonable values. Fortunately C++ gives us a way to do this! Inside an ADT declaration we can specify the _accessibility_ of member variables. Moreover, we will see that we can set the accessibility of the valid operations that can be performed on an ADT (more on that later!).

Let's modify (slightly) our declaration of the Date ADT in order to protect it from having its member variable values corrupted:

```C++
#include <iostream>

struct Date {
private:
  int month;
  int day;
  int year;
};
```
`private` is one of three [access specifier keywords](https://en.cppreference.com/w/cpp/language/access) in C++. We will see `public` below and you will learn about `protected` in future classes.

Now, when we attempt to compile 

```C++
int main() {
  Date d;
  d.month = 2;
  d.day = 29;
  d.year = 2021;
  return 0;
}
```
we get errors (like we should!):

```
**date_struct.cpp:** In function ‘**int main()**’:
**date_struct.cpp:18:5:** **error:** ‘**int Date::month**’ is private within this context
   18 |   d.**month** = 2;
      |     **^~~~~**
**date_struct.cpp:11:7:** **note:** declared private here
   11 |   int **month**;
      |       **^~~~~**
**date_struct.cpp:19:5:** **error:** ‘**int Date::day**’ is private within this context
   19 |   d.**day** = 29;
      |     **^~~**
**date_struct.cpp:12:7:** **note:** declared private here
   12 |   int **day**;
      |       **^~~**
**date_struct.cpp:20:5:** **error:** ‘**int Date::year**’ is private within this context
   20 |   d.**year** = 2021;
      |     **^~~~**
**date_struct.cpp:13:7:** **note:** declared private here
   13 |   int **year**;
      |       **^~~~**
```

This outcome is ideal -- we have prevented malicious users from arbitrarily setting the day, month or year to incorrect values. But, is it really ideal?

Not really. By making these member variables private the user of the Date ADT is unable to actually use it -- they cannot determine the actual day, month or year that it represents. In other words, we have the power to ensure that every instance contains valid values and that the user cannot do anything sneaky but we do not have the power yet to specify valid operations!

## Data Access

How to solve this problem? I have an idea: If we have member variables, I bet that C++ will give us the chance to write *member functions*! And, it does!

A *member function* is a special function that "belongs" with a data type who is privileged with the power to access all of its member variables -- public, protected *and* private! When you implement a member function you do so under the assumption that it will only be invoked in the context of an instance of its type. What's more, in your implementation of a member function you can assume that you have implicit access to that instance. When you are writing the code of a member function, any reference you make to the member variables will access/update the value of those member variables specific to that instance.

Now, let's return to writing these mediator functions that are going to let the user of the `Date` ADT safely update and access it.
If we write these member functions correctly, we will always be able to guarantee the member variables' values stay reasonable. 

We will have two kinds of these mediator member functions: one kind for the ADT users' to _set_ the values of the member variables (_setter member functions_) and one kind for the ADT users' to _get_ the values of the member variables _(getter member functions)._ Let's first write the getter member functions -- they're easier:


```C++
struct Date {
  int getMonth() {
    return month;
  }
  int getDay() {
    return day;
  }
  int getYear() {
    return year;
  }
private:
  int month;
  int day;
  int year;
};
```

Let's say that we have a `Date` ADT object that holds my birthday (`wills_birthday`). We could rewrite the `std::cout` from above that wished my dad a happy birthday by using getter functions:

```C++
  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will be a year older!\n";
```

See how you can use these getters just like you were accessing member variables, albeit with a slightly different syntax? That's really cool!

Now, let's give the `Date` ADT users the opportunity to change the values of member variables by writing setters:

```C++
struct Date {
  int getMonth() {
    return month;
  }
  int getDay() {
    return day;
  }
  int getYear() {
    return year;
  }

  void setMonth(int new_month) {
    month = new_month;
  }
  void setDay(int new_day) {
    day = new_day;
  }
  void setYear(int new_year) {
    year = new_year;
  }

private:
  int month;
  int day;
  int year;
};
```

Now we have all the functionality that we had before, and protection! Wait, really? The setter functions don't actually "sanitize" the caller's input -- they just mindlessly take what the caller passes as an argument and set the member variables. So, we are still stuck with the possibility that the Date ADT user does something stupid like:

```C++
  Date d;
  d.setYear(-10000);
```

All that work for nothing. Don't fret, all we need to do is write smarter setter functions. Let's try these:

```C++
struct Date {
  int getMonth() {
    return month;
  }
  int getDay() {
    return day;
  }
  int getYear() {
    return year;
  }

  void setMonth(int new_month) {
    if (new_month > 0 && new_month < 13) {
      month = new_month;
    }
  }
  void setDay(int new_day) {
    if (new_day > 0 && new_day < 32) {
      day = new_day;
    }
  }
  void setYear(int new_year) {
    if (new_year > 0) {
      year = new_year;
    }
  }

private:
  int month;
  int day;
  int year;
};
```

If we were to use this version of the `Date` ADT in our code,

```C++
  Date d;
  d.setYear(2021);
  d.setYear(-10000);

  std::cout << "d.getYear(): " << d.getYear() << "\n";
```
the output would be 

```
d.getYear(): 2021
```

See how the `Date` ADT says, "You want to assign to me a bogus year? Well, I'm not going to do it!"?

Now we are getting somewhere.

## The Difference Between a Class and a Struct

We hinted above that there is a second way to declare ADTs in C++. In addition to `struct` we can use `class`. In fact, it is more common to see ADTs declared with the `class` keyword than with the `struct` keyword. ADTs are so often defined with the `class` keyword that ADTs are commonly referred to as _classes_ in C++ (even if they are declared with the `struct` keyword -- I know, it's confusing!).

There is only one small difference between a class declared with the `class` keyword and a class declared with a `struct` keyword: By default, all the member variables and the member functions in an ADT declared with `struct` have `public` accessibility and all the member variables and the member functions in an ADT declared with `class` have `private` accessibility. To see the implications of these defaults, let's simply copy and paste the newest version of the Date ADT declaration and change the `struct` to `class`:

```C++
class Date {
  int getMonth() {
    return month;
  }
  int getDay() {
    return day;
  }
  int getYear() {
    return year;
  }

  void setMonth(int new_month) {
    if (new_month > 0 && new_month < 13) {
      month = new_month;
    }
  }
  void setDay(int new_day) {
    if (new_day > 0 && new_day < 32) {
      day = new_day;
    }
  }
  void setYear(int new_year) {
    if (new_year > 0) {
      year = new_year;
    }
  }

private:
  int month;
  int day;
  int year;
};
```

Now, if we try to compile

```C++
  Date d;
  d.setYear(2021);
  d.setYear(-10000);
```

we will get errors:

```
**date_struct.cpp:44:17:** **error:** ‘**void Date::setYear(int)**’ is private within this context
   44 |   d.setYear(2021**)**;
      |                 **^**
**date_struct.cpp:30:8:** **note:** declared private here
   30 |   void **setYear**(int new_year) {
      |        **^~~~~~~**
**date_struct.cpp:45:19:** **error:** ‘**void Date::setYear(int)**’ is private within this context
   45 |   d.setYear(-10000**)**;
      |                   **^**
**date_struct.cpp:30:8:** **note:** declared private here
   30 |   void **setYear**(int new_year) {
```

Our use of the `class` keyword to implement the ADT means that everything (i.e., member functions and variables) are `private`. We are feeling the impact of our decision to use `class` here: our getter and setter member functions are no longer `public` and, therefore, cannot be invoked on instances of the `Date` type. The good news is that there is a simple fix -- we'll just add the `public` access specifier above the declaration/definition of the getter/setter member functions:

```C++
class Date {
public:
  int getMonth() {
    return month;
  }
  int getDay() {
    return day;
  }
  int getYear() {
    return year;
  }

  void setMonth(int new_month) {
    if (new_month > 0 && new_month < 13) {
      month = new_month;
    }
  }
  void setDay(int new_day) {
    if (new_day > 0 && new_day < 32) {
      day = new_day;
    }
  }
  void setYear(int new_year) {
    if (new_year > 0) {
      year = new_year;
    }
  }

private:
  int month;
  int day;
  int year;
};
```

Great! All our persistence is paying off!

### The Persistence of Access Specifiers

As a brief aside, it is important to understand the persistence of access specifiers in a `struct` or `class`. The level of access granted by the use of an accessibility specifier continues until the next access specifier. For example, in

```C++
class A {
public:  
  int a;
  int b;
  int c;
private:
  int d;
  int e;
public:
  int f;
  int g;
};
```

the member variables `a`, `b`, `c`, `f` and `g` are all `public` while member variables `d` and `e` are `private`. As you can see, you can swap back and forth at any time between `public` and `private`. It is customary, however, to write all the `private` member variables/functions in one block and all the `public` member variables/functions in another block.

## Making History -- Constructing A Date

Way back when (a few paragraphs ago), we had the simple definition of the Date ADT:

```C++
struct Date{
  int month;
  int day;
  int year;
};
```

Then, to create an instance of a `Date` ADT we could simply write

```C++
  Date d{2,9,1982};
```
That code declared _and_ initialized `d` at the same time.

If we tried to write that same syntax with the code for the Date ADT that we have now, the compiler would be angry with us. Once we declared that the certain variables are private, we could no longer declare and initialize a `Date` ADT using the syntax from above. Does it mean that we are stuck writing code that looks like

```C++
  Date d{};
  d.setMonth(2);
  d.setYear(9);
  d.setYear(1982);
```

instead? Of course not -- that's _way_ too much typing. In fact, C++ gives us the power to declare/initialize classes in much more powerful ways than we could above! That power comes from special member functions defined in ADTs that are called _constructors_. They are called constructors because they are used to, well, construct instances of `class`es and `struct`s that implement ADTs. Constructors are used to initialize member variables and do other work that must be done at the moment that an object is instantiated so that the instance is ready to function from the jump!

The most basic form of a constructor is called the _default_ constructor. The default constructor for an ADT gets invoked when an instance of the ADT is declared without specifying any special starting values. For the Date ADT, that would look something like:

```C++
  Date d{};
```

A constructor is like an other member function, but it's declaration has a special syntax. Like an member function, the declaration of the constructor goes inside the declaration of the ADT. However, the declaration of the construction does not have a return type and ts name must match exactly the name of the ADT. Here's how you declare/define the default constructor for the `Date` ADT:

```C++
class Date {
public:
  Date() {
  }

...  
};
```

Great, now the compiler will let us write

```C++
Date d;
std::cout << "d.getYear(): " << d.getMonth() << "\n";
```

but the output will probably surprise you:

```C++
d.getYear(): 21845
```

Again, the utility of a default constructor is to set the ADT's member variables to reasonable initial values, even if the user of the ADT did not. The implementation of the `Date` `class`'s default constructor shown above did, well, nothing -- it certainly did _not_ initialize the member variables, that's for sure. As a result, the member variables were left in an uninitialized state and, as a result, we got the funny output shown above.

Let's fix this by actually writing a meaningful implementation of the default constructor of the `Date` class:

```C++
class Date {
public:
  Date() {
    month = 1;
    day = 1;
    year = 1;
  }
  
...  
};
```

Now, if we write

```C++
Date d;
std::cout << "d.getYear(): " << d.getMonth() << "\n";
```

we will get reasonable output:

```
d.getYear(): 1
```

Wohoo! We still don't quite have the old functionality back, though. We wanted to be able to declare/define an instance of a `Date` ADT and set it to a particular date all in one line. In order to do that we can write another constructor -- this one will take three parameters: the initial month, the initial day and the initial year. As we said earlier, constructors are declared/defined like normal member functions (with that odd exception that they have no return value and their name must match the name of the class). Therefore, the syntax for declaring/defining a constructor that take parameters should look very familiar.

```C++
class Date {
public:
  Date() {
    month = 1;
    day = 1;
    year = 1;
  }
  
  Date(int starting_month, int starting_day, int starting_year) {
    month = starting_month;
    day = starting_day;
    year = starting_year;
  }
  
...  
};
```

The body of the constructor with three parameters simply takes the arguments that the user passed to the constructor and sets the values of the member variables accordingly. Let's see this in action. When I execute this program

```C++
int main() {
  Date wills_birthday{2, 9, 1982};

  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will wish myself a happy birthday!n";
  return 0;
}
```
I get the following output:

```
On 2/9, I will wish myself a happy birthday!
```

which is exactly what I want!

There's still a problem. Our parameterized constructor does not sanitize the arguments! A user could very easily write a program like this:

```C++
int main() {
  Date wills_birthday{-5000, 9, 1982};

  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will wish myself a happy birthday!n";
  return 0;
}
```

which will produce the following (obviously erroneous) output:

```
On -5000/9, I will wish myself a happy birthday!
```

What are we to do? I have an idea. We can break down our parameterized constructor into two steps:

1. Invoke the default constructor (that will set our member variables to reasonable default values)
2. From the body of the constructor, instead of setting the member variables directly, invoke each of the setter methods for the month, day and year (that will set the member values according to the arguments as long as those arguments are valid)

Great idea. Now, how do we write that in code? It's easy:

```C++
  Date(int starting_month, int starting_day, int starting_year) : Date() {
    setMonth(starting_month);
    setDay(starting_day);
    setYear(starting_year);
  }
```

The `Date()` after the `:` is in the place of the _member initializer list_ and is called the _delegated constructor._  Delegated constructors are something that you will cover in the future. You can read more about it on [C++ Reference,](https://en.cppreference.com/w/cpp/language/constructor) if you are curious. For now, just understand that this is the syntax for letting this constructor temporarily delegate (as the name implies) control (at least temporarily) to a different constructor to do some work before this constructor does its work! In other words, when the user declares/defines an instance of a `Date` class with initial values for the month, day and year, the default constructor will run first (because it is the delegated constructor for the constructor with three parameters) and then the setters in the body of the parameterized constructor are called.

Because the setters properly sanitize their arguments before setting the value of the corresponding member variable, we are guaranteed to prevent invalid values for the month, day or year from creeping in. With this code in place, if we execute

```C++
  Date wills_birthday(-5000, 9, 1982);

  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will wish you a happy birthday!n";
```

we see

```
On 1/9, I will wish you a happy birthday!
```

as the output. Do you understand why we see the `9` but the `-5000` was replaced with a 1?

There is no limit to the number of different constructors you can write for a class. As long as each is declared with a different list of parameters (with respect to the number of parameters or their types -- remember the rules for function overloading?), you can write as many constructors as you like! For instance, I could write this constructor:

```C++
  Date(const Date &other_date) {
    month = other_date.month;
    day = other_date.day;
    year = other_date.year;
  }
```

What is happening here? First, look at the parameter: it's a reference to an instance of a (unchangeable, i.e., `const`ant) `Date` ADT. Next, look at the body: it is setting the values of the member variables of the object being constructed to the values of the member variables of the `Date` ADT instance passed as a parameter. (Brief aside: Why can this code access the member variables of the `other_date` without going through the getters? I thought that those member variables were `private`? They are `private`. But, remember: you can always access private member variables of a class in the implementation of that class!). The result is that this constructor is constructing a new instance of a `Date` ADT that is a _copy_ of an existing Date ADT. We can use it like this:

```C++
  Date february_ninth_nineteeneightytwo(2, 9, 1982);
  Date wills_birthday(february_ninth_nineteeneightytwo);

  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will wish you a happy birthday!n";
  return 0;
}
```

This form of a constructor is so common that C++ developers have given it a name: the _copy constructor_. In order to qualify as a copy constructor, the parameter to the constructor _must_ be a reference to a(n) (optionally constant) variable of the same class. For more details on this restriction, read [C++ Reference](https://en.cppreference.com/w/cpp/language/copy_constructor).

What's more, the copy constructor gets called in some other situations that are unexpected, but useful. For instance, the copy constructor is invoked during the initialization of `wills_birthday`:

```C++
  Date february_ninth_nineteeneightytwo(2, 9, 1982);
  Date wills_birthday{february_ninth_nineteeneightytwo};
```

It is also invoked if you write the same thing with an `=`:

```C++
  Date february_ninth_nineteeneightytwo(2, 9, 1982);
  Date wills_birthday = february_ninth_nineteeneightytwo;
```