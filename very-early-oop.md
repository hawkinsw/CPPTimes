## What's News
There was a surprise upset in today's finals of the Westminster Kennel Club Dog Show. In the Working Breed category, the winner, Bjark Stroustrup, proved that C++ is really a human's best friend.

### Classes and Objects

Remember the term abstraction? We talked about it, but we never really gave a good definition.

We informally thought about it as a way of hiding something but we can do better than that. 

One of the pioneers in the use of abstraction in computer science said that abstraction "is a mechanism which permits the expression of relevant details and the suppression of irrelevant details."[^liskov]

[^liskov]: Barbara Liskov and Stephen Zilles. 1974. Programming with abstract data types. SIGPLAN Not. 9, 4 (April 1974), 50–59. https://doi-org.uc.idm.oclc.org/10.1145/942572.807045

In a thought-provoking essay, Jeff Kramer defines the destructive aspects of abstraction as "[t]he act or process of leaving out of consideration
one or more properties of a complex object so as to attend to others."[^kramer] As a constructive act, Kramer believes that abstraction is "[t]he process of formulating general concepts by [studying] common properties of instances."[^kramer] 

[^kramer]: Kramer, Jeff. Is Abstraction the Key to Computing? _Communications of the ACM._ April 2007. Vol. 50, No. 4

Whether we know it or not, we are all experts at using abstraction: functions hide the process of doing something from the person who wants that action completed. Every time that we invoke a function, we are using abstraction. Functions allow their users to look past the details of _how_ an action is completed and "attend" only to the "relevant" details, like how to use it!

But, is _procedural abstraction_ the only abstraction that exists? No. In fact, it's not even the only abstraction in C++.

C++ is what computer scientists call an object-oriented programming language (OOP). That means that the language is designed for efficiently defining/processing/manipulating objects. 

Well, what is an _object_? Just like a function is an abstraction, an object is an abstraction. However, instead of just hiding a process (like a function), it hides data, too. More than that, an object groups together (_encapsulates_) that hidden data with a set of actions that can change that data. There are all sorts of advantages of writing programs this way and we will discuss them in additional detail later in the course.

A class is related to an object. A class is the way to declare/define the data and processes that are encapsulated in an object. Some examples will definitely help!

Dogs and cats are classes. Fido and Luna are objects of the class type Dog and Cat. When programmers create objects from classes the process is known as _instantiation_. The Dog and Cat classes declare data (like the number of legs the pet has and the color of its fur) and actions that it can perform (e.g., making a noise). Because Fido and Luna are objects of the Dog and Cat type, respectively, they contain all the data and can perform all the actions defined by Dog and Cat. 

Generally, we say that objects are related to classes by the "is a" relationship: Fido _is a_ dog; Luna _is a_ cat. What are some others? The Tesla I rented to get around while my car was being repaired from pothole damage _is a_ Car. An iPad _is a_ Tablet.

The actions defined by the class are called _methods_ and are almost exactly like functions. The only difference is that when they are called they are associated with an object. And, because of that association, a method can access data from that object during its execution.

What preceded was a very, very rapid introduction to OOP and we will come back to it in more detail much later. 

Despite my promise of more OOP fun in the future, having this basic vocabulary about OOP may clear up some of the confusion around strings in C++: `std::string` is a class! Every time that you write C++ that looks like

```C++
std::string name{"Sharona"};
```

you are instantiating that class. `name` is an instance of the `std::string` class. We could make another instance:

```C++
std::string greeting{"Hello!"};
```

Although `greeting` and `name` are both instances of the `std::string` class, they each have their own contents. Changing the contents of `greeting` by doing

```C++
greeting = "Goodbye!";
```

does not change the contents of `name`.

It is very commonly necessary to identify the length of a string. If someone were clever enough to implement a function that calculated the length of a string, its declaration might look like:

```C++
int string_length(std::string str);
```

To use that to determine the number of characters in our greeting, we would invoke it like

```C++
auto greeting_length{string_length(greeting)};
```

But, we don't have to do that! The `std::string` class includes a method called `length` that will do what we want! How do we use it?

```C++
auto greeting_length{greeting.length()};
```

Now do you see what I meant when I said earlier that methods are like functions except "that when they are called they are associated with an object. And, because of that association, a method can access data from that object during its execution"? The code used to implement the `length` function needs to be able to see the data hidden (abstracted!) from the user's view (the group of characters that exist together to form the string) in order to determine the length of its contents. A method is the perfect tool!

I bet that you've seen this somewhere else, too: The `std::ifstream`. You instantiate a `std::ifstream` when you want to use it to read data from a file on your computer:

```C++
std::ifstream my_resume_fs{"resume.docx"};
```

You can invoke all the methods defined for the `std::ifstream` class on the `my_resume_fs` object. No matter that those methods are defined once for all instances, when you invoke a method on a particular object its code will perform calculations based on the data abstracted by _that_ particular instance. 

You've experimented with the `open` method of `std::ifstream` which will tell you whether an instance of a `std::ifstream` is "connected" to a file on the hardrive.

If we assume that there is a file on your computer named "resume.docx", then 

```C++
my_resume_fs.open()
```

will evaluate to `true`. On the other hand, if you saved your resume as a PDF (i.e., you named it "resume.pdf"), then 

```C++
my_resume_fs.open()
```

will evaluate to `false`.

