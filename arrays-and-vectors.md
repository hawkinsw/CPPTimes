### As An (Array) Actor, What's My Motivation?

Think about the poor professor who needs to write an application to adjust their student's quiz scores. The professor's application will need to read in their students' IDs from a file, prompt for and read their quiz grades (interactively), prompt for and read the curve value, adjust the scores accordingly, and print the updated scores for each student.

For a class of three students, that's relatively easy: The professor just needs to a) declare three variables to store three IDs, and three grades; b) declare a variable to store the curve; c) do the necessary input/output (I/O) for the three students; d) adjust the scores for each of the three students and e) print the three students' results. Here's how the teacher could write it:

```C++
#include <iostream>
#include <fstream>

int main() {

  // Try to open (for reading) the file containing
  // all the mnumbers that we are going to manipulate.
  std::string m_number_file_name{"mnumbers3.txt"};
  std::ifstream m_number_file{m_number_file_name};

  if  (!m_number_file.is_open()) {
    std::cout << "Could not open the m number file (" << m_number_file_name << ".n";
    return 1;
  }

  // Read the three mnumbers from the file.
  std::string mnumber1, mnumber2, mnumber3;
  m_number_file >> mnumber1;
  m_number_file >> mnumber2;
  m_number_file >> mnumber3;

  // We are done with the input file at this point!
  m_number_file.close();

  // For the teacher's sanity, let's print out a class
  // roster to make sure that we got all our students.
  std::cout << "Class Roster:n";
  std::cout << "1. " << mnumber1 << "n";
  std::cout << "2. " << mnumber2 << "n";
  std::cout << "3. " << mnumber3 << "n";

  // Read the scores for each student from the user.
  double grade1, grade2, grade3;
  std::cout << "Enter quiz grade for " << mnumber1 << ": ";
  std::cin >>  grade1;

  std::cout << "Enter quiz grade for " << mnumber2 << ": ";
  std::cin >>  grade2;

  std::cout << "Enter quiz grade for " << mnumber3 << ": ";
  std::cin >>  grade3;

  // Read the value of the curve from the user.
  double curve;
  std::cout << "Enter curve value: ";
  std::cin >> curve;

  // Adjust the student's scores.
  grade1 += curve;
  grade2 += curve;
  grade3 += curve;

  // Print the updated scores to the screen.
  std::cout << "Adjusted quiz grade for " << mnumber1 << ": " << grade1 << "n";
  std::cout << "Adjusted quiz grade for " << mnumber2 << ": " << grade2 << "n";
  std::cout << "Adjusted quiz grade for " << mnumber3 << ": " << grade3 << "n";
}
```

The problem is that our busy professor cannot easily maintain such an application when other students joins their class! To update their application when a new student adds the class, the professor would have to copy/paste code in order to accommodate that fourth student (declare another variable to store an ID (`mnumber4`), declare another variable to store a grade (`grade4`), do an additional step of I/O, perform another score adjustment (`grade4 += curve;`), and execute another `std::cout`). The problem only gets worse if the teacher is popular and lots of students want to join the class!

## Arrays (an Interlude)

We interrupt our narrative for a brief digression into the fundamentals of arrays. We have learned already that all variables have types. For instance, variables can have `int` type, `char` type, `double` type, `std::string` type, `bool`. Well, surprise, an array is also a type! That means we can declare/define variables that have array type. Variables with types that we have learned so far (i.e., `int`, `char`, `double`, `std::string`, `bool`) each hold one value -- they are referred to as _fundamental_ types in C++. A variable with an array type holds a number of values! Because variables with array type are "composed" of several other variables, they are sometimes referred to as _compound_ types.

Formally, an _array_ is a data structure that holds

1.  a _fixed_ number of
2.  individually and collectively accessible values (in other words, we can address any element in the array individually and we can address the entire array as a group),
3.  each of which is the same type.

