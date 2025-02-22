## What's News

Homonyms are in style. In today's Personal Programmer section, our fashion editors explore the way that four homonyms provide foresight to how language will evolved as we prepare for the next several years.

## For Loops

The `while` and the `do ... while` loops are both conditional loops. What features are available in C++ for the programmer that wants to perform an operation a fixed number of times? That's where the `for` loop comes to the rescue!

The `for` loop is known as a _count-controlled loop_ because its body is executed a certain number of times -- the count! The count is determined according to a variable known as a _counter variable_. The syntax of a `for` loop is complicated, but it's operation is not complex. The general format of a `for` loop is

![](./graphics/The%20For%20Loop.png)

The _initialization_ statement (0) executes first -- hence its 0th position in execution order. The initialization statement is _the_ place to initialize the counter variable. The test _expression_ (1) executes second and should use the value of the counter variable to determine whether or not to execute the body of the loop. If the test expression evaluates to true, then the body of the loop (2) will execute; otherwise, the loop terminates. After the body of the loop executes, the _update_ expression (3) is performed. The update expression is _the_ place to change the value of the counter variable. After the update expression is performed, control returns to the test expression (1) and the process repeats. In other words, the steps of execution looks like this:

```
0 1 2 3 1 2 3 1 2 3 1 2 3 ... 1 2 3
```

Notice that the initialization statement is only executed one time!

Like the `while` loop, note that there is no semicolon (`;`) after the closing parenthesis after the update expression. This is very important!

The `for` loop and the `while` loop are very commonly used by professional programmers. The `do ... while` loop is not as common.

> Is the for loop a pre-test or a post-test loop?

## All For One (Example of Using a For Loop)
Let's write a loop that will print the numbers 1 through 10 on the screen:

```C++
#include <iostream>

int main() {
  for (int i{0}; i<10; i++) {
    std::cout << i+1 << "\\n";
  }
}
```

