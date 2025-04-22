# Object-Oriented Programming Introduction - Abstract Data Types

## What's News
Today, two of the most iconic brands announced a tie up. Data abstraction and process abstraction issued a joint press release saying that they are going to work together. In other corporate news, McDonald's announced that they would be expanding their partnership by selling Krispy Kreme doughnuts in [more restaurants throughout the country](https://retailwire.com/mcdonalds-krispy-kreme-nyc-bagel-sandwiches/).

## A Data Abstraction, Indeed

In the past several class sessions, we have started to think about the best way to represent data so that we can work with it as "naturally" as possible. In day-to-day life we are _very_ comfortable working with and manipulating objects. If only it were possible to manipulate objects in the computer the same way: by using them in only the way that their operations support and setting/updating their data in a way that only valid values are stored.[^type]

[^type]: That should really sound familiar!

We will learn that to fully have objects in our computer programs that we can manipulate we will need to learn two new features of C++. These features go hand-in-hand. You almost can't have one without the other, but ... we have to start somewhere.

So, we are going to start by learning a new type of abstraction, *data abstraction*. As we have said on several occasions, abstraction is the art and science of removing details to focus on the essence in a given context.

To date we have seen the power of procedural abstraction using functions. Remember that in procedural abstraction we are removing the details (or "hiding") of "how" things are accomplished and presenting users with a simplified interface for achieving a certain behavior. Data abstraction, similarly, hides something, too. It hides information from the user in order to present them with a simplified interface. In case you forgot already, here's another, pithier, definition of abstraction:

> Remembering what's important and forgetting what's not, depending on the context.[^kramer]

[^kramer]: Kramer, Jeff. Is Abstraction the Key to Computing? _Communications of the ACM._ April 2007. Vol. 50, No. 4

Let's think closely about how we would abstract the data about a date -- like, a date on the calendar. In the same way that the user of a function that we write (procedural abstraction) does not care how we accomplish an operation as long as it works correctly, a user of our date data abstraction (tongue twister!) does not particularly care about how we represent that date behind the scenes as long as it represents all the data relevant to a date.

Like we have to give a function safe, proper values for its parameters when we call it in order to be able to rely on its correct operation, a data abstraction is only  guaranteed to work correctly when we give it compatible data.

Okay, where have we heard something like that before? It sounds, to me, like the first half of the definition of a _type_: A range of valid values. Great! Now keep that in mind -- we'll come back to part two of that definition in a minute.

Because it's a data abstraction, the user only cares that it can be used anywhere a date can reasonably be used. It should be possible to compare two dates to see which one came first. It should be possible to compare two dates to determine the time elapsed between them. It should be possible to print a date to the screen for display on a calendar. The list goes on.

What do all those things sound like? That's right, they sound like examples of _valid operations_ that can be performed on a date! Wait, that seems to sounds like part two of the definition of type! 