A _data structure_ is just a fancy term for a way of "stor\[ing\] and organiz\[ing\] data in order to facilitate access and modifications." Each value in an array is called an element and each element acts just like a variable -- the only difference is that we refer to these "variables" (aka elements) by number (their _index_ in the array) rather than by name (like we do for variables).

It is important to remember that arrays are variables! In some ways they are different than other variables and in some ways they are the same. They are the same because they have a type and a range of valid values. They are different because they hold multiple values rather than just a single value.

Just how many values can an variable of array type hold? The arrays that we use in this class will have a fixed (i.e., unchanging/constant) number of elements known to the programmer when they write their source code. In other words, the number of elements in the array must be specified before the program runs. The programmer makes known the number of elements in the array (called the _size_ of the array) to the compiler using a special syntax and the compiler relies on the constancy of that number when it processes the source code.

If an array is just like any other variable, it stands to reason that we will have to declare/define it like we have to declare/define other variables before we use them! Here is the bare-bones format of a declaration/definition of an array:

_<element type>_ _<array name>_`[`_<number of elements>_`];`

You can read that declaration/definition like:

"_<array name>_ is an array of _<number of elements>_ _<element type>_s." It is important to remember that the size of the array is _part of the type of the array variable_.

Let's look at a few examples:

double batting\_averages\[40\];

We can read that like:

"`batting_averages` is an array of 40 `double`s."

int days\_per\_month\[12\];

We can read that like:

"`days_per_month` is an array of 12 `int`s."

In our declarations/definitions, how come there is only a single type for an array? I mean, there are multiple elements, right? And Will, you _just said_ that elements in an array act like variables and every variable has a type! Shouldn't we have to specify the type of every element in the array? Remember what we said above:

> Formally, an _array_ is a data structure that holds a fixed number of values, **_each of which is the same type_**.

Because each element in an array has the same type, we only need to specify the type once when we declare an array!

Let's visualize arrays in the computer's memory to get a better sense of what is going on. First, remember that every variable has a space in memory:

