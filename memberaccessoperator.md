## What's News

Once spaces reserved for only the VIP-est of the VIPs, clubs like Soho House and Ned NoMad are popping up in cities around the world and offering more access to regular folks than traditional members-only clubs. The pandemic has raised awareness of the importance of in-person, human contact and well-heeled real estate operators with money to invest see a big business opportunity.

## Member Access Operator Details

By now we are very, _very_ familiar with the `.` class member access operator and we are increasingly becoming comfortable with the `->` class member access operator. Although they both help us write code to access members in an instance of a data structure, it is important to thoroughly understand when to use each one.

Let's first think back to the ubiquitous `.` and make a list of the places that we use it:

1. For accessing the members of an object instantiated from a `struct`.
2. For accessing the public member variables of a `class` (truth be told, declaring member variables of a `class` to be public is not always the best software-engineering decision!).
3. Calling member functions on objects instantiated from `class`es.[^others]

[^others]: There are others places that the member access operator is used in C++ besides those mentioned here. You can read all the details at [C++Reference](https://en.cppreference.com/w/cpp/language/operator_member_access.html).

What we left unsaid is that the `.` is used to perform these operations _only_ when the objects are automatic -- i.e., allocated on the stack. The `.` is sometimes referred to as the _member of object_ access operator.[^tech]

[^tech]: Technically, both the `.` and the `->` are referred to as class member access operators. But, because they operate on entities of different value classes, it's nice to have distinguished names sometimes.

Consider a program like

```C++
#include <iostream>

class A {
  public:
    int a;
    void printA() const {
      std::cout << "a: " << a << "\n";
    }
};

struct S {
  int a;
};

int main() {
  A a{};
  S s{1};

  std::cout << "a.a: " << a.a << "\n";
  a.printA();
  std::cout << "s.a: " << s.a << "\n";

  return 0;
}
```
[See it live](https://godbolt.org/z/cYTGjT6K9)

`a` and `s` are both automatic variables. Therefore, it is correct to use the `.` in order to access their member variables and call their member functions.

Before determining just how the `->` relates to the `.`,  let's take a brief digression into the world of board games. There is a game called [Chutes and Ladders](https://en.wikipedia.org/wiki/Snakes_and_ladders). In the game, if your piece lands on a space where there is a ladder, your piece climbs the ladder and makes a quick advance up the board. If your piece lands on a space where there is a chute, your piece will slide down the chute and you lose hard-won progress. When I was a kid playing the game and I landed on a space with either a chute or a ladder, I always liked to think that it took a little extra shove for my piece to go up the ladder or down the chute once it landed on the square.

How does that relate to C++?

Consider an automatic variable named `ptr_to_int` declared and initialized like

```C++
  int *ptr_to_int = new int{0};
```
As a result of that declaration and initialization, part of the memory space of our program might look like this:

![A visualization of the memory space of a program that declares and defines an automatic member variable that points to an `int` in the heap.](graphics/PointerToInt.png)

Think about `ptr_to_int` as a spot on the Chutes and Ladders gameboard connected to either a chute or a ladder. A `*` before the `ptr_to_int` is like pushing our game token up the ladder or down the chute and the ultimate destination is the target of the pointer. Using a `*` is called _dereferencing_ a pointer. Dereferencing a pointer is a very important operation in C++ to understand.

To change the value in the green box pointed to by the purple arrow, we use the `*` as in

```C++
  *ptr_to_int = 5;
```

![A visualization of the memory space of a program that declares and defines an automatic member variable that points to an `int` in the heap after the target value of that pointer has been updated to a new value (5).](./graphics/PointerToInt-TargetValueChanged.png)

On the other hand, to change the target of the pointer itself (i.e., where the ladder or chute leads), we omit the `*` as in

```C++
  ptr_to_int = new int{2};
```

![A visualization of the memory space of a program that declares and defines an automatic member variable that points to an `int` in the heap after the target of that pointer has been updated to a newly allocated `int`.](graphics/PointerToInt-TargetChanged.png)

Why the discussion of the `*` when we were supposed to be talking about the `->`? Because they are fundamentally equivalent.

In short, when we are dealing with dynamic (as opposed to automatic) variables that are instances of `struct`s and `class`es, use the `->` instead of the `.` we used when those were automatic variables. For instance,

```C++
#include <iostream>

class A {
  public:
    int a;
    void printA() const {
      std::cout << "a: " << a << "\n";
    }
};

struct S {
  int a;
};

int main() {
  A *a = new A{};
  S *s = new S{1};

  std::cout << "a->a: " << a->a << "\n";
  a->printA();
  std::cout << "s->a: " << s->a << "\n";

  return 0;
}
```
[See it live](https://godbolt.org/z/e7h3r75M4)

For those who want to go a little deeper, the `->` is equivalent to first dereferencing the pointer to the instance of `struct` or `class` allocated on the heap and then using the `.`.

In other words, we could rewrite the program above like

```C++
#include <iostream>

class A {
  public:
    int a;
    void printA() const {
      std::cout << "a: " << a << "\n";
    }
};

struct S {
  int a;
};

int main() {
  A *a = new A{};
  S *s = new S{1};

  std::cout << "(*a).a: " << (*a).a << "\n";
  (*a).printA();
  std::cout << "(*s).a: " << (*s).a << "\n";

  return 0;
}
```
[See it live](https://godbolt.org/z/vx9PWs9h1)

and get exactly the same result.

Please note: the `(*`...`).`... syntax is rarely used in production code and shown here only to highlight the fundamental similarity between the `->`, the `.` and the `*`.