And, if you thought that, you would be exactly right! In most cases, software developers combine data abstractions (what defines the range of valid values for non-fundamental types -- or [_compound types_](https://en.cppreference.com/w/cpp/types/is_compound) in C++) with procedural abstractions (what defines the valid operations for compound types) to get something called a _class_[^either_or] and a class implements an _abstract data type_ (ADT).

[^either_or]: We will see that the keywords `class` and `struct` can both be used to write classes. I know that it is confusing to see the keyword `struct` and hear it described as a class, but ... well, I don't know what to tell you!

An ADT is conceptual. There is no code associated with an ADT. Programmers and software engineers design ADTs as a way to organize their software into pieces that can interact by invoking the operations of other ADTs. When programmers want to use ADTs to help them organize their software to make it more maintainable, robust, readable, etc., they implement it using a class. As we will see soon, there is a style of software development that treats instances of classes (i.e., implementations of ADTs) as if they are interacting objects. Programmers working in that paradigm are writing in an object-oriented style.

We are getting ahead of ourselves and have talked about plenty of theory. To use an ADT to make our software "better", we will have to implement it somehow. No time to waste: let's go!

## Load-Bearing Walls

The most straightforward way to identify a date is to store the day, the month and the year. At their most basic, we can represent each of these as integers. It would be clunky, but I could write the following code to store my sister's birthday:

```C++

int main() {
  int alis_birthday_month{4};
  int alis_birthday_day{14};
  int alis_birthday_year{1985};
  ...
}
```

However, C++ gives us the `struct` as one of two ways to combine one or more variables of _different_ types to create a _compound_ type (yes, you _can_ combine other compound types to form yet other compound types!).[^different_types] The `struct` declaring a new `Date` _type_ would look like:

[^different_types]: Notice the difference between a _data structure_ (which we usually abbreviate as `struct`) and an array and/or vector. It's really important: An array/vector can only hold a group of elements as long as they are the same type! In a `struct`/`class`, we can store elements in a group together even if they are not the same type!

```C++
struct Date {
  int month;
  int day;
  int year;
};
```

The `int`eger variables `month`, `day` and `year` are known as the _member variables_ of the `Date` type.

That's pretty simple and concise. Now, instead of using three separate variables (one for a day, one for a month and one for a year) to describe, say, my dad's birthday, I can use a single one with a type of `Date`. 

```C++
Date dads_birthday{};
```

The meaning of the code above is to _instantiate_ an _instance_ (yes, I know it's repetitive) of a `Date` type with the name `dads_birthday`. Alternately, we can call `dads_birthday` an _object_ whose type is `Date`. Any developer who instantiates an instance of a class (and classes implement ADTs, remember?!) is called a _user_ of that class and/or ADT.[^overstate] [^objects]

[^overstate]: It is hard to overstate the importance of understanding the concept of _instantiation_ and recognizing the difference between a _type_ and an _instance_ (or _object_). See the following [Sidebar](#sidebar-the-difference-between-types-and-instances) for more!
[^objects]: Recall from our discussion earlier how "we" like to work with objects in real life and we feel comfortable learning about them through their [affordances](https://careerfoundry.com/en/blog/ux-design/affordances-ux-design/)? 

It seems like we might have done too good of a job abstracting the data -- it appears to be completely locked away in the instance. How do we get at the three member variables that are stored inside an instance? The _member access operator_ (`.`), of course.

Let's write a program that wishes my dad a happy birthday:

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
> As a quick reminder, what did we call `dads_birthday`? Yes, an instance (or an object)!

Notice that we can get the value of the `day` member variable of my `dads_birthday` by using the `.` to access that member!

In the C++ program above, `dads_birthday` has the type of `Date`. What does that mean, though? It's analogous  to

```C++
int age{45};
```

where `age` has the type `int`. Yes, what we are learning now gives us the power to make our own types and then declare and define variables that have that type! How cool! When I build and run the program above, it will produce the following output:

```
On 7/18, I will wish him a happy birthday!
```

Exactly what we wanted.

## Sidebar: The Difference Between Types and Instances

To this point in our C++ journey, it has been "okay" to sometimes mixup a variable and its type. After all, the compiler would quickly correct us: we cannot write names of fundamental types in places where we can write the names of variables, and vice versa. Also, it was clear that when our code was doing some declaring/defining, it must have been declaring/defining a variable -- we didn't know that it was even possible to declare our own types.

As we start to gain more confidence in declaring/defining our own types (like the `Date` that we're building), it is really important to understand the distinction between a type and an instance.

Remember from way back, our [definition of a type](./expressions-types.md):

- range of valid values of a value and
- range of valid operations for a value.

When we declare/define a variable we are essentially telling C++ that we will use _this_ name to refer to space in memory that can hold _this_ type of data and _these_ things may be done to it. That memory holds the instance. 

Think about these declaration/definitions:

```C++

int inches_of_rain{12};

int days_of_sunshine{0};

```

How many variables were declared? Yes, two! How many different types were used? That's right, one! The _instances_ `inches_of_rain` and `days_of_sunshine` are governed by the "rules" of the `int` type, but they are completely different from one another. As stated above, _object_ is a great synonym for _instance_. 

Importantly, if you write code that changes the value of `inches_of_rain`, it does _not_ change `days_of_sunshine`. I know that might seem silly: "Will, I'll never mess that up!" Yes, I know! But, when we start working with types that are _not_ as straightforward as `int` and `double`, it _can_ get complicated.

I tend to fall back to this analogy: The type is like the blueprints for a house. Your house (or condo, or apartment) was built _based on_ those plans, but it is different than the plans. Each house (or condo, or apartment) that is based on those plans is a special snowflake. 

What's more, you cannot live in blueprints! But, you can live in your house (or condo, or apartment)!

To properly use the new lingo around your fellow programmer pals, you could say ... 

```C++
Date d{};
```
instantiates a instance, `d`, of type `Date`."[^english]

You could also say ...

```C++
Date d{};
```
instantiates an object, `d`, of type `Date`."

[^english]: Yes, English is awful -- seeing instantiate there twice is a real bummer.

### What Day Is It?

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

Using abstraction was supposed to help us prevent this very situation! We were promised that ADTs' data abstraction power would make it impossible to have these types of goofs.

The reason we are still susceptible to bad users who will set instances of the Date class with bogus values is that we have not implemented the Date ADT in a way that limits access to its member variables from the outside world. Users of the Date class that implements the Date ADT (as written above) may pull back the curtain and assign values to its member variables arbitrarily. In other words, they are allowed to break open the walls that we erected around the way that the internal data of the ADT implementation is stored. Think about the worst-case scenario: an ADT designed to hold the current speed of a vehicle where the programmer using that struct incorrectly sets a very high number.

Fundamentally, the problem is that an ADT implemented like the one above provides the power to build data abstractions but does not give programmers the power to perform _data hiding_.

### Data Hiding

Hiding the member variables of an implementation of an ADT is very important, as we just saw. Ideally, when we write a class we want to make sure that its users can only manipulate instances of that class in a way that maintains the requirements set forth by the ADT that the class is implementing (yes, that _is_ a mouthful!). Fortunately C++ gives us a way to do this! Inside a class declaration we can specify the _accessibility_ of member variables. Moreover, we will see that we can set the accessibility of the valid operations that can be performed on a class (that correspond to the operations of the associated ADT -- but more on that later!).

Let's modify (slightly) our declaration of the implementation of the Date ADT in order to protect it from having its member variable values corrupted:

```C++
#include <iostream>

struct Date {
private:
  int month;
  int day;
  int year;
};
```
`private` is one of three [access specifier keywords](https://en.cppreference.com/w/cpp/language/access) in C++. We will see `public` below and you will learn about `protected` in future classes. Any members that are given `private` accessibility are only accessible from within the implementation of the ADT. Anyone writing code that uses an instance of our Date class _cannot_ access those members directly.

Therefore, when we attempt to compile 

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
date_struct.cpp: In function ‘int main()’:
date_struct.cpp:18:5: error: ‘int Date::month’ is private within this context
   18 |   d.month = 2;
      |     ^~~~~
date_struct.cpp:11:7: note: declared private here
   11 |   int month;
      |       ^~~~~
date_struct.cpp:19:5: error: ‘int Date::day’ is private within this context
   19 |   d.day = 29;
      |     ^~~
date_struct.cpp:12:7: note: declared private here
   12 |   int day;
      |       ^~~
date_struct.cpp:20:5: error: ‘int Date::year’ is private within this context
   20 |   d.year = 2021;
      |     ^~~~
date_struct.cpp:13:7: note: declared private here
   13 |   int year;
      |       ^~~~
```

This outcome is an improvement -- we have prevented malicious users from arbitrarily setting the day, month or year to incorrect values. But, is it really ideal?

Not really. By making these member variables private the user of the Date class is unable to actually use it -- they cannot determine the actual day, month or year that it represents. In other words, we have the power to ensure that every instance contains valid values and that the user cannot do anything sneaky. What's missing is a programmatic, safe way to allow users of the implementation of the Date ADT to update the day, month and year. We need a mediator.

## Data Access

How to solve this problem? I have an idea: If we have member variables, I bet that C++ will give us the chance to write _member functions_! And, it does!

_Member functions_ are special functions that "belong" with a class. Among other special wizardry, member functions are privileged with the power to access all of the member variables of the data structure to which it belongs -- public, protected _and_ private!

That's not where the fun ends, though. A member function also gets something else!

Before we made the member variables of the Date class private, we could write

```C++
Date x{};
std::cout << x.day << "\n"; 
```

to access `x`'s day. That `day` belongs to the instance of the Date class named `x`. There is no `day` "out there" on its own without an associated instance of a Date class. In other words, we could not have written:

```C++
Date x{};
std::cout << day << "\n";
```
[^otherwise]

[^otherwise]: Of course we could write that if we had declared/defined a variable named `day`, but you get the idea!

Does that have a bearing on member functions the way that it does on member variables? The answer is _yes_! When you invoke a member function (call it `get_day`), you _cannot_ write

```C++
Date x{};

auto x_date = get_day();
```

`get_day` is a member function ... we need to say of what it was a member! In other words, for every invocation of a member function, there must be an "associated" instance:

```C++
Date x{};
auto x_date = x.get_day();
```

(In the case above, the associated instance is `x`).

How does that help us write our member functions? Well, in your member function implementations you can assume that you have implicit, easy access to that instance. When you are writing the code of a member function, any reference you make to member variables will access/update the value of those member variables specific to that associated instance. How cool is that?

> Note: It is important to realize that _member functions_ can define an implementation for any type of operation that the ADT designer wants to make available to users of their compound type. Here, however, we are focusing on a particular kind of operation for the sake of the example. That is not to say that writing mediator functions (like the ones that we are about to write) is not common. In fact, it is very, very common. 

Now, let's return to writing these mediator functions that are going to let the user of the Date class safely update and access it. If we write these member functions correctly, we will always be able to guarantee the member variables' values stay reasonable and that the _actual_ data that abstract away from the prying eyes of users will follow the conceptual rules of the associated ADT. 

We will have two kinds of these mediator member functions: one kind for the class users' to _set_ the values of the member variables (_setter member functions_) and one kind for the class users' to _get_ the values of the member variables _(getter member functions)._ Let's first write the getter member functions -- they're easier:


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

Let's say that we have a Date class object that holds my birthday (`wills_birthday`). We could rewrite the `std::cout` from above that wished my dad a happy birthday by using getter member functions:

```C++
  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will be a year older!\n";
```

See how you can use these getters just like you were accessing member variables, albeit with a slightly different syntax? That's really cool!

Now, let's give the Date class users the opportunity to change the values of member variables by writing setters:

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

Now we have all the functionality that we had before, and protection! Wait, really? The setter functions don't actually "sanitize" the caller's input -- they just mindlessly take what the caller passes as an argument and set the member variables. So, we are still stuck with the possibility that the Date class user does something stupid like:

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

If we were to use this version of the Date class in our code,

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

See how the `Date` class says, "You want to assign to me a bogus year? Well, I'm not going to do it!"?

Now we are getting somewhere.

## The Difference Between a Class and a Struct

We hinted above that there is a second way to declare implementations of ADTs in C++. In addition to the `struct` keyword, we can use the `class` keyword. In fact, it is more common to see ADT implementations declared with the `class` keyword than with the `struct` keyword.

In _almost_ every way, `class` and `struct`-declared classes are identical. There is only one small difference between a class declared with the `class` keyword and a class declared with a `struct` keyword: By default, all the member variables and the member functions in an ADT implemented with `struct` have `public` accessibility and all the member variables and the member functions in an ADT implementation whose declaration/definition begins with `class` have `private` accessibility. To see the implications of these defaults, let's simply copy and paste the newest version of the Date ADT implementation declaration and change the `struct` to `class`:

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
date_struct.cpp:44:17: error: ‘void Date::setYear(int)’ is private within this context
   44 |   d.setYear(2021);
      |                 ^
date_struct.cpp:30:8: note: declared private here
   30 |   void setYear(int new_year) {
      |        ^~~~~~~
date_struct.cpp:45:19: error: ‘void Date::setYear(int)’ is private within this context
   45 |   d.setYear(-10000);
      |                   ^
date_struct.cpp:30:8: note: declared private here
   30 |   void setYear(int new_year) {
```

Our use of the `class` keyword to implement the ADT means that everything (i.e., member functions and variables) is `private`, at least by default. We are feeling the impact of our decision to use `class` here: our getter and setter member functions are no longer `public` and, therefore, cannot be invoked on instances of the Date class. The good news is that there is a simple fix -- we'll just add the `public` access specifier above the declaration/definition of the getter/setter member functions:

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

As a brief aside, it is important to understand the persistence of access specifiers in a `struct` or `class`. The level of access granted by the use of an accessibility specifier remains in force until the next access specifier. For example, in

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

Way back when (a few paragraphs ago), we had the simple definition of the Date ADT implementation:

```C++
struct Date{
  int month;
  int day;
  int year;
};
```

Then, to create an instance of a Date class we could simply write

```C++
  Date d{2,9,1982};
```
That code declared _and_ initialized `d` at the same time.

If we tried to write that same syntax with the code for the more robust Date ADT implementation that we have now, the compiler would be angry with us. Once we declared that certain variables are private, we could no longer declare and initialize a Date class using the _aggregate initialization_ syntax from above. Does it mean that we are stuck writing code that looks like

```C++
  Date d{};
  d.setMonth(2);
  d.setYear(9);
  d.setYear(1982);
```

instead? Of course not -- that's _way_ too much typing. In fact, C++ gives us the power to declare/initialize classes in much more powerful ways than we could above! That power comes from special member functions defined in classes that are called _constructors_. They are called constructors because they are used to, well, construct instances of `class`es and `struct`s that implement ADTs. The code that you write in a constructors is executed when an instance of that class is, uhm, constructed. Most programmers use the opportunity to have code execute at the moment of instantiation in order to initialize member variables and do other work that must precede all other operations.

The most basic form of a constructor is called the _default_ constructor. The default constructor for a class gets invoked when an instance of the class is declared/defined without specifying any special starting values. For the Date ADT implementation Date class, that would look something like:

```C++
  Date d{};
```

To reiterate, the code above would trigger the execution of the default constructor for the Date class, if one existed.[^always]

[^always]: Note, it [_always_](https://en.cppreference.com/w/cpp/language/default_constructor) does exist, we just might not write it ourselves or even see it!

A constructor is like any other member function, expect for when it's not!

First things first: it's declaration has a special syntax. Like a member function, the declaration of the constructor goes inside the declaration of the class implementing the ADT. However, the declaration of the constructor does not have a return type and its name must match exactly the name of the class. Here's how you declare/define the default constructor for the Date class:

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

Again, the utility of a default constructor is to set the class's member variables to reasonable initial values, even if the user of the class did not when they instantiated an object. The implementation of the Date class's default constructor shown above did, well, nothing -- it certainly did _not_ initialize the member variables, that's for sure. As a result, the member variables were left in an uninitialized state and, as a result, we got the funny output shown above.

Let's fix this by actually writing a meaningful implementation of the default constructor of the Date class:

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

Wohoo! We still don't quite have the old functionality back, though. We wanted to be able to declare/define an instance of a Date class and set it to a particular date all in one line. In order to do that we can write another constructor -- this one will take three parameters: the initial month, the initial day and the initial year of the about-to-be-created instance of a Date class. As we said earlier, constructors are declared/defined like normal member functions (with that odd exception that they have no return value and their name must match the name of the class). Therefore, the syntax for declaring/defining a constructor that takes parameters should look very familiar.

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

The body of the constructor with three parameters simply takes the arguments that the user passed to the constructor and sets the values of the member variables accordingly.[^overloading] Let's see this in action. When I execute this program

[^overloading]: Just how does C++ tell the difference between those two functions? I mean, we would be in real trouble if C++ let us declare two variables with the same name _but_ different types? Well, that's the key: remember function overloading!

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

The `Date()` after the `:` is in the place of the _member initializer list_ and is called the _delegated constructor._  Delegated constructors are something that you will cover in the future. You can read more about it on [C++ Reference,](https://en.cppreference.com/w/cpp/language/constructor) if you are curious. For now, just understand that this is the syntax for letting this constructor temporarily delegate (as the name implies) control (again, at least temporarily) to a different constructor to do some work before this constructor does its work! In other words, when the user declares/defines an instance of a Date class with initial values for the month, day and year, the default constructor will run first (because it is the delegated constructor for the constructor with three parameters) and then the setters in the body of the parameterized constructor are called.

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

There is no limit to the number of different constructors you can write for a class. As long as each is declared with a different list of parameters (with respect to the number of parameters or their types -- remember the rules for function overloading? Well, they apply here, too!), you can write as many constructors as you like! For instance, I could write this constructor:

```C++
  Date(const Date &other_date) {
    month = other_date.month;
    day = other_date.day;
    year = other_date.year;
  }
```

What is happening here? First, look at the parameter: it's a reference to an instance of a (unchangeable, i.e., `const`ant) Date class. Next, look at the body: it is setting the values of the member variables of the object being constructed to the values of the member variables of the Date instance passed as a parameter.[^access_aside] The result is that this constructor is constructing a new instance of a Date that is a _copy_ of an existing instance of a Date. We can use it like this:

[^access_aside]: Brief aside: Why can this code access the member variables of the `other_date` without going through the getters? I thought that those member variables were `private`? They are `private`. But, remember: you can always access private member variables of a class in the implementation of that class!

```C++
  Date february_ninth_nineteeneightytwo{2, 9, 1982};
  Date wills_birthday{february_ninth_nineteeneightytwo};

  std::cout << "On " << wills_birthday.getMonth() << "/"
            << wills_birthday.getDay()
            << ", I will wish you a happy birthday!n";
  return 0;
```

This form of a constructor is so common that C++ developers have given it a name: the _copy constructor_. In order to qualify as a copy constructor, the parameter to the constructor _must_ be a reference to a(n) (optionally constant) variable of the same class. For more details on this restriction, read [C++ Reference](https://en.cppreference.com/w/cpp/language/copy_constructor).

What's more, the copy constructor gets called in some other situations that are unexpected, but useful. Obviously (as we _just_ saw), the copy constructor is invoked during the initialization of `wills_birthday`:

```C++
  Date february_ninth_nineteeneightytwo{2, 9, 1982};
  Date wills_birthday{february_ninth_nineteeneightytwo};
```

However, it is also invoked if you write the same thing with an `=`:

```C++
  Date february_ninth_nineteeneightytwo(2, 9, 1982);
  Date wills_birthday = february_ninth_nineteeneightytwo;
```

Wild!