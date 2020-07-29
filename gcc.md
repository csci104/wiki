---
layout: asides
toc: true
tasks: false
---

# G++ Cheatsheet

GCC, the GNU Compiler Collection, is used to build your C++ executables.
If you're interested in reading about GCC, you can check out the [GNU website](http://gcc.gnu.org/).
You can also take ITP 439, which covers compiler development.
If you're looking for how to compile your programs before submitting, check the [compiling homework](#cheat-sheet) section below.

## Options

Here are a few of the more useful command options when using `g++`.
These compiler flags may be used in any order you wish.
However, by convention, we usually group the list of source files together and the list of options together.

### Most Common

These are generally most important options for normal use on your VM and in your assignment `Makefile`s.
You'll probably end up knowing them by heart.

- `-Wall` show all warnings.
  It turns on all standard C++ warnings about code that might cause unexpected or undefined behavior.
- `-g` provides debugging feature for your program.
  You will need this when you want to use gdb or valgrind.
- `--std=c++<##>` uses version `<##>` of C++ when compiling.
  This will allow you to use specific features of that C++ version.
  Typically, we have you use `--std=c++17`.
- `-o <filename>` compiles and links files into an executable named `<filename>`.
  The default filename is `a.out`.
- `-c` compiles and assembles files but doesn't link them.
  This is useful when building large projects to separate file compilation and minimize what is re-compiled.

### Bonus Round

Here are a couple compiler flags that we don't as often use in this class.
They're still pretty handy to know.
You can read about all the options for using `g++` [here](https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html).

- `--sys-root=<directory>` uses `<directory>` as root directory for headers and libraries.
- `-I /<absolute-path>` adds `<absolute-path>` to the compiler's search paths.
  The path must written from the root of the filesystem, `/`.
- `-Werror` make all warnings into errors *do this before submitting your code to avoid getting points deducted!*
- `-pedantic` issues all warnings demanded by strict [ISO C++](https://en.wikipedia.org/wiki/C%2B%2B#Standardization) rules if you want to be *extra* safe
- `-Wextra` enables some extra warnings not turned on by `-Wall`. These include warnings for bad pointer to integer zero comparisons, base class not initialized in copy constructor of derived class, etc.
- `-Wfatal-errors` is similar to `-Wall` but treats an error as fatal and stops before  dumping a long list of errors into the terminal.
- `-fmax-errors=n` tells the compiler to stop after encountering `n` errors in your code.
  Usually you want to see all errors in your code some error messages may get too long for you to read.
- `-Wshadow` warns whenever a local variable or type declaration shadows another variable, parameter, class member, etc.
- `-Wsign-conversion` or `-Wconversion` warn if making unsafe, implicit conversions between signed and unsigned types (i.e. unsigned int + (constant expression) )
- `-O2` optimize code and check for errors in parallel to find potential bugs. ([read more about this here](#more-error-checks))
- `--help` gets help on specific options for the compiler.

### More Error Checks

Sometimes you'll have a program that seems to work but will produce a ton of Valgrind errors.
You'll get some fairly strange errors about invalid reads or confusing c++ library functions.
Ever thought the compiler might be able to help solve these issues before you had to slog through pages of these debugger errors?
 
Introducing the compiler flag `-O2` (that's an O as in Oreo, or Optimize) to the rescue! 
Normally, when you compile c++ programs, you'll ask for all warnings with the `-Wall` flag. 
After the compiler checks for these errors, it will build your program with a ton of fancy optimizations to make your code faster (you'll learn about these more in CSCI 356).
The problem with this is sometimes you'll miss a few potential errors by not including the optimization step.
You can use the `-O2` flag to optimize and error check at the same time.

For example, a student's code seem to be perfectly normal when compiling with
the normal flags.

```shell
# Before, compiled without errors
$ g++  -Wall -std=c++17 main.cpp -o main

# After, we get more helpful warnings about potential bugs
$ g++ -O2 -Wall -std=c++17 main.cpp -o main

In file included from main.cpp:9:
avl/avlbst.h: In member function 'void AVLTree<Key, Value>::balance(AVLNode<Key, Value>*&) [with Key = std::__cxx11::basic_string<char>; Value = int]':
avl/avlbst.h:106:54: error: 'y' may be used uninitialized in this function [-Werror=maybe-uninitialized]
  return static_cast<AVLNode<Key,Value>*>(this->mRight);
                                                      ^
avl/avlbst.h:199:23: note: 'y' was declared here
  AVLNode<Key, Value>* y;
```

You can read more about `-O2` [online](https://www.linuxtopia.org/online_books/an_introduction_to_gcc/gccintro_52.html).

## Cheat Sheet

```shell
# Compile a single program
g++ -Wall -g -std=c++17 test.cpp -o test

# Print all possible warning flags available to compile with
g++ --help=warnings

# Compile program using header files in ./libs
# $(pwd) will expand to your current working directory
g++ -I /$(pwd)/libs -Wall -g -std=c++17 test.cpp -o test

# Compile and optimize in parallel for helpful warnings about potential bugs
g++ -O2 -Wall -std=c++17 main.cpp -o main
```

We compile your homework with a lot of extra flags.
Make sure you compile with the following flags and fix the errors before submitting your homework.
If you want to use a Makefile instead of writing out these flags every time, check out our guide on [Makefiles](/wiki/makefile).

```shell
# use the following warnings to compile your program
-std=c++17 -pedantic -Wall -Wextra -Werror -Wshadow -Wsign-conversion -g

# example compilation
g++ -g -std=c++17 -pedantic -Wall -Wextra -Werror -Wshadow -Wconversion -Wunreachable-code homework_program.cpp -o homework_program
```
