# Trailing Return Types

## Prerequisites
> [Type declaration](https://gnegd.net/drafts/cpp/declarations.md)<br>
> [The `auto` keyword](https://gnegd.net/drafts/cpp/auto.md)

## Abstract

A trailing return type describes a function's return type appended to the end of the function's [prototype](https://gnegd.net/drafts/prototypes.md), as opposed to the beginning.

Take a simple function `add(int, int)` for instance:
```cpp
// C++

/// @brief Returns the sum of two ints.
int add(int, int);
```

In this prototype, the function `add(int, int)` is prefixed with its return type, which, of course, discerns the data type of the value the function is expected to return. However, in other languages, the return type of a function may be inferred, and in some cases, the return type is denoted at the end of the function. `_(I will do more research on this topic so what im saying makes more sense... lol)_`

<br>


For instance, the same function in JavaScript may look like:
```js
// JavaScript

const add = (a, b) => a + b;
```

Or in Rust:
```rs
// Rust

fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

<br>

Knowing and understanding these two languages is not necessary. What's important is to observe the ways in which each language structures function prototypes. In JavaScript, the types are inferred (not to mention that JavaScript is [loosely-typed](https://gnegd.net/draft/paradigms.md)), and in Rust, the type is appended at the end of the prototype rather than the beginning.

<!-- 
In C++, we can achieve mostly what Rust does using *trailing return types.* An example using `add(int, int)` may look like: -->
In C++, we can **sort of** achieve this with the `auto` keyword:
```cpp
// C++

auto add(int a, int b) {
    return a + b;
}
```

This alone compiles just fine, because the compiler can infer the return type of `add(int, int)` by virtue of the sole return statement. But this design pattern becomes a problem when we want to write function declarations:
```cpp
// C++

auto add(int a, int b); // Error
```

However, we can amend this bug by appending a ***trailing return type***:
```cpp
// C++

auto add(int a, int b) -> int;
```
Using the `auto` keyword, the compiler can deduce the return type of `add(int, int)` based on its **trailing** type, even without including the definition for the function.

Semantically-speaking, this has no structural difference from prepended return types. You can do `int add(int, int);` or `auto add(int, int) -> int;` and observe no changes in the control flow of the function. For instance, you can do this to `main()` as well:
```cpp
// C++

auto main() -> int {
    puts("Hello, world!\n");
}
```

***

## Bonus Section
I recommend you read up on [templates](https://gnegd.net/drafts/cpp/templates) before reading this section. You have been forewarned!

> *"If that's the case, why did they add trailing return types in C++11?"*

Great question!

Let's go ahead and generalize our `add(int, int)` function with a template:
```cpp
// C++

template <class T>
auto add(T a, T b) -> T;
```

Without making any other assumptions, this code seems satisfactory. However, it should be noted that not all addition operations involve the same data type, or even return the same data type. For instance, one may add an `int` and a `double`:
```cpp
int a = 5;
double b = 2.0;

auto x = a + b;
```

So how should we generalize this, then? Well, we can use two different types in our template:
```cpp
// C++

template <class T, class U>
auto add(T a, U b) -> ???;
```

Ah, there's our first issue. What should the trailing return type be? Let's start off with just using `auto` and letting the compiler figure out the rest:
```cpp
// C++

template <class T, class U>
auto add(T a, U b) {
    return a + b;
}
```

This implementation is fine, however we yet again run into the issue of prototyping.

If we want to specify the return type in the prototype of the function rather than relying on type inference, one could use the [`decltype`](https://en.cppreference.com/w/cpp/language/decltype) specifier:
```cpp
// C++

template <class T, class U>
auto add(T a, U b) -> decltype(a + b);
```

***

(c) MakotoMiyamoto or something