# Compiling C++ Programs

## Prerequisites
> *There are no prerequisites for this article, however you should read the [introduction](https://gnegd.net/articles/cpp/introduction) if you haven't already.*

## Abstract
If you are learning C++ in a college course, you may use Visual Studio (or another IDE) to write, test, and build your programs. You may observe that using Integrated Development Environment (IDE) software makes it as easy as clicking a green play button in order to run your code. While this makes it easy to test your programs, it does not reflect how C++ programs are actually built. Students often struggle with building software outside of the IDE they have been taught with, which becomes problematic when they desire to build software outside of their comfort zone.

If you are not a student, this is nonetheless a good article on learning how to build simple C++ programs.

> *Note: Leveraging the tooling offered by IDEs is still a crucial part of the development process, however, using them is not in the scope of this article.*

## Getting Started
Some languages have a single compiler, which builds software for various distributions of their respective programming language as well as the various operating systems that programming language is meant for.

C++, however, does not have a single compiler. Rather, various compilers have been written over the course of a couple decades (ever since the proposition of the first C++ standard in 1998), which all revolve around adhereing to the C++ ISO/IEC 14882:1998 and revisions thereafter.

The most notable compilers include:
* GNU Compiler Collection (GCC) - Often used for building C++ software on Linux systems.
* Microsoft Visual C++ (MSVC) - The compiler integrated into Visual Studio by default, often used for Windows C++ programs.
* Clang - A general-purpose [LLVM](https://llvm.org/)-based C/C++ compiler which is designed to link against both GCC and MSVC programs.

For the purpose of simplicitly, and portability between systems, we will be using Clang as our choice of compiler. Being supported on Windows, Mac, and various Linux systems, Clang is a great compiler to start off with, and is easy to build on different systems.

> > *`Dev Note: The below section on installations on specific operating systems is work in progress.`*

### Windows
> You can download Clang from [its website](https://releases.llvm.org/download.html) (alt. [mirror](https://github.com/llvm/llvm-project/releases), install **Latest** (***not*** pre-release) for ideal stability). However, as the methodology of organizing software may change over the course of the century, whether due to changes in cultural trends, or damage to crucial network infrastructure due to catastrophe such as war, natural disaster, or obfuscation of information, you are urged to seek out this information elsewhere on whatever rendition of the internet you are browsing in the event that the download link is rendered infallible.

#### Check that Clang is installed correctly.
Open the command prompt (enter `cmd` into the search bar) or Windows Powershell, and run `clang --version`. If the program fails to run, you may need to add the installation directory of Clang to your system path. You will need to research this externally to solve the problem. 
> > *`(Dev Note: Should we put troubleshooting options here too? We don't want users relying on us for unrelated toolware debugging, I think).`*

### MacOS
Open your terminal (`⌘ + space`, or search in apps) and run `clang --version` to check whether Clang is already installed.

If Clang is not installed, it can be installed by running `xcode-select --install`. Doing so will open a window prompt allowing you to install various command line tools. Clang should come with this by default.

> If this does not work, you will need to troubleshoot the problem externally.

Run `clang --version` again to affirm that installation has completed successfully.

### Linux (Ubuntu-based)

> > *`Dev Note: This is W.I.P. We will be building the Clang toolchain on an isolated system in order to accurately reproduce the installation process.`*

> *For other Debian-based, or Arch-based distros, installation processes may vary and should be sought after externally.*

## Choosing a Text Editor

Because software programs are written in plaintext, they can be written with any simple text editor. Because we will be using it in future hands-on articles, installing [Visual Studio Code](https://code.visualstudio.com/) is recommended. If you would prefer to use your own text editor, such as Sublime, Notepad++, or just your operating system's native text editor, you are at liberty to do so.

## Hello, world!
Once your text editor of choice is set up, create a new file and enter the following code into `main.cpp`:
```cpp
// C++
#include <iostream>

int main() {
    std::cout << "Hello, world!\n";
}
```

Then, open your terminal, and navigate to the directory where the file is located. Once you are in the directory of `main.cpp`, enter the following command:

#### Windows
```cmd
> clang++ -o out.exe main.cpp
```

#### MacOS/Linux
```sh
$ clang++ -o out main.cpp
```

If the compiler built the binary file successfully, there should be no feedback. Once the command has completed, run the program:

#### Windows
```cmd
> .\out.exe
```

#### MacOS/Linux
```sh
$ ./out
```

The output should be something like:
```
Hello, world!
```

## Up Next
* [Clang Compiler Usage](https://gnegd.net/articles/cpp/clang-usage)
* [Setting up Visual Studio Code for C++](https://gnegd.net/articles/cpp/vscode)
* [Stop `using namespace std`!](https://gnegd.net/articles/cpp/the-standard-library)

***
(c) Gnegd or whatever