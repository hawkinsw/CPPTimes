## What's News
[Paramount Skydance](https://www.bloomberg.com/quote/PSKY:US) announced today that it will be using some of the proceeds from the synergies of its [recently-completed merger](https://apnews.com/article/paramount-skydance-media-cbs-trump-merger-a030c4f2c1903ed0e7f927782a64fcc0) to fund a sequel to the hit Tom Hanks film [_Cast Away_](https://en.wikipedia.org/wiki/Cast_Away).

## Casts

Assume that you have a function with the following declaration:

```C++
double trouble(double* d) {
  *d = 0.0;
  return *d;
}
```

To safely call that function, you must provide an object whose type is _pointer to `double`_ as an argument. For instance,

```C++
double dv{0.0};
auto result{trouble(&dv)};
```

would work just fine.

On the other hand, if you provided an object whose type is, say, _pointer to `int`_ as an argument, your program would contain undefined behavior.[^ub] For instance,

```C++
int di{0.0};
auto result{trouble(&di)};
```

The good news is that the compiler will not let us write code like that! We would get a compilation error along the lines of

```
<source>:17:13: error: no matching function for call to 'trouble'
   17 |   auto result{trouble(&di)};
      |             ^~~~~~~
<source>:1:8: note: candidate function not viable: no known conversion from 'int *' to 'double *' for 1st argument
    1 | double trouble(double* d) {
      |        ^       ~~~~~~~~~
1 error generated.
```

[^ub]: The undefined behavior is a result of the program's attempt to write a value with the type _`double`_ to a place in memory where an _`int`_ is stored.

Whew. Saved by the bell!

## Be Careful What You Ask For

C++ is a powerful tool. It's so powerful that it offers us tools that might require us to be very careful when we use them -- working without a net is exhilirating, but requires great attention to detail.

A C-style cast (also known in C++ as an [explicit cast](https://cppreference.com/cpp/language/explicit_cast)) tells the compiler to try to find some type of cast that will compile. One of the casts that it will try in that search (and early in the search, in fact) is [`static_cast`](https://en.cppreference.com/cpp/language/static_cast). But, if using that type of cast would cause a compilation failure, C++ will try others. One of the other types of casts that it will try is the [`reinterpret_cast`](https://cppreference.com/cpp/language/reinterpret_cast) -- a very powerful tool that must be used with care to avoid introducing undefined behavior into our programs.

As a result, when C++ programmers write a C-style cast, they are really giving up some amount of control. C++ tries to help the programmer by finding a cast that will succeed ...  but C++'s definition of success is whether the cast will allow for successful compilation -- not whether it will generate a correct program!

```C++
int di{0.0};
auto result{trouble((double*)(&di))}
```