![One Variable In Memory.png](https://uc.instructure.com/courses/1559160/files/157853045/preview)

Assume that `days_in_june` is declared/defined as having `int` type. The space in red in the figure above is big enough to hold a single value of type `int`. The variable `days_in_june` stores the value 30. If we updated `days_in_june` using an assignment statement (e.g., `days_in_june = 31;`) the computer's memory would look like this:

![One Variable In Memory(1).png](https://uc.instructure.com/courses/1559160/files/157852939/preview)

Let's say that I want to store the number of days in each of the summer months! That sounds like a job for an array! Why? Because

1.  There are a fixed number of summer months (3)
2.  I want to refer to those months as a collection (`days_in_summer_months`, perhaps) and individually (so that I could update the number of days in, say, June)
3.  Each element is the same type (an `int`)

We declare such an array like this:

int days\_in\_summer\_months\[3\];

The compiler will allocate space in memory for the array variable (`days_in_summer_months`) just like it allocated space for the `days_in_june`. The only difference is that there will be space for 3 `int`s this time!

![Uninitialized Array In Memory.png](https://uc.instructure.com/courses/1559160/files/157852943/preview)

The _??_s in each of the blocks indicates that the values of each of the elements is uninitialized after a declaration/definition like the one shown above. The first of the summer months is June, the second is July and the third is August. June has 30 days. July has 31 days. August has 31 days. Let's use code to update our array of three (3) `int`s to match reality:

  days\_in\_summer\_months\[0\] = 30;
  days\_in\_summer\_months\[1\] = 31;
  days\_in\_summer\_months\[2\] = 31;

After these assignment statements, our memory looks like this:

![Array In Memory(1).png](https://uc.instructure.com/courses/1559160/files/157853039/preview)

If you'd like, you can think of `days_in_summer_months[0]` as a variable declared like

int june;

and `days_in_summer_months[1]` as a variable declared like

int july;

and `days_in_summer_months[2]` as a variable declared like

int august;

As we've said over and over, the elements in an array can be treated just like variables. Let's change the values of the elements as if they were variables:

  days\_in\_summer\_months\[2\] += 40;

If your favorite month is August, it can now be a little longer!

  days\_in\_summer\_months\[2\] /= 2;

August is too hot and we want it to last only half as long!

  int total\_days\_in\_summer = days\_in\_summer\_months\[0\] +
                             days\_in\_summer\_months\[1\] +
                             days\_in\_summer\_months\[2\];

We can also read from them just like normal variables.

We always want to initialize normal variables before we use them. Using a variable before initialization results in the introduction of undefined behavior into the program (see below and/or earlier C++ Times for a refresher on this topic). So, how do we initialize values of the elements of an array? There are a few different methods. First,

  int days\_in\_summer\_months\[3\]{30, 31, 32};

will initialize element `0` of the `days_in_summer_months` to `30` (i.e.,

(days\_in\_summer\_months\[0\] == 30) == true

), element `1` of the `days_in_summer_months` to `31` and element `2` of `days_in_summer_months` to `31`.

If we want to declare/define an array with a set of values and know those values at the time of the declaration/definition, we can leave out the size of the array:

  int days\_in\_summer\_months\[\]{30, 31};

will declare/define an array of _2_ `int`_s_ with element `0` of the `days_in_summer_months` to `30` and element `1` of the `days_in_summer_months` to `31`.

If we want to declare/define an array with the value of each element initialized to 0, we can use this syntax:

  int days\_in\_summer\_months\[3\]{0};

There are [several variations on this theme](https://en.cppreference.com/w/cpp/language/aggregate_initialization) but these are the most common ways of initializing elements of an array.

#### Danger

C++ offers you _no protection_ against accessing an array beyond its defined size. Let's say that an intrepid programmer thought they could set the number of days in September using our array by writing code like this:

  days\_in\_summer\_months\[3\] = 30;

There are only three elements in the array but the programmer is attempting to access the fourth element. This type of mistake is so common that it has its own name: an _out-of-bounds array access_. Because C++ itself does not offer protection against such illegal accesses, you will often have to write code that performs explicit _bounds checking_ (making sure that the value you use to index an array is valid). 

Accessing an array out of bounds introduces _undefined behavior_ into your program. We have heard about undefined earlier when we talked about using variables before they have been initialized. Once your program contains undefined behavior "there are no restrictions on the behavior of the program".

### Snap Back to Reality: Our Tortured Teacher

Let's get back to helping our client. As it stands, their application only works for a class with three students. The professor knows the number of their students at the beginning of each semester (i.e., before they compile their code), wants to refer to the students' grades individually and collectively and each of the grades is the same type. It fits the use case of the array perfectly!

Let's slowly update our existing code to use arrays!

We don't need to make any changes to the code that opens the input file and checks for its existence. The first pieces of code that we will need to update are the bits that read in the M Numbers from that file. Here is our original version:

  // Read the three mnumbers from the file.
  std::string mnumber1, mnumber2, mnumber3;
  m\_number\_file >> mnumber1;
  m\_number\_file >> mnumber2;
  m\_number\_file >> mnumber3;

Our updated application will handle 5 students. Because we know how terrible it is to use magic numbers in our code, we'll use a constant to represent that value:

  const int STUDENTS\_IN\_CLASS{5};

Next, we will declare an array of the proper type and size to hold those M Numbers that we read from the file:

  std::string mnumbers\[STUDENTS\_IN\_CLASS\]{""};

We _could_ read those M Numbers from the file and store them into the `mnumbers` array one at a time:

  m\_number\_file >> mnumbers\[0\];
  m\_number\_file >> mnumbers\[1\];
  m\_number\_file >> mnumbers\[2\];
  m\_number\_file >> mnumbers\[3\];
  m\_number\_file >> mnumbers\[4\];

but that's lots of copying and pasting! Squint at that code and you will see a `for` loop asking to be written! Why? We know the number of times (counter!) that we want to execute an action (a body!) -- that's _exactly_ what a `for` loop is, well, for!

  for (int i = 0; i<STUDENTS\_IN\_CLASS; i++) {
    m\_number\_file >> mnumbers\[i\];
  }

Now, we will want to replace the old code that generated our roster

  std::cout << "Class Roster:\\n";
  std::cout << "1. " << mnumber1 << "\\n";
  std::cout << "2. " << mnumber2 << "\\n";
  std::cout << "3. " << mnumber3 << "\\n";

with new, more flexible code:

  std::cout << "Class Roster:\\n";
  for (int i = 0; i<STUDENTS\_IN\_CLASS; i++) {
    std::cout << (i+1) << ". " << mnumbers\[i\] << "\\n";
  }

Let's take a closer look at this code. Our counter/loop variable (named `i`) ranges between `0` and `4`, inclusive. That makes it perfect for _indexing the array_, specifying an element by its index, during iteration. However, most users will expect that the list be shown with prefixes starting at 1 and ending at 5 -- most people aren't computer scientists and prefer to start counting at 1 and not 0. So, we have to use `(i + 1)` in the first part of the `std::cout` statement and `i` in the second.

Next, we will update the code for reading the quiz grades from the terminal. Whereas the first version of the code read the grades using named variables

  double grade1, grade2, grade3;
  std::cout << "Enter quiz grade for " << mnumber1 << ": ";
  std::cin >>  grade1;

  std::cout << "Enter quiz grade for " << mnumber2 << ": ";
  std::cin >>  grade2;

  std::cout << "Enter quiz grade for " << mnumber3 << ": ";
  std::cin >>  grade3;

our updated application will read them using (another) `for` loop:

  double grades\[STUDENTS\_IN\_CLASS\]{0.0};
  for (int i = 0; i<STUDENTS\_IN\_CLASS; i++) {
    std::cout << "Enter the quiz grade for " << mnumbers\[i\] << ": ";
    std::cin >> grades\[i\];
  }

Here we are declaring an array variable named `grades` that holds `STUDENTS_IN_CLASS` `double`s. Then we are asking the user to enter each student's grade and saving those response in the array.

With the loop above, we are employing a very common strategy: _parallel_ arrays. The elements at index `i` in `grades` and `mnumbers` are _logically_ related. `grades[i]` is the quiz grade for the student with the `mnumbers[i]` M Number.

![Parallel Arrays(1).png](https://uc.instructure.com/courses/1559160/files/157853101/preview)

This is a pattern that you will see throughout your career as computer scientists!

In the old and the new applications, gathering the curve value is the same -- no need to change any of the existing code!

Once we have the value of the curve from the professor, we need to adjust the value of the students' scores. In V1 of the program we adjusted those values one at a time:

  grade1 += curve;
  grade2 += curve;
  grade3 += curve;

In the new version of the code, we will make the same adjustments but use a loop in order to future-proof our code:

  for (int i = 0; i<STUDENTS\_IN\_CLASS; i++) {
    grades\[i\] += curve;
  }

I bet that you are starting to see a pattern!

With the grades adjusted appropriately, the final step is to print out the scores. In the initial version of our application, we explicitly printed out each of the three student's grades:

  std::cout << "Adjusted quiz grade for " << mnumber1 << ": " << grade1 << "\\n";
  std::cout << "Adjusted quiz grade for " << mnumber2 << ": " << grade2 << "\\n";
  std::cout << "Adjusted quiz grade for " << mnumber3 << ": " << grade3 << "\\n";

By now you shouldn't be surprised to hear that we will use a loop in the updated application to accomplish the same thing with (parallel) arrays:

  for (int i = 0; i < STUDENTS\_IN\_CLASS; i++) {
    std::cout << "Adjusted quiz grade for " << mnumbers\[i\] << ": " << grades\[i\]
              << "\\n";
  }

### Welcome To The Future

With that final change, we have built an updated version of the Curve application that is far more future proof. How is the updated program easier to maintain than the original? By centralizing the number of students in the class in one variable (`STUDENTS_IN_CLASS`) and then using loops and arrays whose number of iterations and number of elements are based on that value, our tired teacher can make a single change (updating the `STUDENTS_IN_CLASS` variable) when the number of students in their class increases or decreases and everything will "just work". Pretty amazing!

### Like Cheese And Bread

Like spaghetti and chili (or peanut butter and jelly), you might have noticed that arrays and loops (especially `for` loops) go well together. When you are working with arrays you will often find yourself using loops. In fact, rare will be the occasion when you work with an array and do not use a loop.

### Range-based For Loops Over Arrays

  for (int i = 0; i < _<size of array\_name>_; i++) {
     ...  
     ... _<array\_name>_\[i\] ...  
     ...    
  }

is such a common _pattern_ ([a general, reusable solution to a commonly occurring problem within a given context in software design](https://en.wikipedia.org/wiki/Software_design_pattern)) that the designers of C++ added special syntax to make writing it easier:

  for (auto v : array\_name) {
    ...
    ... element ...
    ...
  }

This type of loop is known as a _range-based_ `for` loop. One iteration of the loop will occur for each of the elements in `array_name` and the variable `v` will be a _copy_ of the that element from `array_name`. For example,

  for (auto days\_in\_month : days\_in\_summer\_months) {
    std::cout << days\_in\_month << "\\n";
  }

will print

30
31
31

Be very careful: with the syntax above, `v` is a _copy_ of the current element from `array_name` and changing the value of `v` in the body of the loop will have no impact on the values in `array_name`.

  for (auto days\_in\_month : days\_in\_summer\_months) {
    days\_in\_month += 1;
  }
  for (auto days\_in\_month : days\_in\_summer\_months) {
    std::cout << days\_in\_month << "\\n";
  }

will print

30
31
31

If you want to be able to update the values in `array_name` using a range-based for loop, you will have to declare `v` as a reference variable (for more information see Section 6.13 in the text book):

  for (auto &element : array\_name) {
    ...
    element = <expression>;
    ...
  }

Using this syntax,

  for (auto &days\_in\_month : days\_in\_summer\_months) {
    days\_in\_month += 1;
  }
  for (auto days\_in\_month : days\_in\_summer\_months) {
    std::cout << days\_in\_month << "\\n";
  }

will print

31
32
32

Notice how the `&` works in a range-based `for` loop much the same way that it works in defining a reference parameter for a function!

It is important to note that range-based `for` loops can only be used to iterate through arrays when the compiler knows the size of the array. Also, notice that with a range-based for loop you do not get an index variable "for free". In other words, rewriting a loop like

  std::cout << "Class Roster:\\n";
  for (int i = 0; i < STUDENTS\_IN\_CLASS; i++) {
    std::cout << (i + 1) << ". " << mnumbers\[i\] << "\\n";
  }

with a range-based `for` loop is a little cumbersome.

However, when it _is_ possible for you to use a range-based `for` loop, use it! Range-based `for` loops offer you protection from the dreaded _off-by-one error_ where you (accidentally) miscalculate the bounds of the loop counter variable in a `for` loop and access an array beyond its bounds.

  const int STUDENTS\_IN\_CLASS{5};
  std::string mnumbers\[STUDENTS\_IN\_CLASS\]{""};

  for (int i = 0; i <= STUDENTS\_IN\_CLASS; i++) {
    m\_number\_file >> mnumbers\[i\];
  }

contains an off-by-one error -- the mixup between the `<=` and the `<` means that the code will access one more element than the array contains and cause the program to contain undefined behavior.