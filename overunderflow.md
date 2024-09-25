## What's News

In today's news, researchers uncover what happens when people take the advice to "Go big or go home" too seriously. 

## Overflow and Underflow

Remember the definition of _type:_ the range of valid values of a variable and the valid operations on that variable. For variables that represent numbers, what happens when you try to a value into a variable that is _not_ one of those valid ones? In particular, what happens when you try to store a number into an `int` that is invalid? In cases where the value is too big, you get _overflow_. In cases where the value is too small, you get _underflow_. For example, what is the output of this program?

```C++
#include <iostream>

int main() {
  int bearcat_fans{2147483647};
  std::cout << "bearcat_fans: " << bearcat_fans << "\n";
  bearcat_fans += 1;
  std::cout << "bearcat_fans: " << bearcat_fans << "\n";
  return 0;
}
```

Huh, now this output is odd:

```
bearcat_fans: 2147483647
bearcat_fans: -2147483648
```

Did UC suddenly become very unpopular?

It just so happens that on the computer where we executed this program, 2147483647 is the largest value that can be stored in an `int`-typed variable. Therefore, attempting to store a number bigger than that (which we create with the compound assignment operator [e.g., `+=`]) in the variable results in overflow and the value _overflows_. You can try this program live at [Godbolt](https://godbolt.org/z/acrfhPGPn). 

While that overflow-wraps-around behavior seems rational and stable, it actually is not! In the C++ language, if we are ever to introduce overflow in our programs, we are subject to the laws of undefined behavior. Undefined behavior is a type of program behavior that does not have to follow any rigorous specification. In other words, if we have undefined behavior in our program, it is allowed to do anything!

> Note: If we are operating with *unsigned* numbers (a special type of `int`eger), the wraparound behavior appears again. In that case, however, the behavior is perfectly acceptable and well defined by the standard. Be careful!

Overflow can happen in the opposite direction, too! What is the output of this program?

```C++
#include <iostream>

int main() {
  int bearcat_fans{-2147483648};
  std::cout << "bearcat_fans: " << bearcat_fans << "\n";
  bearcat_fans -= 1;
  std::cout << "bearcat_fans: " << bearcat_fans << "\n";
  return 0;
}
```

I'm starting to expect odd things to happen:

```
bearcat_fans: -2147483648
bearcat_fans: 2147483647
```

You can try _this_ program live at [Godbolt](https://godbolt.org/z/M9aEW8xbY).

These type of "anomalies" are hard to detect and end up being the root cause of many, many security vulnerabilities and hard-to-spot bugs, many of which have very [expensive consequences](https://www.nytimes.com/1996/12/01/magazine/little-bug-big-bang.html)[^1]. Be sure to always be on the lookout for what might happen if your program has to deal with data that stretch the limits of what your variable's types are designed to hold!

[^1]: Overflow and underflow errors can also result in [hilarious, absurd](https://en.wikipedia.org/wiki/Nuclear_Gandhi) outcomes. 