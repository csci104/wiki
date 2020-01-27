# GDB Cheat Sheet

By Noah Kim and Leif Wesche 

## Why use a Makefile?

Makefiles are tools that help us compile and maintain code locally. Make is a program that comes with the GNU compiler.
Makefiles will save you lots of time typing and clicking. 
You are required to write them for your homework. 

## How does it work?
 
- When you type `make` or `make [target]`, your computer knows to look through the current directory for a file called `makefile` or `Makefile` (it must be named one of these).
- The program will look for the corresponding target in the makefile, or if you dont provide a target it will just run the first one (by convention called "all").
- When the target is found, the dependencies will be run as needed, then the commands will run. 
- Typically these commands look something like "g++ -g ...", but they don't have to be! You can run any command this way.
- The target dependency format looks like this. (Note the tab indent before the commands - these are required!!!)
  
```
target: dependencies
	command_1
	command_2
	...
```

## Simple Example

This simple makefile will compile a "hello" program, and includes a "clean" taget that will remove all object files (using the wildcard character `*`) and our executable. 

```
hello: hello.cpp
	g++ -g -Wall hello.cpp -o hello

clean:
	rm -f *.o hello
```

## Multi File Template

Most of your homeworks will require you to compile multiple files, then link them all at once. To do this, we will use an `all` target and the `-c` compiler flag/option.
Here is a template that might be useful:

```
all: program

program: main.cpp file1.o file2.o
	g++ -g -Wall main.cpp file1.o file2.o -o program

file1.o: file1.cpp file1.h
	g++ -g -Wall -c file1.cpp -o file1.o

file2.o: file2.cpp
	g++ -g -Wall -c file2.cpp -o file2.o

clean:
	rm -f *.o hello
```
