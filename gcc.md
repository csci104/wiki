# `gcc` cheatsheet

## Description

GCC (the GNU Compiler Collection) is used to build your C++ executables. It's a powerful tool and you kinda have to use it to test your programs.

## Details

Here are a few of the more useful command options when using `gcc`. These compiler flags may be used in any order you wish. However, by convention, we usually group the list of source files together and the list of options together.

**NOTE:** The most important for normal use on your VM are `-Wall`, `-g`, `--std=c++11`, `-o`.


`-Wall`:  
Show all warnings. Turns on all standard C++ warnings about code that might cause unexpected behavior.

`-g`: 
Provide debugging feature for your program. You will need this when you want to use gdb or valgrind.

`--std=c++<##>`: 
Use version `##` of C++ when compiling. This will allow you to use specific features of that C++ version.

`-o <filename>`: 
Compile and link files into an executable named `filename`. The default filename is `a.out`

`-c`: 
Compile and assemble files but don't link them. This is useful when building large projects to separate file compilation and minimize what is re-compiled.

`--sys-root=<directory>`: 
Use `<directory>` as root directory for headers and libraries

`-I /<absolute-path>`:
Add `absolute-path` to the compiler's search paths

`-Wextra`:
Enables some extra warnings not turned on by `-Wall`. These include: <br> Warnings for bad pointer to integer zero comparisons <br> Base class not initialized in copy constructor of derived class


`-Wfatal-errors`:
Similar to `Wall` but treat an error as fatal and stop before <br> dumping a long list of errors into the terminal.


`-fmax-errors=n`:
Tells compiler to stop after encountering `n` errors in your code. Usually you want to see all errors in your code some error messages may get too long for you to read.


`--help=<class>`:
Get help on specific options for the compiler


## Examples

[//]: <> (A few brief practical examples of using the tool)

```shell
# compile single program like we will compile your homework for grading
g++ -Wall -g --std=c++11 test.cpp -o test


# print all possible warning flags available to compile with
g++ --help=warnings


# compile program using header files in another directory
#   include header files in ./libs directory
#   $(pwd) will expand to your current working directory
g++ -I /$(pwd)/libs -Wall -g -std=c++11 test.cpp -o test
```

## Additional Resources

You can read about all the options for using `gcc` [here](https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html).
