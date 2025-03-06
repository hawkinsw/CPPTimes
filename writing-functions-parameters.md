## What's News

The World's Most-Wanted C++ programmer, Bjarne Stroustrup, who has gone by various criminal aliases (including Bjarne Scope and Binary Stroustrup), has finally been captured. Police from the special For Loop of the federal Marshalls picked up Mr. Stroustrup today in NYC.

## Why Do I Have To State The Obvious?

The parameters of a function are _the_ way to give its user the power to customize its behavior. Parameters are like the sliders that we turn to customize the ratio of bass and treble in our AirPods. Powerful functions often provide the caller many, many options for customization.

That said, it may not always be strictly necessary that the caller specify arguments for each of those parameters. Some powerful functions are designed to work well with some _default_ values. Having default values for parameters is a perfect way to balance simplicity and power. If a user does not know that they need special handling, then the defaults are great and make the function easy to use. On the other hand, users are not restricted from using a function in a way that fits their _exact_ needs.

For example, consider a function that inflates the tires on our cars. It could be defined like

```C++
bool inflateTires(double toPSI) {
    ...
    return true;
}
```

The function will inflate each of the tires of the car to a certain PSI. That's great, but I am not much of a car person. When I take my Camry to the BP and add air to the tires, I simply want to fill up the tires to the manufacturer-recommended level. In other words, I want to invoke the `inflateTires` function but I just want to use the default value for `toPSI`.

On the other hand, there are people who take their SUVs to drive in the sand on the beach. They have to inflate their tires to a very specific PSI so that they get the maximum grip. They are also interested in being able to escape water at high tide and stay mobile in very loose sand. For them, they will want to call `inflateTires` with a non-manufacturer-recommended pressure level.

C++ gives us the tools to write functions for are great for the neophytes (like me) and off roaders. In C++, the person writing a function can specify default values for its parameters. When another programmer invokes a function that has default parameter, they can accept the default value (simply by not passing an argument corresponding to the parameter) or give a customized value (simply by specifying an argument for the parameter). For instance,

```C++
bool inflateTires(double toPSI = 31.0) {
    ...
    return true;
}

int main() {
    ...
    if (!inflateTires(28.0)) {
        std::cout << "There was an error preparing your tires for the sand. Stay on firm ground.\n";
    }
    ...
    if (!inflateTires()) {
        std::cout << "There was an error returning your tires to a normal PSI\n";
    }
}
```

In the first invocation of the `inflateTires` function, the programmer using it is saying that they do not want to accept the default inflation level of 31.0 PSI. Instead, by supplying their own argument for the parameter, they are expressing their desire to have the tires inflated to a custom level (in this case, 28.0 PSI). In the second invocation of the `inflateTires` function, the programmer is saying that they want to accept the normal, default value for the inflation target. How do we know? Because the `inflateTires` function expects an argument but the caller did not provide one. If there were no default given, the code would not compile.

Again, the ability to offer a function's users parameters for customization _only when necessary_ is really powerful. It allows function authors to meet the needs of both regular and power users. In industry, this technique is sometimes used to write functions with a final parameter to indicate whether or not to provide debugging output:

```C++

void optimizedHardwareCustomization(double v, int a, bool debug = false) {
    ...
    if (debug) {
        std::cout << "The device's impedence before the change is " << getImpedence() << "\n";
    }
    impedence = v;
    if (debug) {
        std::cout << "The device's impedence after the change is " << getImpedence() << "\n";
    }
    ...
}
```

During debugging and testing, the programmer may call the function with the `debug` flag set to `true`. However, when the code ships to the customer, it may make sense to only selectively enable that type of output.

There are several rules to follow when using default arguments:
1. The values for default arguments are only given in the function declaration. In many cases, the function declaration and definition are combined so this rule seems like it won't cause may problems. However, when the declaration and definition of a function with default arguments are separate, the default values must not be repeated at the point of the definition.[^default_declaration]
2. When reading the parameters from left to right, every parameter to the right of the leftmost parameter with a default value must also have default values. In other words, 

