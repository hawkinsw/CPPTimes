## What's News

The Union of Concerned C++ists is spending the last day of their yearly conclave  editing the their draft communique that will summarize this week's meetings. All the parties who will be signatories of the group's final statement want to make sure that their platform on the suitability of pseudocode for silencing semantic slipups strongly worded.


## A Young Programmer's Journey from Milan to Minsk

Whether you realize it or not, a requirement to write pseudocode (by your teacher, professor, boss or best friend) is not a punishment. Programmers outline our solutions as a low-stakes means to test our ideas for solving a problem before we start worrying about the details that come with writing code. By writing pseudocode, we can drop all the distractions of "Does this `;` go here? or there?" and focus on the constraints of the problem and building an efficient solution. Writing pseudocode helps us balance the need for specifying our solution rigorously and precisely with the need for freedom and creativity that makes finding a solution possible. When we have finished our solution outline, we know that we have examined the problem and solution thoroughly and can use the pseudocode as a guide for our implementation.

I think that the best way to see the value of writing pseudocode is to, well, see it in action. We will work together from pseudocode to implementation on a function that will count the number of consecutive consonants at the beginning of a given word.

Our approach will be to write successively more detailed pseudocode until we are able to make a rather mechanical translation from pseudocode to working C++.

## Count Consonants At Front

