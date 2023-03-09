## What's News

Mathematicians provide mathematical proof that you can walk and chew gum at the same time.

## Compound Assignment Operators

It is often the case that you have a variable and you want to update its value based (somehow) upon its current value. Updating the value (obviously) requires the use of an assignment statement. And, because we are using the previous value of the variable, we will want to evaluate that variable:

```C++
int main() {
  int age{40};
  age = age + 1;
  return 0;
}
```

That code will add `1` to `age` when, say, they have a birthday. But, that seems like a lot of typing for something that is a very common operation. There are going to be many, many times that you want to update a variable based on its current value. 

To save our fingers from fatigue, the designers of the C++ language gave us something called _compound assignment operators_. These operators let you accomplish the same thing with far less typing:

```C++
int main() {
  int age{40};
  age += 1;
  return 0;
}
```

The third line in both programs is equivalent and the latter is much shorter than the former! How cool is that?

The best part is that we can use compound assignment operators with all of our operations -- `-`, `/`, `*`, etc!

FOr instance, if I wanted to convert my (hypothetical) brother's age into Dog Years, I could write:


```C++
int main() {
    int brothers_age{42};
    brothers_age /= 7;
    ...

    return 0;
}
```

 The semantics (remember that *semantics* means "meaning" [sorry, pun intended!]) of the compound operator is: 

1. Read the existing value of the variable on the left-hand side of the operator.
2. Perform the requested operation on that value (according to the value given on the right-hand side of the operator).
3. Update the value of the variable on the left-hand side of the operator with the new value.

To check your understanding, decipher what the following code will print:

```C++
#include <iostream>

int main() {
  int share_it{75};
  share_it /= 2;
  std::cout << "share_it: " << share_it << "\n";
  return 0;
}
```

I assume that you remembered to account for the semantics of integer division! Great job!

The code above will print

```
share_it: 37
```

when executed.
