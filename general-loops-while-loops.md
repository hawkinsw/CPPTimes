## What's News
For decades and decades Western society has assumed that Einstein was correct. Not about what he said regarding quantum mechanics, but about what he said about doing the same thing repeatedly and expecting a different result: ["Insanity is doing the same thing over and over and expecting different results."](https://www.scientificamerican.com/article/einstein-s-parable-of-quantum-insanity/) Today, historians announced that his famous observation may not be true after all.

## Loops: Doing the Same Thing Over and Over Again

In order to solve certain problems, it is sometimes necessary to perform an action repeatedly until a condition is met. For example, we may repeatedly ask the user to enter their password until it matches the one that we have on file:

1.  Ask for the user's password
2.  Until the password is correct, repeat (1).

Or, if we are writing a game, we might need to calculate the total score based on the score of each round:

1.  Initialize the total score to 0.
2.  Start with the first round.
3.  Add the current round's score to the total score.
4.  While there are still rounds to score, advance to the next round and repeat (3). 

These are just two pseudocode examples of the many, many uses of _loops,_ programming structures that cause a group of statements to repeat. The group of statements to be repeated by a loop is called the _body_ of the loop. Programmers often say that the body is "in" a loop. We call every repeated execution of the body of a loop an _iteration_. Loops are an incredibly important tool for programmers -- almost as important as the if statement. There are three different loops: the `for` loop, the `while` loop and the `do...while` loop. Each of these loops exists for a reason and solves a particular type of problem. It is important to understand when to use each of the loops.

### The `do...while` loop

The `do ... while` loop is the oddball among the three types of loops. The `do...while` loop's syntax looks like this:

![The general format of the `do ... while` loop.](./graphics/The%20Do%20While%20Loop.png)

The placeholder `statement`s listed in green represent the body of the loop and can be any valid C++ code. The semantics of the `do...while` loop are relatively straightforward: Repeatedly execute the body while `expression` evaluates to `true`. Remember that the type of the value of `expression` does not necessarily need to be a `bool`. C++ will convert the value of `expression` to a `bool` if it is some other type (as long as such a conversion is possible!). When you rely on this behavior to control a loop, be sure to think about the algorithm that C++ uses to determine whether a value with non-`bool` type is `true` or `false`.

The statements in a `do...while` loop are always executed at least once. Can you see why? Because the condition is only checked at the end of each iteration. Because the check is at the end of the iteration, the `do...while` loop is called a _post-condition loop_. 

Because the body of the `do...while` loop is always done at least once, the `do...while` loop is particularly suited for _input validation_, the process of determining whether input to a program meets requirements. If the program didn't ask for input at least once, it would have nothing to validate! Let's go back to the password validation example from above where the pseudocode looked like this:

1.  Ask for the user's password
2.  Until the password is correct, repeat (1).
    
We will flip the "until" on its head and rewrite the pseudocode like this:

1.  Ask for the user's password
2.  While the password is incorrect, repeat (1).

Let's assume that we have a function called `valid_password` that takes a single parameter, the user's password. `valid_password` will return true when the password is valid and false otherwise. The `do...while` loop that we use to implement the flipped pseudocode algorithm above would look like this:

<html><head></head><body><pre>
 1 #include &lt;iostream&gt;
 2 
 3 bool valid_password(std::string password_to_check) {
 4   ...
 5 }
 6 
 7 <font color=green>int</font> main() {
 8   std::string entered_password{<font color=red>""</font>};
 9   <font color=green>do</font> {
10     std::cout << <font color=red>"Please enter your password: "</font>;
11     std::cin >> entered_password;
12   } <font color=green>while</font> (!valid_password(entered_password));
13   ...
14   <font color=green>return</font> <font color=red>0</font>;
15 }
</pre></body></html>


Look closely at the condition for terminating the `do...while` loop. Convince yourself that it is the correct condition: When the user's password is invalid, `valid_password` returns `false` which means that the program should prompt the user to enter their password again. But the `do...while` loop only repeats the execution of its body when the condition is true! So, we use the `!` operator to flip the result of the validation function. Voila! Our loop does our bidding!

We will refer to the `do ... while` loop as a _conditional_ loop. I wonder why? Think about the meaning or function of a `do ... while` loop. The structure gives the programmer to power to repeatedly execute a set of statements as long as a condition (encoded in the `expression`) is `true`. See it? _condition_? Exactly!

### While Loops: The Variation on the Theme

The `while` loop is another type of conditional loop. The `while` and the `do ... while` loop are very similar but there is one very important difference: the `do ... while` loop is guaranteed to execute its body _at least one time_ but the `while` is not.

Here is the general format of the `while` loop:

![TODO](./graphics/The%20While%20Loop.png)

The placeholder `statement`s listed in green represent the body of the `while` loop and can be any valid C++ code. The semantics of the `while` loop are relatively straightforward: 

1. Upon encountering the `expression` at the top of the `while` loop for the first time, evaluate it.
2. If the value is `true`,
   1.  execute the body of the loop; 
   2.  evaluate `expression`;
   3.  if the result is `true`, return to (2.1); 
   4.  otherwise, continue executing the program at the first statement after the body of the loop.
3.  otherwise, skip the body of the loop entirely.

Not to sound like a broken record, but remember that the type of the value of `expression` does not necessarily need to be a `bool`. C++ will convert the value of `expression` to a `bool` if it is some other type (as long as such a conversion is possible!). When you rely on this behavior to control a loop, be sure to think about the algorithm that C++ uses to determine whether a value with non-`bool` type is `true` or `false`.

Unlike the `do ... while` loop, the statements in the body of `while` loop are not always executed. Can you see why? Because the loop's condition (embedded in the `expression`) is checked at the beginning of each iteration, the `while` loop is called a _pre-condition loop_. 


### Use It Or Lose It

One of the adages about learning a language is that if you don't use your skills then you will lose your skills, if you even had them in the first place.

So, why don't we practice solving some real-world problems with our new tools!

We'll start by using the `do ... while` loop to calculate a student's lab grades. We will make several simplifying assumptions to make the calculations more straightforward:

1. The labs are all out of 100 points.
2. The user enters their input without error.
3. The user always enters at least one grade.

<html><head></head><body><pre>
 1 #include &lt;iomanip&gt;
 2 #include &lt;iostream&gt;
 3 
 4 <font color=green>int</font> main() {
 5   <font color=green>double</font> current_score{<font color=red>0.0</font>};
 6   <font color=green>double</font> total_pts{<font color=red>0.0</font>};
 7   <font color=green>int</font> lab_num{<font color=red>0</font>};
 8 
 9   <font color=green>do</font> {
10     std::cout << <font color=red>"Enter lab score (or -1 if there are no more scores): "</font>;
11     std::cin >> current_score;
12 
13     <font color=green>if</font> (current_score >= <font color=red>0</font>) {
14       total_pts += current_score;
15       lab_num++;
16     }
17   } <font color=green>while</font> (current_score >= <font color=red>0</font>);
18 
19   <font color=green>auto</font> grade = (total_pts) / lab_num;
20   std::cout << <font color=red>"Grade: "</font> << std::fixed << std::setprecision(2) << grade
21             << <font color=red>"%\n"</font>;
22 
23   <font color=green>return</font> <font color=red>0</font>;
24 }

</pre></body></html>

In this version of the program we are using a `do ... while` loop. That is safe because we are allowed to assume (from above) that the user enters at least one valid grade for the student. 

Look closely at the code above and think about the problem that may arise if this assumption did not hold. That's right! You could end up with a situation where you have `lab_num` set to `0` and use it as the dividend in a mathematical operation. In "Math World", division by 0 is not allowed and that is no different in "C++ World." When performing floating-point division, the result of dividing by 0 is a specially defined value (almost 100% of the time *not* the value that you want) and when performing integer division, the result is an *exception* that will halt your program. In other words, neither is a good situation.

Could we solve this problem by converting our code into a `while` loop? Maybe. It seems like solving the problem of potentially dividing by zero is something that we will have to deal with no matter whether we use `do ... while` loop or a `while` loop. So, is there something that we get from rewriting the solution above in terms of a `while` loop?

Yes, there is!


One of the nasty things about the `do ... while` loop that we wrote above was that we had to check for the loop's termination condition (`grade < 0`) in multiple places. It would be nice if we could do that a single time and save ourselves some typing. The `while` loop is there to help us out! In order to make sure that we only have to check our termination condition in the loop a single time, we do what is called a _priming_ operation. The priming operation prepares the necessary variable(s) for the evaluation of the `while` loops condition for the first iteration. When converting between a `do ... while` and a `while` loop, the priming operation must set the state of the program in such a way that the loop will execute its body at least once! Why? Because that is *exactly* what the `do ... while` guarantees -- that the body of the loop will be executed exactly once!

<html><head></head><body><pre>
 1 #include &lt;iomanip&gt;
 2 #include &lt;iostream&gt;
 3 
 4 <font color=green>int</font> main() {
 5   <font color=green>double</font> current_score{<font color=red>0.0</font>};
 6   <font color=green>double</font> total_pts{<font color=red>0.0</font>};
 7   <font color=green>int</font> lab_num{<font color=red>0</font>};
 8 
 9   std::cout << <font color=red>"Enter lab score (or -1 if there are no more scores): "</font>;
10   std::cin >> current_score;
11 
12   <font color=green>while</font> (current_score >= <font color=red>0.0</font>) {
13     total_pts += current_score;
14     lab_num++;
15     std::cout << <font color=red>"Enter lab score (or -1 if there are no more scores): "</font>;
16     std::cin >> current_score;
17   }
18 
19   <font color=green>auto</font> grade = (total_pts) / lab_num;
20   std::cout << <font color=red>"Grade: "</font> << std::fixed << std::setprecision(2) << grade
21             << <font color=red>"%\n"</font>;
22 
23   <font color=green>return</font> <font color=red>0</font>;
24 }

</pre></body></html>

Look very closely at the version of the application written using the `while` loop and compare it with the version written using the `do ... while` loop. Although we meant to make the `while` loop an exact copy of the `do ... while` loop, we did so with a combination of different tools and different code behavior. In particular, in the `do ... while` loop the body always executed at least once. The body of the loop contains the code that prompts the user for input and reads their response. In the version of the program written using `while` loop reading the input from the user happens *both* inside *and* outside the loop. As a result, the `while` loop does actually perform the same operations the same number of times as the `do ... while` loop. We just needed to move them around slightly. 

As you can start to see, the constructs for writing code that performs repeated operations are, in a sense, interchangeable. We will see that more completely as we begin to learn about the power  and utility of the `for` loop.

Because the while loop checks the condition before (pre) it executes its body, it is called a _pre-test loop_. If the condition of the while loop is not true the first time that it is encountered, the body of the loop will never execute. Because it is not guaranteed to execute at least once, we cannot ask for user input only in the body of the loop. If we did, we would never get the user's first input! So, when a while loop is used to, for example, validate user input, we perform a _priming read_ that initializes the variable used in the loop condition. In the example above,

  std::cout << "Please enter the first grade: ";
  std::cin >> grade;

prompts the user for the input and performs the priming read.

The general format of the while loop is

while (_condition_) {
  _statements_
}

Note that there is no semicolon (`;`) after the closing parenthesis demarcating the condition. This is very important!

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