# The Standard Library - Stop `using namespace std`!

## Table of Contents
- [Prerequisites](#prerequisites)
- [Abstract](#abstract)
- [What is `std`?](#what-is-std)
- [Namespaces](#namespaces)
- [The Standard Namespace](#the-standard-namespace)
    - [`using namespace std;`](#using-namepace-std)

## Prerequisites
> *There are no prerequisites for this article, however you should read the [introduction](./introduction.md) and [Compiling C++ Programs](./compiling-code.md) if you haven't already.*

## Abstract
If you've taken any course that teaches and/or uses C++, you may be familiar with `using namespace std;`. Although you may never have been told what this does -- or its functionality has simply been forgotten -- you will observe that none of your assignments compile without this line of code (assuming you are one of the many students that used it). Unfortunately, this is a very bad design pattern for various reasons, as you will learn reading this article.

If you are not a current (nor former) student, you should nonetheless read this article to avoid making the mistakes of the many.

## What is `std`?
`std` refers to the standard library namespace (often referred to simply as "the standard namespace"). The standard library is a suite of classes and algorithms specified by the various C++ standards. To understand what this means, one must learn about namespaces.

## Namespaces
In C++ (and most other languages that incorporate one), a namespace is a body of functions, classes, and variables declared within a named scope. 

To refer to a symbol within a namespace, one must prefix a statement with the name of the namespace, followed by a pair of colons, and finally by the symbol in question:

```cpp
namespace foo {
    void bar();
}

int main() {
    foo::bar();
}
```

Namespaces can also be declared recursively:
```cpp
namespace foo {
    namespace bar {
        void baz();
    }
}

int main() {
    foo::bar::baz();
}
```

Namespaces are often used in C++ to prevent naming conflicts between libraries (and more importantly, the standard library).

Take, for instance, library A, which contains `fizz_buzz(int l, int u)`, and library B, which contains `fizz_buzz(int lower_bound, int upper_bound)`:

```cpp
// Library A
void fizz_buzz(int l, int u);

// Library B
void fizz_buzz(int lower_bound, int upper_bound); 
```

To even an inexperienced programmer, it makes intuitive sense that the first function belongs to library A, and the second one to library B, because the programmer reasons, based on both the function prototype of each function, as well as the conspicuous comments above each declaration, that both functions are different.

However, before a program in C++ is compiled into object code, the *preprocessor* first checks the file for naming conflicts `[citation needed]`. When a C++ compiler scans each prototype declared in a file, it ignores the names of functions' parameters, as function resolution is done only through the name of the function and the types of its parameters. Because of this, the compiler merely sees:

```cpp
void fizz_buzz(int, int);

void fizz_buzz(int, int);
```

Which is a naming conflict, because both functions share the same prototype.

An effective way to solve this issue (without renaming stuff) is by putting each function in its own namespace:

```cpp
namespace A {
    void fizz_buzz(int l, int u);
}

namespace B {
    void fizz_buzz(int lower, int upper);
}
```

thusly disambiguating both functions.

Now, we can call `fizz_buzz(int, int)` from library A by doing:

```cpp
int main() {
    A::fizz_buzz(1, 100);
}
```

As well as `fizz_buzz(int, int)` from library B:

```cpp
int main() {
    B::fizz_buzz(1, 100);
}
```

> *Note: Like any other symbol, namespaces can be declared in any header or source file.*

## The Standard Namespace
The standard namespace (`std`) contains implementations for every single function, type, and variable defined by the version of the C++ standard your compiler adheres to (more on this later).

> *Everything defined in each version of the standard library can be found at https://cppreference.com*.

Let's take a naive "hello world" program, likely your first C++ program:

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello, world!" << endl; // endl is bad, more on this later
    return 0;
}
```

In this program, we refer to two different symbols from within the standard namespace. `cout` is our default output stream, which typically writes text to a console. `endl` appends a newline to a stream and flushes it.

Now, given that *every* symbol defined by the standard is declared within the standard namespace, and that both [cout](https://en.cppreference.com/w/cpp/io/cout) and [endl](https://en.cppreference.com/w/cpp/io/manip/endl) are defined by the standard, one can rewrite the above program as:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, world!" << std::endl;
    return 0;
}
```

So, what is `using namespace std;` exactly?

### `using namepace std;`

The above statement is a `using-directive`, which injects symbols declared within the standard namespace into the global scope, allowing you to refer to any symbol within the standard library without prepending `std::`. However, [this behavior is actually condemned by the official C++ standard](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rs-using-directive), and should be omitted in order to avoid resolution conflicts.

The primary purpose of a using-directive is to allow easier usage of nested namespaces in higher scopes. For instance, let's take our old nested library:

```cpp
namespace foo {
    namespace bar {
        void baz();
    }
}
```

Instead of calling `foo::bar::baz()` every time we want to invoke `baz()`, we can simply inject the `bar` namespace into our main function:

```cpp
int main() {
    using namespace foo::bar;
    baz();
}
```

And indeed, this is a common practice. Let's take, for instance, a simple C++ program which displays the number of milliseconds elapsed since January 1st, 1970:

```cpp
#include <iostream>
#include <chrono>

int main() {
    std::chrono::milliseconds ms = std::chrono::duration_cast<std::chrono::milliseconds>(std::chrono::system_clock::now().time_since_epoch());
    std::cout << "Time since Jan 1, 1970: " << ms.count() << "ms" << std::endl;
}
```

Depending on the way this article is rendered on your browser, you are either required to scroll horizontally to see the rest of the code, or the text has wrapped around on itself (or it's just really long). It is self-evident that nobody wants to read this, let alone write it. So, we can mitigate the tediousness with using-directives:

```cpp
#include <iostream>
#include <chrono>

int main() {
    using namespace std::chrono;
    milliseconds ms = duration_cast<milliseconds>(
        system_clock::now().time_since_epoch()
    );
    std::cout << "Time since Jan 1, 1970: " << ms.count() << "ms" << std::endl;
}
```

A little lengthy, but preferrable to the alternative.

## Up Next

> *Nothing yet.*

***