Our mission, and, yes, we have chosen to accept it, is to count the consonants that appear consecutively at the beginning of a word (_word_ = `1`). If the first letter of the word is a vowel, then, well, there are $0$ consonants at (_at_ = `0`) the beginning of the word. On the other hand, if the word contains nothing but consonants (_brr_, it's cold in here, = `3`) then the function, effectively, calculates the length of the word. 

### Assumptions

At our disposal we have a function (and it works) that accepts a single parameter (a `char`) and determines whether the letter in that parameter is a consonant or not (its return type is `bool`). We know that the name of that function is `is_consonant`. Because we are programmers and enjoy details, we want to see the function's declaration. Here it is:

```C++
bool is_consonant(char);
```

### Take 1

At a very high level, our pseudocode to count the consonants at the start of a word might be

> Starting at the beginning of the word, look at each letter, one at a time. If that letter is a consonant, add $1$ to a _count_ and continue to the next letter. If that letter is not a consonant, the number of consonants at the front of the word is the current value of the _count_.

### Take 2

That's definitely a start, but there are some details that we omitted that are pretty crucial:

**add $1$ to a _count_**

That pseudocode directive implies that our _count_ always has a value. Because we are detail oriented (as are all good programmers), we should be explicit about _count_'s value before our pseudocode starts to consider letters in the word[^anthro].

[^anthro]: Throughout this description we will anthropomorphize our pseudocode. We will say that our pseudocode "does" things but it's really more accurate to say that our pseudocode, when converted to actual code, will become machine code that instructs the computer to do those things.

For clarity of exposition, a name for that _count_ would be good. We will call it `ccount`.

Here's a slightly-more-refined version of the pseudocode:

1. Initialize `ccount` to $0$.
2. Starting at the beginning of the word, look at each letter, one at a time.
3. If that letter is a consonant, add $1$ to `ccount` and continue to the next letter.
4. If that letter is not a consonant, the number of consonants at the front of the word is `ccount`.

### Take 3

We aren't going to break the momentum. We will continue forward and see if we can make 

**If that letter is a consonant, ...**

a little more explicit. Because we have the `is_consonant` function, we can just use it. To see if _that letter_ is a consonant, we will just call that function:

1. Initialize `ccount` to $0$.
2. Starting at the beginning of the word, look at each letter, one at a time.
3. if `is_consonant(`_that letter_`)` is `true` , add $1$ to `ccount` and continue to the next letter.
4. If that letter is not a consonant, the number of consonants at the front of the word is `ccount`.

Notice how our pseudocode is starting to look more and more C++-ish? Pretty cool!

### Take 4

Our pseudocode is going to handle normal cases really well. Just what is a normal case? We'll define it by contrast with a _corner case_[^case] and given an example: Remember earlier we said that `count_consonants_at_front` would need to handle the case where all the letters in the word are consonants? Let's take `brr` as an example and see weather (pun intended!) our pseudocode performs. 

[^case]: Corner cases are sometimes also called _edge cases_ or _special cases_.

| `ccount` value | _that letter_ | Description | Pseudocode Step # |
| -- | -- | -- | -- |
| `0` | _?_ | Initializing `ccount`. | (1) | 
| `0` | _b_ | | (2) |
| `1` | _b_ | _b_ is a consonant; add $1$. | (3) |
| `1` | _b_ | Let's continue ... | (3) |
| `1` | _r_ | | (2) |
| `2` | _r_ | _r_ is a consonant; add $1$ | (3) |
| `2` | _r_ | Let's continue ... | (3) |
| `2` | _r_ (second one!) | | (2) |
| `3` | _r_ | _r_ is a consonant; add $1$. | (3) |
| `3` | _r_ | Let's continue ... | (3) |
| ... |||

Well, that's not great. We looked at each letter in the word but we did not execute Step (4). Because Step (4) is the step that produces the result, it seems that if the input to our function is all consonants then our pseudocode fails to produce a value.

Something needs to be fixed.

The good news is that once our specified procedure has looked at every letter in the word but not found a vowel, `ccount` faithfully tracks the number of consonants at the beginning of the word. Is there a way that we can reorganize our pseudocode so that its final step when processing the _normal_ case (where not all letters in the word are consonants) and this special case are the same?

If we specify that the final step of our pseudocode, the one that produces the result, executes once our pseudocode has looked at each of the letters in the word _necessary to correctly calculate the number of consonants at the beginning of the word_, then I think we have something to work with. 

4. Once we have looked at each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word, the number of consonants at the beginning of the word is `ccount`.

In the special case, our pseudocode considers every letter in the word -- certainly that is _each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word ..._.

What about the normal case? I think that we need to make a small adjustment for everything to fit just right. Let's add some detail to Step (3):

3. if `is_consonant(`_that letter_`)` is `true`
   1. add $1$ to `ccount` and look at the next letter.
   2.  otherwise, declare that we have considered _each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word ..._.

What just happened? We modified our pseudocode so that its execution reaches Step (4) in both the normal and special cases. We were able to make this reconfiguration because no matter whether the word contains all consonants or not, `ccount` is always the number of consonants at the beginning of the word.

Let's recap:

1. Initialize `ccount` to $0$.
2. Starting at the beginning of the word, look at each letter, one at a time.
3. if `is_consonant(`_that letter_`)` is `true`
   1. add $1$ to `ccount` and look at the next letter.
   2. otherwise, declare that we have considered each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word.
4. Once we have looked at each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word, the number of consonants at the beginning of the word is `ccount`.

Here is an updated version of the chart from above using our new pseudocode:

| _ccount_ value | _that letter_ | Description | Pseudocode Step # |
| -- | -- | -- | -- |
| `0` | _?_ | Initializing `ccount`. | (1) | 
| `0` | _b_ | | (2) |
| `1` | _b_ | _b_ is a consonant. | (3) |
| `1` | _b_ | Add and continue. | (3.1) |
| `1` | _r_ | | (2) |
| `1` | _r_ | _r_ is a consonant. | (3) |
| `2` | _r_ | Add and continue ... | (3.1) |
| `2` | _r_ (second one!) | | (2) |
| `2` | _r_ | _r_ is a consonant. | (3) |
| `3` | _r_ | Add and continue. | (3.1) |
| `3` |  | We have looked at all the letters ... | (4) |
| `3` |  | Our calculation is correct. | (4) |

Just for the sake of completeness, let's see whether our pseudocode can handle the normal case (e.g., _cat_):

| _ccount_ value | _that letter_ | Description | Pseudocode Step # |
| -- | -- | -- | -- |
| `0` | _?_ | Initializing `ccount`. | (1) | 
| `0` | _c_ | | (2) |
| `1` | _c_ | _c_ is a consonant. | (3) |
| `1` | _c_ | Add and continue. | (3.1) |
| `1` | _a_ | | (2) |
| `1` | _a_ | _a_ is _not_ a consonant. | (3) |
| `1` | _a_ | Make the declaration ... | (3.2) |
| `1` | _a_ | We have looked at all the letters ... | (4) |
| `1` | _a_ | Our calculation is correct. | (4) |


### Take 5

Whether we are willing to admit it or not, there is a loop hiding in the pseudocode that we have not made explicit. Let's do that now. 

Steps (2) and (3) are clearly part of the loop. They work over and over as each letter of the word is considered. Let's step back and rephrase those steps at a higher level before adding more detail -- a retreat to move forward, if you will:

> Consider the letters one at a time and use a variable, `that_letter` to refer to those letters. If `that_letter`_ is a consonant, then increment `ccount` and repeat. Otherwise, stop the loop.

Taking a moment to regroup like we just did is a really important part of writing pseudocode. It gives us a chance to ground ourselves and make sure that we are on the right track. To confirm that we are working in the right direction, let's see if we can annotate what we just wrote with different parts of our existing pseudocode:

> Consider the letters one at a time (**(2)**) and use a variable, `that_letter` to refer to those letters. If `that_letter`_ is a consonant (**(3)**), then increment `ccount` and repeat (**(3.1)**). Otherwise, stop the loop (**(3.2)**).

I hope that you can see that we are in perfect alignment. Nicely done! How else does our rewrite help us? Well, we can rewrite our pseudocode in a way that helps us advance toward actual code: We can make the loop explicit.

### Take 6

It is more and more common these days that when we go to a store (a grocery, a pharmacy, etc), the good things that we want to buy are locked behind the counter. We have to ask for help getting what we want. I often ask an employee to, "Hand me that tube of toothpaste."[^toothpaste]. 

[^toothpaste]: No, I don't know who is stealing toothpaste, but apparently people do.

The attendant _usually_ says, "Which one?" Then, I try to be a little more clear: "Can you hand me the tube of toothpaste three from the left.". Then I get what I want!

I bet that we can apply that repartee in this context. As we do our calculation, we want _that letter_. "Which one?", says the attendant? "Can you hand me the letter _x_ from the left?"

And there it is!! _x_! If our pseudocode is to work as intended, we may need to ask the attendant for every letter. In other words, we will need to ask

- "Can you hand me the letter 0 from the left?"
- "Can you hand me the letter 1 from the left?"
- "Can you hand me the letter 2 from the left?"

and so on until ... Until when? Until we ask

- "Can you hand me the letter _L_ from the left?"

where _L_ is one less than the length of the word. After all, it makes no sense to ask

- "Can you hand me the letter 3 from the left?"

if the word doesn't have at least four letters![^zero]

[^zero]: Remember, we count from $0$ as computer scientists.

We can use our PhD in Purchasing to make

2. Starting at the beginning of the word, look at each letter, one at a time.
more explicit.

First, 

- 2(a) Initialize `i` to $0$.
- 2(b) As long as `i` is less than the length of the word,

Fantastic! Let's see where we stand by looking at an updated version of the entire pseudocode:

1. Initialize `ccount` to $0$.
2. Initialize `i` to $0$.
3. As long as `i` is less than the length of the word,
4. if `is_consonant(that_letter)` is `true`
   1. add $1$ to `ccount` and look at the next letter.
   2. otherwise, declare that we have considered each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word.
5. Once we have looked at each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word, the number of consonants at the beginning of the word is `ccount`.

### Take 7

Thanks to our current approach, we can now be more explicit about `that_letter`. Because we have `i`, we can use it to explicitly access letters in the word. Let's split Step (4) into Steps (4a) and (4b):

- 4(a): Make "the letter `i` from the left" `that_letter`.
- 4(b): if `is_consonant(_that_letter_)` is `true`

It seems like 4(b) is _really_ close to actual code, so, let's just finish the deal:

- 4(b): `if (is_consonant(_that_letter)) {`

Let's see where we stand overall:

1. Initialize `ccount` to $0$.
2. Initialize `i` to $0$.
3. As long as `i` is less than the length of the word,
4. Make "the letter _i_ from the left" `that_letter`.
5. `if (is_consonant(_that_letter)) {`
   1. add $1$ to `ccount` and look at the next letter.
   2. otherwise, declare that we have considered each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word.
6. Once we have looked at each letter in the word necessary to correctly calculate the number of consonants at the beginning of the word, the number of consonants at the beginning of the word is `ccount`.

### On The Precipice

There comes a time as we write pseudocode that it becomes easier to write _real code_ with a smattering of pseudocode instead of writing _pseudocode_ with a smatter of real code. I think that we are at that point. Let's turn what we have into real code, as much as possible:

```C++
int ccount{0};
```

for Step (1).

```C++
int i{0};
```

for Step (2).

```C++
while (i < length_of_the_word) {
```

for Step (3).

```C++
char that_letter{letter_i_from_the_left};
```

for Step (4).


```C++
if (is_consonant(that_letter)) {
```

for Step (5).

```C++
   ccount++;
   i++;
   continue;
```

for Step (5.1).

```C++
} else {
    break;
```

for Step (5.2).

And, finally, 

```C++
return ccount;
```

for Step (6).

Whew. That was exhausting but it was a major step. Let's make sure that we have all of our `{`s and `}`s matching and write it all out:


```C++
int ccount{0};
int i{0};
while (i < length_of_the_word) {
    char that_letter{letter_i_from_the_left};
    if (is_consonant(that_letter)) {
        ccount++;
        i++;
        continue;
    } else {
        break;
    }
}
return ccount;
```

### Taking Liberties

Notice what happened here. Before, when working mostly with pseudocode, we used some bits of quasi-C++. Now we are working mostly with real C++ and we need some quasi-pseudocode. What do I mean?

Look at `length_of_the_word` and `letter_i_from_the_left`. We haven't defined those precisely yet in real C++. As part of transition to real code, we had to write _something_ where those appear. So, we made up something and we'll go back to it! 

It may sound silly, but that is a really important technique: During this transition between pseudocode and real code, we don't to let a need to figure out the details prevent us from making progress.

If you are uncomfortable using `made_up_variable_names`, I sometimes like to use `/* comments to leave notes for myself */`:

```C++
int ccount{0};
int i{0};
while (i < /* the length of the word */) {
    char that_letter{/* the letter i from the left*/};
    if (is_consonant(that_letter)) {
        ccount++;
        i++;
        continue;
    } else {
        break;
    }
}
return ccount;
```

### Wrap It Up

Our code is an implementation of the `count_consonants_at_the_front` function. We've been assuming that there is a _word_ that we are inspecting. If we put our code into an actual function, we can be explicit about the provenance of that word:

```C++
int count_consonants_at_front(std::string word) {
    int ccount{0};
    int i{0};
    while (i < length_of_the_word) {
        char that_letter{letter_i_from_the_left};
        if (is_consonant(that_letter)) {
            ccount++;
            i++;
            continue;
        } else {
            break;
        }
    }
    return ccount;
}
```

### Find Some Meaning

Our final steps are to realize `letter_i_from_the_left` and `length_of_the_word`. We can use the [`at`](https://en.cppreference.com/w/cpp/string/basic_string/at) and [`length`](https://en.cppreference.com/w/cpp/string/basic_string/size) member functions of the [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string) type, respectively, in order to accomplish the task:

```C++
int count_consonants_at_front(std::string word) {
    int ccount{0};
    int i{0};
    auto length_of_the_word{word.length()};
    while (i < length_of_the_word) {
        char that_letter{word.at(i)};
        if (is_consonant(that_letter)) {
            ccount++;
            i++;
            continue;
        } else {
            break;
        }
    }
    return ccount;
}
```

### Clean Up Time

We're done!! The code that we have works perfectly. In fact, you can check it out for yourself: [https://godbolt.org/z/TYcKPrc36](https://godbolt.org/z/TYcKPrc36).

Unfortunately, that's about the best that we can say about our code. It works. It's not great, though. And, we're all about "that great".

Here are a few ways that we can make our code great:

1. The `while` loop is just begging to be converted into a `for` loop. Why? Because the loop is so regular: `i` starts at `0`, increases by `1` at each iteration and has a simple condition for evaluating whether execution continues.

```C++
int count_consonants_at_front(std::string word) {
    int ccount{0};
    auto length_of_the_word{word.length()};
    for (int i{0}; i < length_of_the_word; i++) {
        char that_letter{word.at(i)};
        if (is_consonant(that_letter)) {
            ccount++;
            continue;
        } else {
            break;
        }
    }
    return ccount;
}
```

2. `length_of_the_word` is a variable whose value is only used one place. In cases like that, it usually makes sense to use the value of the expression directly[^when_variable].

```C++
int count_consonants_at_front(std::string word) {
    int ccount{0};
    for (int i{0}; i < word.length(); i++) {
        char that_letter{word.at(i)};
        if (is_consonant(that_letter)) {
            ccount++;
            continue;
        } else {
            break;
        }
    }
    return ccount;
}
```

3. The same transformation can be done for `that_letter`:

```C++
int count_consonants_at_front(std::string word) {
    int ccount{0};
    for (int i{0}; i < word.length(); i++) {
        if (is_consonant(word.at(i))) {
            ccount++;
            continue;
        } else {
            break;
        }
    }
    return ccount;
}
```

4. The final transformation is a little more complex, but applying it will really make our code shine. Having a `continue` _and_ a `break` in the body of a loop is very abnormal. Remember that loops really _want_ to keep looping until their conditions are invalid. Loops gonna loop. Here, the loop wants to continue as long a `i < word.length()`. Until that is the case, it is an exceptional situation that the loop would cease to continue. In other words, in the body of the loop, it is the _normal_ case that we would add one to `ccount` and continue. The exceptional case is that the loop would end when the letter is _not_ a consonant. Flipping the condition around in the body of the loop will make the code easier to read for our fellow programmers:

```C++
int count_consonants_at_front(std::string word) {
    int ccount{0};
    for (int i{0}; i < word.length(); i++) {
        if (!is_consonant(word.at(i))) {
            break;
        }
        ccount++;
    }
    return ccount;
}
```

Now _that_ is code that really shines.

See it in action at [https://godbolt.org/z/KhnY7WW8x](https://godbolt.org/z/KhnY7WW8x).

## What Have You Done?

Now that you have read this issue of the C++ Times you have additional experience building a solution to a problem. You have more experience expressing those solutions using pseudocode and translating that pseudocode to real C++ code. Finally, you have gained experience about how to _refactor_ your code to really make it shine.

[^when_variable]: One time when it _does_ make sense to use a variable for an expression whose value is used a single time is when giving that value a name would aid readability. Here, though, the difference between `length_of_the_word` and `word.length()` does not make the code any more or less readable.
