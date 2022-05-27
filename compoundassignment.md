## What's News

Mathematicians provide mathematical proof that you can walk and chew gum at the same time.

## Compound Assignment Operators

It is often the case that you have a variable and you want to update its value based (somehow) upon its current value. Updating the value (obviously) requires the use of an assignment statement. And, because we are using the previous value of the variable, we will want to evaluate that variable:

<html><head></head><body><pre>
1 int main() {
2   <font color=green>int</font> age{<font color=red>40</font>};
3   age = age + <font color=red>1</font>;
4   <font color=green>return</font> <font color=red>0</font>;
5 }

</pre></body></html>

That code will add `1` to `age` when, say, they have a birthday. But, that seems like a lot of typing for something that is a very common operation. There are going to be many, many times that you want to update a variable based on its current value. 

To save our fingers from fatigue, the designers of the C++ language gave us something called _compound assignment operators_. These operators let you accomplish the same thing with far less typing:

<html><head></head><body><pre>
1 int main() {
2   <font color=green>int</font> age{<font color=red>40</font>};
3   age += <font color=red>1</font>;
4   <font color=green>return</font> <font color=red>0</font>;
5 }

</pre></body></html>

The third line in both programs is equivalent and the latter is much shorter than the former! How cool is that?

The best part is that we can use compound assignment operators with all of our operations -- `-`, `/`, `*`, etc!

 The semantics (meaning) of the compound operator is this: 

1. Read the existing value of the variable on the left-hand side of the operator.
2. Perform the requested operation on that value (according to the value given on the right-hand side of the operator).
3. Update the value of the variable on the left-hand side of the operator with the new value.

To check your understanding, decipher what the following code will print:

<html><head></head><body><pre>
1 #include &lt;iostream&gt;
2 
3 <font color=green>int</font> main() {
4   <font color=green>int</font> share_it{<font color=red>75</font>};
5   share_it /= <font color=red>2</font>;
6   std::cout << <font color=red>"share_it: "</font> << share_it << <font color=red>"\n"</font>;
7   <font color=green>return</font> <font color=red>0</font>;
8 }

</pre></body></html>

Did you remember to account for the semantics of integer division? Great job!
