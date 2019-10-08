# Valgrind Cheat Sheet

By Cameron Durham

Valgrind provides a suite of debugging tools to increase the perforance and fix problems with your programs.
Memcheck is one of the more commonly used tools.
This tool helps find memory-related errors that might cause your program to crash or leak memory.

## Using Valgrind

Make sure you compile your program with `-g` to generate debugging symbols.
Both GDB and Valgrind use these symbols to show line numbers in error messages.
For more on compilation, see the [gcc](gcc.md) wiki.
To compile with debugging symbols:

```shell
g++ -Wall -std=c++11 -g program.cpp -o program
```

To run Valgrind on your program:

```shell
valgrind --tool=memcheck --leak-check=yes ./program
```

To run with more verbose, sometimes helpful error messages:

```shell
valgrind --tool=memcheck --leak-check=full ./program
```

## Common Errors

Valgrind will tell you about possible errors in your program that may cause it to crash or have unexpected output.
Here are a few examples of common errors:

### X bytes in Y blocks are definitely lost

This error is a memory leak that will show up in your [Heap and Leak Summary](#heap-and-leak-summary).
You have leaked memory and this error points you to where that memory was allocated.
Error example:

```txt
8 bytes in 1 blocks are definitely lost in loss record 1 of 7
   at 0x489C344: operator new[](unsigned long) (vg_replace_malloc.c:433)
   by 0x1091CB: main (valgrind-test.cpp:5)
```

Here is the code that caused this error.
The line number points to the commented line where the lost memory was allocated.

```cpp
int ** matrix = new int*[2];
matrix[0] = new int[2];  // error points where memory was allocated
delete [] matrix;
```

### Conditional jump or move depends on uninitialized value(s)

This error is caused if you forget to initialize variables before using or accessing them.
Error example:

```txt
Conditional jump or move depends on uninitialised value(s)
   at 0x1091D1: main (valgrind-test.cpp:9)
```

All the examples below would have caused this error:

```cpp
// Example 1
int j;
j += 6; // error

// Example 2
int s;
if (s == 66) { // error
  printf("Hello there, General Kenobi!");
}

// Example 3
int i;
while (i < 104) { // error
  printf("I'm still working on fixing Valgrind errors!\n");
}
```

### Invalid read/write of size X

This error happens if you deleted memory and tried to access it in some way.
Valgrind will tell you where the read or write occurred as well as where the memory was originally allocated.
Unfortunately, Valgrind doesn't tell you where the delete occurred.
Error example:

```txt
Invalid read of size 8
   at 0x10922C: main (valgrind-test.cpp:17)

Block was alloc'd at
   by 0x1091D9: main (valgrind-test.cpp:7)

Invalid write of size 4
   at 0x10922F: main (valgrind-test.cpp:17)
Address 0x0 is not stack'd, malloc'd or (recently) free'd
```

This was caused by the following code:

```cpp
int * matrix = new int[2];
delete [] matrix;
matrix[1] = 2;
```

## Heap and Leak Summary

The `MemCheck` tool in Valgrind runs by default, so you don't have to specify `--tool=memcheck` when running Valgrind.
It will help you determine if your program is leaking memory and provide error messages where the program thinks problem is occurring.

You will see two summaries at the end of Valgrind's output: the heap summary and the leak summary.
The heap summary tells you how many memory allocations occurred (how many times `new` was called directory or indirectly in your program) and how many bytes of memory were lost.
Here's some example output:

```txt
==29== HEAP SUMMARY:
==29==     in use at exit: 74,043 bytes in 11 blocks
==29==   total heap usage: 13 allocs, 2 frees, 74,115 bytes allocated
==29==
==29== LEAK SUMMARY:
==29==    definitely lost: 16 bytes in 2 blocks
==29==    indirectly lost: 0 bytes in 0 blocks
==29==      possibly lost: 0 bytes in 0 blocks
==29==    still reachable: 0 bytes in 0 blocks
==29==         suppressed: 74,027 bytes in 9 blocks
```

For an example of this, read through the errors above.
Here is what the two most important parts of the summary mean:

- **definitely lost**: your program is leaking memory and you need to fix it!
- **indirectly lost**: your program may have crashed and couldn't clean up memory.
- possibly lost: your program is leaking memory unless you're doing odd things with pointers.

**Extension**: suppressed memory is memory still allocated when Valgrind exits that we tell Valgrind to ignore via a configuration file in your virtual machine.
This memory is usually being used by either the system's dynamic library loader or parts of the standard library that use custom allocators and deallocators.

## Additional Resources

You can read more about Valgrind error messages in [the docs](http://valgrind.org/docs/manual/mc-manual.html#mc-manual.errormsgs).
Move information on Valgrind can be found [on their website](http://valgrind.org) if you're interested.
