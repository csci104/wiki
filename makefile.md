# Makefile Cheat Sheet

By Leif Wesche

## Why use a Makefile?

Makefiles are tools that help us compile and maintain code locally.
Make is a program that comes with the GNU compiler.
Makefiles will save you lots of time typing and clicking. 
You are required to write them for your homework. 

## How does it work?
 
- When you type `make` or `make [target]`, the Make will look through your current directory for a Makefile.
  This file must be called `makefile` or `Makefile`.
- Make will then look for the corresponding target in the makefile.
  If you don't provide a target, Make will just run the first target it finds.
- If the target is found, the target's dependencies will be run as needed, then the target commands will be run. 
- Typically these commands look something like `g++ ...`, but they don't have to be!
  You can run any command this way.
- The target dependency format looks like this. 
  **Note the tab indent before the commands; these are required!**
  
```
target1: dependency1 dependency2 ...
	command1
	command2
	...
```

- The target of a Makefile can be named anything.
- Dependencies can either be other targets or file names; if a target depends on another target, it guarantees that target will be run prior, and if a target depends on a file, it will check to see if that file has changed to avoid executing redundantly.
- Commands can be anything you could run on your command line.

## Simple Example

This simple Makefile compiles a `hello.cpp` program.
It also includes a `clean` target that removes our executable in case we want to clean up the directory.

```
hello: hello.cpp
	g++ -g -Wall hello.cpp -o hello

clean:
	rm -f hello
```

If we're in the same directory as our Makefile, we can run the following to compile `hello.cpp`:

```
$ make  # or make hello
```

And we can run the following to delete the file we just generated during compilation:

```
$ make clean
```

## Multi-file Example

It's important to note a **target can be named after after a file**.
This is most commonly used to:

1. Indicate that our target requires a file that must be compiled by another target.
2. Only run our target when that dependency has changed to avoid doing extra work.

Most of your homeworks will require you to compile multiple files, then link them all at once.
To do this, we will use an `all` target and the `-c` compiler flag, which compiles without linking.
Note that all the files we compile with `-c` have target names that correspond to the object files we're expecting out.

First, take a look at the imaginary file tree we're basing this Makefile off of:

```
program/
  main.cpp
  file1.cpp
  file1.h
  file2.cpp
```

And now, the Makefile:

```
all: program

program: main.cpp file1.o file2.o
	g++ -g -Wall main.cpp file1.o file2.o -o program

file1.o: file1.cpp file1.h
	g++ -g -Wall -c file1.cpp -o file1.o

file2.o: file2.cpp
	g++ -g -Wall -c file2.cpp -o file2.o

clean:
	rm -f *.o program
```

You might notice that we added a dependency on `file1.h` to the `file1.o` target.
This makes sure that if `file1.h` changes, `file1.o` will be recompiled.
You also might notice that our clean deletes `*.o`. 
This is simply shorthand for deleting all files that end with `.o`, and is a more convenient way to clean up the objects we created while compiling `program`.
