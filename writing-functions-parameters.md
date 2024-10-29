## What's News

The World's Most-Wanted C++ programmer, Bjarne Stroustrup, who has gone by various criminal aliases (including Bjarne Scope and Binary Stroustrup), has finally been captured. Police from the special For Loop of the federal Marshalls picked up Mr. Stroustrup today in NYC.

## Why Do I Have To State The Obvious?

The parameters of a function are _the_ way to give the user the power to customize the behavior of a function. Parameters are like the sliders that we turn to customize the level of bass in our AirPods. Powerful functions often provide the caller many, many options for customization.

That said, it may not always be strictly necessary that the caller specify arguments for each of those parameters. Some powerful functions are designed to work well with some _default_ values. Having default values for parameters is a perfect way to achieve simplicity and power. If a user does not know that they need special handling, then the defaults are great and make the function easy to use. On the other hand, users are not restricted from using a function in a way that fits their _exact_ needs.

For example, consider a function that inflates the tires on our cars. It could be defined like

```C++
bool inflateTires(double toPSI) {
    ...
    return true;
}
```

The function will inflate each of the tires of the car to a certain PSI. That's great, but I am not much of a car person. When I take my Camry to the BP and add air to the tires, I simply want to fill up the tires to the manufacturer-recommended level. In other words, I want to invoke the `inflateTires` function but I just want to use the default value for `toPSI`.

On the other hand, there are people who take their SUVs to drive in the sand on the beach. They have to inflate their tires to a very specific PSI so that they get the maximum grip. They are also interested in being able to escape water at high tide staying mobile in very loose sand. For them, they will want to call `inflateTires` with a non-manufacturer-recommended pressure level.

C++ gives us the tools to meet my needs and the needs of the off roaders. In C++, the programmer can specify the default value for parameters for functions. When a programmer invokes a function with a default parameter, they can accept the default value (simply by not passing an argument corresponding to the parameter) or give a customized value (simply by specifying an argument for the parameter). For instance,

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

In the first invocation of the `inflateTires` function, the programmer is saying that they do not want to accept the default level of 31.0 PSI and wants to inflate their tires to only 28.0 PSI. In the second invocation of the `inflateTires` function, the programmer is saying that they want to accept the normal, default value for the inflation target. 

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
1. The values for default arguments are only given in the function declaration. In many cases, the function declaration and definition are combined so this rule seems extraneous. However, when the declaration and definition of a function with default arguments are separate, the default values must not be repeated at the point of the definition.
2. When reading the parameters from left to right, every parameter to the right of the leftmost parameter with a default value must also have default values. In other words, 

```C++
void f(int a, int b = 5, int c) {
    ...
}
```
is invalid.

3. When reading arguments from left to right, every argument to the right of the leftmost ommitted argument must _also_ be ommited (and, therefore, the caller must accept the default). In other words,

```C++
void f(int a, int b = 5, int c = 6) {
    ...
}

int main() {
    f(5, 6);
}
```

will call `f` with `b` equal to `5` and accept the default for `c` -- there is no way to "skip" an argument. The caller cannot, for example, accept the default for `b` and customize `c`. It's all or nothing after accepting the first default argument value.

## I Am More Than a Coin Flip

In C++ there is a fundamental limit that seems to affect the utility of functions -- can you name it?

That's right -- you can only return a _single_ value from a function[^num_return_values]. So, how, then, are we to write a function that prompts the user for, say, their age and

- indicates whether the age they entered was reasonable *and*
- returns that value when it is reasonable?

Tricky.

[^num_return_values]: There are other possible solutions to the C++-only-`return`s-a-single-value problem, but this solution is very common and also illustrates a tool that can be used to accomplish other feats.

As we have learned previously, arguments are matched with parameters at runtime and the initial values of parameters are _copies_ of the values of the corresponding argument at the time of the function invocation. The arguments in this type of a function invocation are said to be *passed by value.* Because "value parameters" occupy a completely different place in memory than the argument that gave them their initial value, any changes to the parameter in the body of the function are *not* seen by the caller. 

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

Just before the the invocation of the `get_age` function, the state of the local variables are as shown in the image below.

![](./graphics/Reference%20Parameters%20-%200.jpg)

Upon invocation of the `get_age` function, the value of the `user_entered_age` argument is *copied* to the value of the `user_age` parameter. A new scope is created that will persist as long as the `get_age` function executes and you can think of that scope as demarcating the space used to store all the function's local variables, especially its `user_age` parameter.

![](./graphics/Reference%20Parameters%20-%201.jpg)

![](./graphics/Reference%20Parameters%20-%202.jpg)

Once the user has entered their input and it has been processed by the `std::cin >> user_age;` statement, the values of the variables in memory are as shown below.

![](./graphics/Reference%20Parameters%20-%203.jpg)

Finally, when `get_age` completes and execution passes back to the point in `main` where `get_age` was invoked, the `get_age` scope is vacated and the space allocated to the storage of the local variables *for this particular invocation of the function* are destroyed.