[^default_declaration]: In fact, it's slightly more complicated than that. If there is more than one declaration for a function visible to the compiler, only one of those declarations is allowed to specify the default arguments. But, that's not where the oddities end. Check out this code: [https://godbolt.org/z/x68PM9vjs](https://godbolt.org/z/x68PM9vjs).

```C++
void f(int a, int b = 5, int c) {
    ...
}
```
is invalid.

3. Assuming that we reading parameters of a function from left to right, every argument to the right of the leftmost omitted argument must _also_ be omitted (and, therefore, the caller must accept the default). In other words,

```C++
void f(int a, int b = 5, int c = 6) {
    ...
}

int main() {
    f(5, 6);
}
```

will call `f` with `b` equal to `5` and accept the default for `c` -- there is no way to "skip" an argument. The caller cannot, for example, accept the default for `b` and customize `c`. Once the caller of the function accepts the "first" (again, in left to right order) default, they must accept every remaining default. It's all or nothing after accepting the first default argument value.

## I Am More Than a Coin Flip

In C++ there is a fundamental limit that seems to affect the utility of functions -- can you name it?

That's right -- you can only return a _single_ value from a function[^num_return_values]. So, how, then, are we to write a function that prompts the user for, say, their age and

- indicates whether the age they entered was reasonable *and*
- returns that value when it is reasonable?

Tricky.

[^num_return_values]: There are other possible solutions to the C++-only-`return`s-a-single-value problem, but the solution described here is very common and also illustrates a tool that can be used to accomplish other feats.

All parameters have to get their values from somewhere before that function can start executing.

The way that we have learned to accomplish this initialization feat to date is by matching arguments with parameters (in their positional order) and _copying_ values of the argument (whether that is a literal, a variable or any other expression) to the corresponding parameter. This technique for initializing parameters at the time of a function call is known as _pass by value_.

The C++ compiler generates code that will allocate different space in memory for each variable in our program. The C++ compiler also generates code that will make different space for each of a function's parameters _every time_ that it is invoked. In other words, there is a (potentially) different place in memory for each of the local variables of a function each time that it is invoked.[^lies] 

If even the space in memory for the parameters of a function are not (necessarily) the same at every invocation, it makes sense that the space for those parameters is not the same as the space for the arguments in a given call.[^literal_arguments] Because "value parameters" occupy a completely different place in memory than the argument that gave them their initial value, any changes to the parameter in the body of the function are _not_ seen by the caller. 

[^lies]: That statement is not entirely true. However, when it is _not_ true it is either because of randomness or because the programmer has given the compiler specific instructions to do otherwise (see e.g. [`static`](https://en.cppreference.com/w/cpp/keyword/static)).

[^literal_arguments]: By-value arguments do not even need to be variables. They just need to be expressions. So, yeah, ...

```C++
bool get_age(int user_age) {
    std::cout << "Please enter your age: ";
    std::cin >> user_age;
    if (user_age > 0 && user_age < 150) {
        return true;
    }
    return false;
}

int main() {
    int user_entered_age{-1};
    bool valid_age{get_age(user_entered_age)};
    return 0;
}
```

Just before the invocation of the `get_age` function, the state of the local variables are as shown in the image below.

![](./graphics/Reference%20Parameters%20-%200.jpg)

Upon invocation of the `get_age` function, the value of the `user_entered_age` argument is _copied_ to the value of the `user_age` parameter. A new scope is created that will persist as long as the `get_age` function executes and you can think of that scope as demarcating the space used to store all the function's local variables, especially its `user_age` parameter.

![](./graphics/Reference%20Parameters%20-%201.jpg)

![](./graphics/Reference%20Parameters%20-%202.jpg)

Once the user has entered their input and it has been processed by the `std::cin >> user_age;` statement, the values of the variables in memory are as shown below.

![](./graphics/Reference%20Parameters%20-%203.jpg)

Finally, when `get_age` completes and execution passes back to the point in `main` where `get_age` was invoked, the `get_age` scope is vacated and the space allocated to the storage of the local variables _for this particular invocation of the function_ are destroyed.

![](./graphics/Reference%20Parameters%20-%204.jpg)

As a result of the semantics just described, an invocation of `get_age` will complete without having any effect on `user_entered_age` in the scope of the `main` function.

> Note: The invocation _does_ have an impact on the value of `valid_age` because it is the place where the invocation's return value is stored.

Given that the _obvious_ rationale for writing a function like `get_age` is, you know, getting someone's age, this behavior is _far_ from ideal. What are we going to do?

We will turn to _pass-by-reference_ parameters. "By reference" parameters are a way for the person writing the function and the person calling the function to provide an _alias_ within the scope of the called function that _references_ a variable from the calling scope. To see how by-value and by-reference parameters are different, let's redefine `get_age` using reference parameters and see how the program's execution differs as a result.

The situation just before the invocation of the updated definition of the `get_age` function, is the same as the situation just before the older version of the `get_age` function.

![](./graphics/Reference%20Parameters%20-%200.jpg)

The moment that the function `get_age` is invoked, things take a turn for the different! Again, a new scope is created to provide space for the values of the variables local to the called function. This time, however, the scope contains `user_age` as simply an alias for the variable used by the caller for the argument: `user_entered_age`. 

![](./graphics/Reference%20Parameters%20-%201r.jpg)

![](./graphics/Reference%20Parameters%20-%202r.jpg)


Once the user has entered their input and it has been processed by the `std::cin >> user_age;` statement, the values of the variables in memory are as shown below.

![](./graphics/Reference%20Parameters%20-%203r.jpg)

Note _very well_ that the assignment to the `user_age` variable updates the value assigned to the `user_entered_age` in the scope of the `main` function _because it is just an alias (or reference) to that variable_. Any time the fictional minion inside our computer attempts to access the value of the `user_entered_age` variable during execution of the by-reference version of the `get_age` function, they find the value by walking through that by-reference parameter as if it were a portal to the real variable. At the same time, any time Kevin (our fictional computer minion) attempts to update the value of the `user_entered_age` variable during execution of the by-reference version of the `get_age` function, they find the location in memory that will hold the new value by walking through that same portal.

Finally, when `get_age` completes and execution passes back to the point in `main` where `get_age` was invoked, the `get_age` scope is vacated and the space allocated to the storage of the local variables _for this particular invocation of the function_ are destroyed.

![](./graphics/Reference%20Parameters%20-%204r.jpg)

Unlike the invocation of the by-value version of `get_age`, the changes to `user_age` have resulted in a change to `user_entered_age` (while maintaining the update to the `valid_age` flag that _also_ occurred in the by-value version). How cool! It's exactly as we hoped!

The only thing cooler than _how_ by-reference functions work is the way that we indicate that a parameter is passed by reference. Functions can have a mix of by-value and by-reference parameters. A by-reference parameter is indicated with a `&` in front of the name of the parameter -- there is no change to the way that the function is called, as demonstrated by the code above.

![](graphics/popeil.jpg)
But just wait, there's more! `int`s and `double`s and `char`s can hold (relatively) few values and, therefore, require relatively little space in memory. As a result, copying the value of an argument to give the corresponding parameter its initial value for a particular invocation will not take very long when those are the types that are passed by value. 

On the other handle, `std::string`s can take quite a bit of memory. Just think about a `std::string` that contains the entire Oxford English Dictionary and needs to be passed to a function that performs spell check. In order to spellcheck a user's rant on social media, the spell check function is invoked. There will be one copy of the dictionary per word in the user's rant! That seems seriously wasteful. By-reference parameters to the rescue again -- the code that the compiler generates to create a reference runs very fast! For cases like passing `std::string`s (or `std::vector`s or any other compound types) to functions as parameters, it makes sense to pass them by reference to save time (not to mention the space savings from not having two copies of the dictionary's words in memory at the same time.). 

It's a perfect solution? Not so fast. As a result of passing by reference, the caller has lost the ability to assume that the variable it provided as an argument to the function will be unchanged as a result of that function invocation.

> Could they ever have actually made this assumption? Yes! Remember: If the argument were passed by value, even assuming that the parameter is changed in the function, the value of the caller's argument does not change (because those modification operations done in the space of the invoked function act on the per-invocation _copy_ of the other variable).

Can we get the best of both worlds? Amazingly, the answer is _yes_, we can! When we are writing a function and declaring that a parameter is by reference _just for efficiency sake_ and not because we need to modify it, we will declare the parameter to be a by-`const`-reference parameter. The `const`ness of the by-reference parameter will ensure the caller that the value of its precious argument will not be tampered during the function's execution. 

How can it be so sure? Because if the author of the function _did_ attempt to edit it, the source code would not compile!

The function declaration for the hypothetical spell check function would look something like ...

```C++
bool is_spelled_correctly(const std::string &word_to_check, const std::string &dictionary) {
    ...
}
```

If the function included code that attempted to "learn" a misspelled word by adding it to the dictionary (e.g.,

```C++
bool is_spelled_correctly(const std::string &word_to_check, const std::string &dictionary) {
    ...
    dictionary += word_to_check;
    ...
}
```

) the compiler would freak out and fail to compile the source code! If you would like to see this failing code in action, it's available on [Godbolt](https://godbolt.org/z/1W76njP8T).

## Adult Swim

Let's write a function that drives kids crazy over the summer: something that will check whether a pool-goer is eligible for getting into the pool during adult swim or whether they have to sit out for the full 15 minutes. 

The input to the function is going to be the prospective swimmer's age (an `int` seems like a good type for that!) and then the return value will be `true` if the swimmer can dive in during adult swim, and `false` otherwise (that makes the type of the return value a `bool`):

```C++
bool can_adult_swim(int &age) {
    if (age >= 18) {
        return true;
    }
    return false;
}
```

That works really well. So, let's try to write some code that uses it:

```C++
int main() {
    if (can_adult_swim(20)) {
        std::cout << "A 20 year old can swim with the adults\n";
    } else {
        std::cout << "A 20 year old cannot swim with the adults\n";
    }
    return 0;
}
```

Take a look at [Compiler Explorer](https://godbolt.org/z/bY5zqvr3s) and let me know if you see anything, well, odd. 

I know, right? Why doesn't it compile? Well, let's take a closer look and see why. We were trying to be very "smart" by making `age` a by-reference parameter to speed up function calls[^reference_for_speed].

The compiler is mad because it expects that the argument we use during the invocation be an _lvalue_. What does that mean? Well, the name _lvalue_ historically comes from the term's use to designate what you could write on the *l*eft side of an `=` in an expression statement. Over time, the definition of an lvalue has become more and more specialized, but the idea is still the same.

If you have something (call that something `s`) that can go on the left side of the `=` in an assignment statement, that means that you can update `s`'s value. Under most common circumstances, it is safe to assume that a (non-`const`) variable is an lvalue.

We know that a literal (e.g., the `20` that we are using as the argument in our small program) cannot be modified. If there were code in the definition of the `can_adult_swim` function that modified `age`, then it's easy to see that there is a problem. `age` (because it is a reference parameter) is no more than another name for the argument (albeit a temporary one) and, so, any changes to `age` in the body of `can_adult_swim` are changes to the value of the argument. But, in our usage here, the argument is a literal -- there's no way that we can change a literal!

> If we could change a literal then we would be able to write

```C++
int main() {
    int height{56};
    70 = height;
    return 0;
}
```

> or something else as equally nonsensical.

Now we know why it is illegal to use a literal as an argument for a by-reference parameter, generally. But, in the case of `can_adult_swim`, the function doesn't ever actually modify `age`. So, in _this_ case, what's the problem?

The problem is that the compiler is not smart enough to _know_ that the parameter is not modified. The way that the function is declared, it is absolutely possible that the code in the body of the function does modify it's value. So, the compiler has to play it very safe.

Does this sad fact mean that we are never allowed to use literals as arguments for by-reference parameters? No, not at all!

If the problem is that the compiler cannot guarantee that the by-reference parameter is not changed, then let's just help the compiler with judging that assessment by reassuring it that the parameter's value is not changed. How can we do that? Well, we can add a `const` to the parameter's declaration:

```C++
bool can_adult_swim(const int &age) {
```

That's awesome! Now we have told the compiler that it can give us grief if we try to modify the value of `age` inside the body of `can_adult_swim`. And, therefore, the compiler can guarantee that the parameter is never changed. Ergo[^ergo], the compiler is now safe to allow the caller of `can_adult_swim` to give a literal as an argument!

[^ergo]: I have always wanted to use the word _ergo_.

Look at the code now that the compiler is happy: [https://godbolt.org/z/h43bKv6dr](https://godbolt.org/z/h43bKv6dr).

Whew. I know that is a relatively lengthy bit of reasoning, but when you put it all together, it makes sense. The moral of the story? If you want to write a function that takes a parameter by reference _for the purpose of improving the program's efficiency_, mark that by-reference parameter as `const`. 

As we know, being able to "speak" things about our code to another programmer is really important for communication. If we want to describe the `age` parameter in 

```C++
bool can_adult_swim(const int &age) {
```
we would say, "`age` is passed by `const` ref". Yes, we programmers are so lazy that we can't even be bothered to say the whole word "reference".

[^reference_for_speed]: Remember the caveat about using references to gain a speed advantage when calling a function: If the types of the function are a fundamental type, there's no great advantage to using by-reference parameters.