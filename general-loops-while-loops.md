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

These are just two pseudocode examples of the many, many uses of _loops,_ programming structures that cause a group of statements to repeat. The group of statements to be repeated by a loop is called the _body_ of the loop. Programmers often say that the body is "in" a loop. We call every repeated execution of the body of a loop an _iteration_. Loops are an incredibly important tool for programmers -- almost as important as the `if` statement. There are three different loops: the `for` loop, the `while` loop and the `do...while` loop. Each of these loops exists for a reason and solves a particular type of problem. It is important to understand when to use each of the loops.

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

Let's assume that we have a function called `valid_password` that takes a single parameter, the user's password. `valid_password` will return `true` when the password is valid and `false` otherwise. The `do...while` loop that we use to implement the flipped pseudocode algorithm above would look like this:

```C++
#include <iostream>

bool valid_password(std::string password_to_check) {
  ...
}

int main() {
  std::string entered_password{""};
  do {
    std::cout << "Please enter your password: ";
    std::cin >> entered_password;
  } while (!valid_password(entered_password));
  ...
  return 0;
}
```

Look closely at the condition for terminating the `do...while` loop. Convince yourself that it is the correct condition: When the user's password is invalid, `valid_password` returns `false` which means that the program should prompt the user to enter their password again. But the `do...while` loop only repeats the execution of its body when the condition is true! So, we use the `!` operator to flip the result of the validation function. Voila! Our loop does our bidding!

We will refer to the `do ... while` loop as a _conditional_ loop. I wonder why? Think about the meaning (or function) of a `do ... while` loop. The structure gives the programmer to power to repeatedly execute a set of statements as long as a condition (encoded in the `expression`) is `true`. See it? _condition_? Exactly!

### While Loops: The Variation on the Theme

The `while` loop is another type of conditional loop. The `while` and the `do ... while` loop are very similar but there is one very important difference: the `do ... while` loop is guaranteed to execute its body _at least one time_ while (sorry, I couldn't resist) the `while` is not.

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
4. The bodies of all declared functions are provided (correctly) elsewhere.

```C++
#include <iostream>

int get_grade();
std::string format_grade(double grade);

int main() {
  double current_score{0.0};
  double total_pts{0.0};
  int lab_num{0};

  do {
    std::cout << "Enter lab score (or -1 if there are no more scores): ";
    current_score = get_grade();

    if (current_score >= 0) {
      total_pts += current_score;
      lab_num++;
    }
  } while (current_score >= 0);

  auto grade = (total_pts) / lab_num;
  std::cout << "Grade: " << format_grade(grade)
            << "%\n";

  return 0;
}
```

In this version of the program we are using a `do ... while` loop. That is safe because we are allowed to assume (from above) that the user enters at least one valid grade for the student. 

Look closely at the code above and think about the problem that may arise if this assumption did not hold. That's right! You could end up with a situation where you have `lab_num` set to `0` and use it as the dividend in a mathematical operation. In "Math World", division by 0 is not allowed and that is no different in "C++ World." When performing floating-point division, the result of dividing by 0 is a specially defined value (almost 100% of the time *not* the value that you want) and when performing integer division, the result is an *exception* that will halt your program. In other words, neither is a good situation.

Could we solve this problem by converting our code into a `while` loop? Maybe. It seems like solving the problem of potentially dividing by zero is something that we will have to deal with no matter whether we use `do ... while` loop or a `while` loop. So, is there something that we get from rewriting the solution above in terms of a `while` loop?

Yes, there is!

One of the nasty things about the `do ... while` loop that we wrote above was that we had to check for the loop's termination condition (`grade < 0`) in multiple places. It would be nice if we could do that a single time and save ourselves some typing. The `while` loop is there to help us out! In order to make sure that we only have to check our termination condition in the loop a single time, we do what is called a _priming_ operation. The priming operation prepares the necessary variable(s) for the evaluation of the `while` loop's condition for the first iteration. If we were converting between a `do ... while` and a `while` loop, the priming operation must set the state of the program in such a way that the loop will execute its body at least once! Why? Because that is *exactly* what the `do ... while` guarantees -- that the body of the loop will be executed exactly once!

```C++
#include <iomanip>
#include <iostream>

int main() {
  double current_score{0.0};
  double total_pts{0.0};
  int lab_num{0};

  std::cout << "Enter lab score (or -1 if there are no more scores): ";
  current_score = get_grade();

  while (current_score >= 0.0) {
    total_pts += current_score;
    lab_num++;
    std::cout << "Enter lab score (or -1 if there are no more scores): ";
    current_score = get_grade();
  }

  auto grade = (total_pts) / lab_num;
  std::cout << "Grade: " << format_grade(grade)
            << "%\n";


  return 0;
}
```
Look very closely at the version of the application written using the `while` loop and compare it with the version written using the `do ... while` loop. Although we meant to make the `while` loop an exact copy of the `do ... while` loop, we did so with a combination of different tools and different code behavior. In particular, in the `do ... while` loop the body always executed at least once. The body of the loop contains the code that prompts the user for input and reads their response. In the version of the program written using `while` loop, reading the input from the user happens *both* inside *and* outside the loop. As a result, the `while` loop does actually perform the same operations the same number of times as the `do ... while` loop. We just needed to move them around slightly. 

### One Of These Things Is Just Like The Other

As you can start to see, the constructs for writing code that performs repeated operations are interchangeable. Having worked through proving to yourself that a `do ... while` and a `while` loop can both effectively be used to write the grade calculation example above, you should start to see a general _algorithm_ that could be used to convert from one loop to the other. 

We will see the flexibility and interchangeability of the different types of loops in more depth as we begin to learn about the power and utility of the `for` loop.
