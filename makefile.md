---
layout: asides
toc: true
tasks: false
---

# Makefile Cheat Sheet

By Leif Wesche

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
- Oftentimes these commands start with `g++`, but they can be anything!
  You can run any command this way.
  
The target dependency format looks like this. 
**Note the tab indent before the commands; these are required!**
  
```makefile
target1: dependency1 dependency2 ...
	command1
	command2
	...
```

**Targets** in a Makefile can be named anything (though as you'll see, certain names can incur certain behavior).
**Dependencies** can either be other targets or file names; if a target depends on another target, it guarantees that target will be run prior, and if a target depends on a file, it will check to see if that file has changed to avoid executing redundantly.
Finally, **commands** can be anything you could run on your command line.

## Simple Example

This simple Makefile compiles a `hello.cpp` program.
It also includes a `clean` target that removes our executable in case we want to clean up the directory.

```makefile
hello: hello.cpp
	g++ -g -Wall hello.cpp -o hello

clean:
	rm -f hello
```

If we're in the same directory as our Makefile, we can run the following to compile `hello.cpp`:

```shell
make  # or make hello
```

And we can run the following to delete the file we just generated during compilation:

```shell
make clean
```

## Multi-file Example

It's important to note a **target can be named after after a file**.
This is most commonly used to:

1. Indicate that our target requires a file that must be compiled by another target.
2. Only run our target when that dependency has changed to avoid doing extra work.

Most of your homeworks will require you to compile multiple files, then link them all at once.
To do this, we'll use the `-c` compiler flag, which compiles without linking.
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

```makefile
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

There are a couple things to note here:

1. We added a dependency on `file1.h` to the `file1.o` target.
   This makes sure that if `file1.h` changes, `file1.o` will be recompiled.
2. Our `clean` target deletes `*.o`. 
   This is simply shorthand for deleting all files that end with `.o`, and is a more convenient way to clean up the objects we created while compiling `program`.
3. The first target is `all`.
   This is simply what the central or default task of a Makefile is customarily called; there's no rule that requires you to do this.
   However, having a separate task in charge of orchestrating the overall compilation process can keep your Makefiles tidy.

## Using Variables

When you start to write more complicated Makefiles, you'll find yourself frequently repeating commands and arguments.
In order to alleviate this, Makefile offers variables.
Here's an example Makefile where we've abstracted most of the `g++` calls into variables.

```makefile
COMPILER=g++
OPTIONS=-g -std=c++17 -pedantic -Wall -Wextra -Werror -Wshadow -Wconversion -Wunreachable-code
COMPILE=$(COMPILER) $(OPTIONS)

program: main.cpp
	$(COMPILE) main.cpp -o program
```

## Build Folder

Another way to keep your build process organized is to use a subdirectory for build artifacts.
This allows you to keep all of the compiled objects out of the way of the files you're trying to edit.
It also improves the ergonomics of `clean`; all you have to do is delete the directory you have all your objects in.

Note that because we still want Make to check file creation and edit times correctly, we also have to change target names.
Using a build directory might make your `Makefile` look something like this:

```makefile
BUILD=build

$(BUILD)/file2.o: file2.cpp
	g++ -g -Wall -c file2.cpp -o $(BUILD)/file2.o

build:
	mkdir -p $(BUILD)
```

## Special Symbols

There are a bunch of fancy things you can do with Make using wildcard and expanded symbols.
Given the following snippet, here are a couple that might come in handy.

```makefile
target1: dependency1 dependency2
	command1
	command2
```

- `$@` expands to the target name, i.e. `target1`
- `$<` expands to the first dependency, i.e. `dependency1`
- `$^` expands to the complete list of dependencies, i.e. `dependency1 dependency2`

You can use the wildcard operator, `%`, to apply a rule to multiple files.
For example, the following rule compiles all `.cpp` files in the directory to correspondingly named `.o` files:

```makefile
%.o: %.cpp
	g++ -Wall -c $< -o $@ 
```

## Cheat Sheet

The following is a Makefile template that uses all of the strategies we've discussed above.
Feel free to use it for your projects, just make sure to modify it to fit our assignment specifications.

```makefile
COMPILER=g++
OPTIONS=-g -std=c++17 -pedantic -Wall -Wextra -Werror -Wshadow -Wconversion -Wunreachable-code
COMPILE=$(COMPILER) $(OPTIONS)
BUILD=build

# Compile main by default
all: program

# $(BUILD)/*.o expands to all .o files in the $(BUILD) directory
# In this case, we'll get $(BUILD)/file1.o $(BUILD)/file2.o
program: main.cpp $(BUILD)/file1.o $(BUILD)/file2.o
	$(COMPILE) $< $(BUILD)/*.o -o $@

$(BUILD)/file1.o: file1.cpp file1.h build
	$(COMPILE) -c $< -o $@

$(BUILD)/file2.o: file2.cpp build
	$(COMPILE) -c $< -o $@

# Make the build directory if it doesn't exist
build:
	mkdir -p $(BUILD)

# Delete the build directory and program
clean:
	rm -rf $(BUILD) program

# These rules do not correspond to a specific file
.PHONY: build clean
```
