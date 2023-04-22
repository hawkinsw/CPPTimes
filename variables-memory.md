## What's News

Knole Smusk's latest startup just went public. The entrepreneur is attempting to revolutionize computer control with a brain implant. The tool has shortcomings and investors are worried -- they realize his device is limited by our forgetfulness.

## Stack, Heap, Variables and You

### Variables Im-memory-ial

As a computer scientist and C++ programmer, there are a few things that we always must keep in the back of our minds. One of those *things* that we always must remember is that every variable has a _type_. The type of a variable specifies

1.  the range of valid values that variable can contain and
2.  the operations that can be performed on those values.

If a type specifies the range of valid values, it stands to reason that a variable always contains one of those valid values. We also talked about the _scope_ of a variable: the area of the program where the variable can be accessed. Finally, we discussed that every variable takes up space in memory. That is, every variable has some bytes associated with it in the computer's memory where its value is stored.

> Remember we said that the _byte_ is the smallest addressable unit of memory. A byte is a sequence of 8 bits, a [portmanteau](https://en.wikipedia.org/wiki/Portmanteau) of _binary digits_. This fact will be useful later!.

If we combine those important facts about variables with one of the important facts we learned about computer architecture -- that every byte in memory is addressable by some number (its _address_) --  we can define the _address of a variable_ as the address of the first byte of the variable's storage associated.

To recap, every variable has:

1.  A type,
2.  A scope,
3.  A place in memory (and, therefore, an address), and
4.  A value.

### Your Memory

Like humans, computers have several places where they can store data. For the forgetful, there are notebooks and apps that let us write down things that we want to remember. It can be sometimes laborious and time consuming to transfer what's in our brains to those places, but it pays off in the long run -- you can always rummage through those notes and retrieve that information. On the other hand, we keep more fleeting memories in our brain. They are easy to recall but sometimes they drift away -- like after we sleep or as time passes. Those memories are quick to recall but not as permanent.

Your computer works the same way. Your computer has a disk with (almost) an infinite amount of storage -- like the notebooks and apps you have to contain your TODO list or class notes. The disk is sometimes hard to access (it's slow (relatively) but it remembers everything! Your computer also has memory (RAM, volatile memory) that is like your brain -- it has limited storage and forgets things when it loses power but is tremendously fast.

In an ideal world, while your program runs, all of the things that it needs to remember will remain in memory (in RAM, that is).

The operating system of your computer (e.g., Windows, macOS, Linux) divides the RAM into logical pieces -- the heap and the stack. Though there is only one physical manifestation of RAM, the operating system maintains this logical separation for its convenience (you will learn more about *why* in future classes!).

### Limited Thoughts

Like your brain, your computer's RAM can get overwhelmed and forgetful. There is only so much space in my little brain, I know that for sure! When our computer's RAM runs out of space it is forced to forget things. Needless to say, this situation is perilous. We would prefer that our programs be able to run continuously and store only the data it needs to for its correct operation. We don't want our program to use memory that it doesn't need.

### Automatic ~~Transmission~~ Variables
Have ever you worried about the space that your program is using for its variables? Not yet you haven't! And why not? Because all the variables that you have been using so far are *automatic* variables. The space for automatic variables are managed, well, automatically by the compiler.

We can even see this in action!

```C++

int do_some_work(int a, int b) {
    int c{a + b};
    return c;
}

int main() {
    while (true) {
        int alpha{1}, beta{2}, result{};
        result = do_some_work(alpha, beta);
    }
    return 0;
}
```

If you look (not so closely) at that program, you will see that it contains an infinite loop. The program will, until the end of time, invoke `do_some_work`. To do its *some work*, `do_some_work`, needs space for variables -- in particular, it needs space to hold the values of its parameters (e.g., `a` and `b`) and the its local variables (e.g., `c`). 

If the computer continously created and used new space in memory *every time* that `do_some_work` was called, we would quickly run out of the limited space that we have in the computer's RAM! As an expert at writing wrong code, I know that the program above will *not* stop any time soon.

So, if we haven't consciously thought about limiting memory, then there must be *something* behind the scenes that is helping us by _automatically_ creating memory for those local variables and then _automatically_ destroying that memory when it is no longer needed. It is here where the compiler enters, stage left.

The compiler will handle the storage *allocation* and *deallocation* for all automatic variables. As a programmer, you do not have to worry about it!

Every time that `do_some_work` is executed, code executes that
1. Creates the space necessary to hold the values of `a`, `b` and `c`.
2. Copies the values of the arguments `alpha` and `beta` into the space created for `a` and `b`.
3. Turns the program loose to execute the code in the function.

Then, when the function is done executing, code runs that
1. Copies the value of `c` into the space associated with `result`.
1. Releases the space used to hold `a`, `b` and `result` back to the system so that it can be reused!

All that work and you don't have to to do a thing -- *truly* automatic! Automatic variables are always stored on the stack.

### Uh Oh

You might see the danger ahead, though! In order for the compiler to be able to generate code that does that management on our behalf, it must be able to learn how much space we need. Remember, importantly, that the compiler only executes *once*. It does not execute every time that the program runs. Therefore, if it has any chance of generating code that allocates the proper amount of space, it has to know the necessary sizes at the time it executes!

What happens if the amount of space needed to hold a variable is determined by the whims of the user? It'd be impossible for the compiler to know just how I would work with a program -- even *I* don't know that half of the time. To solve this problem, programmers are forced to resort to *dynamic* variables. The space allocated to store the values of dynamic variables must be managed manually (the reason they are sometimes called *manual* variables) by us, the programmer. 

While we gain more flexibility from using dynamic variables, there is a huge risk -- we fallible programmers forget to release the memory associated with variables when it is no longer needed. When that happens, the longer that our program runs, the more space it uses until, eventually and tragically, the program has used all available memory and it crashes!

Dynamic variables are always stored on the heap.