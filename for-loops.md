
### For Loops

The while and the do ... while loops are both conditional loops. What features are available in C++ for the programmer that wants to perform an operation a fixed number of times? That's where the for loop comes to the rescue!

The for loop is known as a _count-controlled loop_ because its body is executed a certain number of times. The number of times that its body executes is determined according to a variable known as a _counter variable_. The syntax of a for loop is complicated, but it's operation is not complex. The general format of a for loop is

![Untitled drawing(4).png](https://uc.instructure.com/courses/1533374/files/155681385/preview)

The _initialization_ statement (0) executes first -- hence its 0th position in execution order. The initialization statement is _the_ place to initialize the counter variable. The test expression (1) executes second and should use the value of the counter variable to determine whether or not to execute the body of the loop. If the test expression evaluates to true, then the body of the loop (2) will execute; otherwise, the loop terminates. After the body of the loop executes, the update expression (3) is performed. The update expression is _the_ place to change the value of the counter variable. After the update expression is performed, control returns to the test expression (1) and the process repeats. In other words, the steps of execution looks like this:

0 1 2 3 1 2 3 1 2 3 1 2 3 ... 1 2 3

Notice that the initialization statement is only executed one time!

Test your understanding: Is the for loop a pre-test or a post-test loop?

Let's write a loop that will print the numbers 1 through 10 on the screen:

#include <iostream>

int main() {
  int i{0};

  for (i = 0; i<10; i++) {
    std::cout << i+1 << "\\n";
  }
}

There are several important things to notice about this code snippet:

1.  The value of the counter variable (`i`) is only updated one place -- the update "position". (Test your understanding: Why isn't the `i+1` in the body changing `i`?). **This is really, really crucial**. Although it is _possible_ to update the value of `i` in the body of the for loop, it is generally considered to be bad practice. As a programmer, you must have a _very good reason_ for updating the counter variable of a for loop somewhere other than the update position.
2.  The value of the counter variable ranges from 0 to 9, inclusive. C++ programmers generally start counting at 0. When we study arrays you will find out why they are 0 zealots.
3.  It looks like the value of `i` is being initialized in two places -- first where it is declared/defined and second in the initialization position.
4.  The scope of the counter variable `i` is the entire `main` function.

Let's think about (2) and (3) together. There are plenty of good reasons why we would want to be able to access the value of the counter variable outside the for loop. But, there are also some really good reasons why we do _not_ want to be able to access the value of the counter variable outside the for loop. When the latter is the case, then we can do something really tricky in the initialization position of the for loop:

#include <iostream>

int main() {
  for (int i{0}; i<10; i++) {
    std::cout << i+1 << "\\n";
  }
}

Look closely at how we are able to declare/define/initialize the counter variable `i` all within the initialization position of the for loop. One important consequence is that the counter variable is in the scope of the body of the for loop. Mind. Blown. This technique for declaring/defining/initializing all in the initialization position of the for loop is very common.

Like the while loop, note that there is no semicolon (`;`) after the closing parenthesis after the update expression. This is very important!

The for loop and the while loop are very commonly used by professional programmers. The do ... while loop is not as common.

### To Infinity and Beyond

Any loop that does not terminate is known as an _infinite loop_. I won't give you any examples of infinite loops because, if you are like me, you'll find them all on your own -- and when you least expect it!

### Classes and Objects

Remember the term abstraction? We defined it as a way of hiding something! We introduced abstraction when we were talking about functions because functions hide the process of doing something from the person who wants that action completed.

C++ is what computer scientists call an object-oriented programming language (OOP). That means that the language is designed for efficiently defining/processing/manipulating objects. 

Well, what is an _object_? Just like a function is an abstraction, an object is an abstraction. However, Instead of just hiding a process (like a function), it hides data, too. More than that, an object groups together (_encapsulates_) that hidden data with a set of actions that can change that data. There are all sorts of advantages of writing programs this way and we will discuss them in additional detail later in the course.

A class is related to an object. A class is the way to declare/define the data and processes that are encapsulated in an object. Some examples will definitely help!

Dogs and cats are classes. Fido and Luna are objects of the class type Dog and Cat. When programmers create objects from classes the process is known as _instantiation_. The Dog and Cat classes declare data (like the number of legs the pet has and the color of its fur) and actions that it can perform (e.g., making a noise). Because Fido and Luna are objects of the Dog and Cat type, respectively, they contain all the data and can perform all the actions defined by Dog and Cat. 

Generally, we say that objects are related to classes by the "is a" relationship: Fido is a dog; Luna is a cat. What are some others? A Tesla is a Car. An iPad is a Tablet.

The actions defined by the class are called _methods_ and look almost exactly like functions. The only difference is that when they are called they are associated with an object. And, because of that association, a method can access data from that object during its execution.

What proceeded was a very, very rapid introduction to OOP and we will come back to it in more detail much later. However, we needed the vocabulary as prerequisite for file operations.