![](./graphics/Reference%20Parameters%20-%204.jpg)

As a result of the semantics just described, an invocation of `get_age` will complete without any influence on `user_entered_age` in the scope of the `main` function (although the invocation *does* update the value of `valid_age` by way of its return value).

Given that the _obvious_ rationale for writing a function like `get_age` is, you know, getting someone's age, this behavior is _far_ from ideal. What are we going to do?

We will turn to _pass-by-reference_ parameters. "By reference" parameters are a way for the person writing the function and the person calling the function to provide an _alias_ within the scope of the called function that _references_ a variable from the calling scope. To see how by value and by reference parameters are different, let's redefine `get_age` using reference parameters and see how the program's execution differs as a result.

The situation just before the invocation of the updated definition of the `get_age` function, is the same as the situation just before the older version of the `get_age` function.

![](./graphics/Reference%20Parameters%20-%200.jpg)

The moment that the function `get_age` is invoked, things take a turn for the different! Again, a new scope is created to provide space for the values of the variables local to the called function. This time, however, the scope contains `user_age` as simply a named alias for the variable named by the argument: `user_entered_age`. 

![](./graphics/Reference%20Parameters%20-%201r.jpg)

![](./graphics/Reference%20Parameters%20-%202r.jpg)


Once the user has entered their input and it has been processed by the `std::cin >> user_age;` statement, the values of the variables in memory are as shown below.

![](./graphics/Reference%20Parameters%20-%203r.jpg)

Note _very well_ that the assignment to the `user_age` variable updates the value assigned to the `user_entered_age` in the scope of the `main` function _because it is just an alias (or reference) to that variable_. Any time the fictional operator inside our computer attempts to access the value of the `user_entered_age` variable during execution of the by-reference version of the `get_age` function, think about them walking through the portal to the other side as they read the value from memory. At the same time, any time the fictional operator inside our computer attempts to update the value of the `user_entered_age` variable during execution of the by-reference version of the `get_age` function, think about them walking through the portal to the other side as they place a new value in memory.

Finally, when `get_age` completes and execution passes back to the point in `main` where `get_age` was invoked, the `get_age` scope is vacated and the space allocated to the storage of the local variables _for this particular invocation of the function_ are destroyed.

![](./graphics/Reference%20Parameters%20-%204r.jpg)

Unlike the invocation of the by-value version of `get_age`, the changes to `user_age` have resulted in a change to `user_entered_age` (while maintaining the update to the `valid_age` flag that _also_ occurred in the by-value version). How cool! It's exactly as we hoped!

The only thing cooler than _how_ by-reference functions work is the way that we indicate that a parameter is passed by reference. Functions can have a mix of by-value and by-reference parameters. A by-reference parameter is indicated with a `&` in front of the name of the parameter -- there is no change to the way that the function is called, as demonstrated by the code above.

![](graphics/popeil.jpg)
But just wait, there's more! `int`s and `double`s and `char`s can hold (relatively) few values and, therefore, require relatively little space in memory. As a result, copying the value of an argument to give the corresponding parameter its initial value will not take very long when those are the types that are passed by value. 

On the other handle, `std::string`s can take quite a bit of memory. Just think about a `std::string` that contains the entire Oxford English Dictionary and needs to be passed to a function that performs spell check. In order to spellcheck a user's Tweetstorm, the spell check function is invoked. There will be one copy of the dictionary performed per word in the user's rant! That seems seriously wasteful. By-reference parameters to the rescue again -- the code that the compiler generates to create a reference runs very fast! For cases like passing `std::string`s (or `std::vector`s or any other compound types) to functions as parameters, it makes sense to pass them by reference to save time (not to mention the space savings from not having two copies of the dictionary's words in memory at the same time.). 

It's a perfect solution? Not so fast. As a result of passing the `std::string` (or `std::vector`, etc) by reference, the caller has lost the ability to assume that the variable it provided as an argument to the function will be unchanged as a result of that function invocation. Remember: If the argument were passed by value, even assuming that the parameter is changed in the function, the value of the caller's argument does not change (because those modification operations done in the space of the invoked function act on the per-invocation _copy_ of the other variable).

Can we get the best of both worlds? Amazingly, the answer is _yes_, we can! When we are writing a function and declaring that a parameter is by reference _just for efficiency sake_ and not because we need to modify it, we will declare the parameter to be a by-`const`-reference parameter. The `const`ness of the by-reference parameter will ensure the caller that the value of its precious argument will not be tampered during the function's execution. 

How can it be so sure? Because if the compiler _did_ attempt to edit it, the source code would not compile!

The function declaration for the hypothetical spell check function would look something like ...

```C++
bool is_spelled_correctly(const std::string &word_to_check, const std::vector<std::string> &dictionary) {
    ...
}
```


If the function included code that attempted to "learn" a misspelled word (e.g.,

```C++
bool is_spelled_correctly(const std::string &word_to_check, const std::vector<std::string> &dictionary) {
    ...
    dictionary.push_back(word_to_check);
    ...
}
```

) the compiler would freak out and fail to compile the source code!