```console
1
2
3
4
5
6
7
8
9
10
```
(You can play with this code online at [https://godbolt.org/z/1fvW8E5WW](https://godbolt.org/z/1fvW8E5WW).)

Some analysis will help!

| Loop Part | `i` | Notes |
| -- | -- | -- |
| Step 0 | `0` | `i` is just being declared and will receive the initial value of `0`. |
| Step 1 | `0` | Because `i` is `0`, `0 < 10` is true and the body will execute. |
| Step 2 | `0` | The body of the loop executes and ... the value of the expression `i+1` (i.e., `1`) is printed to the screen.|
| Step 3 | `0` | The value of `i` is incremented by `1`. |
| Step 1 | `1` | Because `i` is `1`, `1 < 10` is true and the body will execute. |
| Step 2 | `1` | The body of the loop executes and ... the value of the expression `i+1` (i.e., `2`) is printed to the screen.|
| Step 3 | `1` | The value of `i` is incremented by `1`.|
| Step 1 | `2` | Because `i` is `2`, `2 < 10` is true and the body will execute. |
| Step 2 | `2` | The body of the loop executes and ... the value of the expression `i+1` (i.e., `3`) is printed to the screen.|
| Step 3 | `2` | The value of `i` is incremented by `1` |
| ... | ... | ... |
| Step 3 | `9` | The value of `i` is incremented by `1` |
| Step 1 | `10` | Because `i` is `10`, `10 < 10` is false and the body will _not_ execute and the loop ends. |

## A For Loop Scope Sidebar

In [past issues](./ScopeSupplement.md) of the C++ Times we discussed the rules for the scope of a variable. We said that _usually_ the scope of a variable begins at the nearest `{` and the matching `}` enclosing the variable's declaration. A variable is _local_ to the scope where it is declared (exclusive of any enclosed scopes).

But, _usually_? The `for` loop in C++ is one example of why we have to say _usually_. 

A variable declaration/definition is _allowed_ in the initializer position.[^required]. Despite the fact that execution did not pass a `{`, if there is a declaration/definition in that position, then the variable being declared is in a scope that encompasses the entire `for` loop (including code in the test expression and the update expression!).

What, exactly, does this have to do with the way that we write code for a `for` loop?

It means that if we attempted to use `i` after the body of the `for` loop, we would get a syntax error. The code below would not compile for exactly this reason.

```C++
#include <iostream>

int main() {
  for (int i{0}; i<10; i++) {
    std::cout << i+1 << "\n";
  }
  std::cout << "We ended with i at " << i << "\n";
  return 0;
}
```

The scope of any variable declared in the initializer is also important because it helps us understand why the value of the variable declared/defined in the initializer spot (if any!) maintains its value between iterations. Look at how the value of `i` maintains its value between iterations and the value of `j` starts every iteration at `0`:

```C++
#include <iostream>

int main() {
  for (int i{0}; i<10; i++) {
    int j{0};
    std::cout << "i: " << i << "\n";
    std::cout << "j: " << j << "\n";
  }
  return 0;
}
```

```console
i: 0
j: 0
i: 1
j: 0
i: 2
j: 0
i: 3
j: 0
i: 4
j: 0
i: 5
j: 0
i: 6
j: 0
i: 7
j: 0
i: 8
j: 0
i: 9
j: 0
```

Wow! Because `j` is declared/defined in the scope of the body of the `for` loop, every time C++ passes the `}` ending the body, `j`'s local scope is destroyed and, by extension, `j` itself (along with any other variables that are local to that scope). If the loop performs another iteration, then execution reenters that scope and a new `j` (along with other variables local to that scope) gets a life.

See the image below for a visual depiction of the extent of each of the scopes that we are discussing.

![](./graphics/The%20For%20Loop%20-%20Scope.png)

The scope with the extent indicated by the dashed orange line is the scope that contains the `i` declared in the initializer. See how the execution of the program never "leaves" that scope while the `for` loop executes? That should help make it a little more obvious why `i` maintains its value between every iteration of the loop. See how the execution of the `for` loop leaves the local scope of `j` (shown in pink) after every iteration? That should help make it a little more clear why there is a new `j` for every iteration!

[^required]: It is not required, however. You can simply put an assignment statement there if that is the initialization operation that you need to perform for your `for` loop to be correct.

## A Variable On the Loose

What if we _did_ have reason to use the counter variable's value after the `for` loop terminated?[^goodreason]

[^goodreason]: Yes, there _are_ plenty of good reasons why you might want to write code in this way!

```C++
#include <iostream>

int main() {
  int i{0};

  for (i = 0; i<10; i++) {
    std::cout << i+1 << "\\n";
  }
  std::cout << "(after the loop) i: " << i << "\n";
  return 0;
}
```

```console
1
2
3
4
5
6
7
8
9
10
(after the loop) i: 10
```

| Loop Part | `i` | Notes |
| -- | -- | -- |
| Step 0 | `0` | `i` is just being assigned the value `0`. |
| Step 1 | `0` | Because `i` is `0`, `0 < 10` is true; the body will execute. |
| Step 2 | `0` | The body of the loop executes and ... the value of the expression `i+1` is printed to the screen.|
| Step 3 | `0` | The value of `i` is incremented by `1`. |
| Step 1 | `1` | Because `i` is `1`, `1 < 10` is true and the body will execute. |
| Step 2 | `1` | The body of the loop executes and ... the value of the expression `i+1` is printed to the screen.|
| Step 3 | `1` | The value of `i` is incremented by `1`.|
| Step 1 | `2` | Because `i` is `2`, `2 < 10` is true and the body will execute. |
| Step 2 | `2` | The body of the loop executes and ... the value of the expression `i+1` is printed to the screen.|
| Step 3 | `2` | The value of `i` is incremented by `1` |
| ... | ... | ... |
| Step 3 | `9` | The value of `i` is incremented by `1` |
| Step 1 | `10` | Because `i` is `10`, `10 < 10` is false and the body will _not_ execute and the loop ends. |

Although there is not much interesting being computing in this `for` loop, it does highlight what we thought was the case: The loop terminates the first time that the test expression is evaluated and `i < 10` evaluates to false. Because the counter variable in the loop written above increases [monotonically](https://en.wikipedia.org/wiki/Monotonic_function) from 0, `i` is `10` the first time that the evaluation of the test expression is false. At least that's what our analysis says. 

No, in fact, our experience shows it, too! That's one of the great things about computer science: We have the ability to verify what we've been told by seeing it with our own eyes.

### To Infinity and Beyond

Any loop that does not terminate is known as an _infinite loop_. I won't give you any examples of infinite loops because, if you are like me, you'll find them all on your own -- and when you least expect it!