---
layout: asides
toc: true
tasks: false
---

# GDB Cheat Sheet

By Spencer Davis

GDB is a debugging program that will save your life in this class and beyond.
This file aims to make it more accessible for beginner's use.

## Why Use GDB?

There are essentially two methods of debugging you'll use in this class: GDB and couts.
If you're anything like me, the first time you run your program, it's probably going to segfault.
If and when it does that, you're gonna have to backtrace it with GDB anyway.
From there, you may as well test your variable values from that line instead of placing the couts, recompiling, and rerunning.
Obviously this doesn't replace couts entirely, but it's a tool you need to be comfortable using.

## GDB Commands

Most commands have 2 ways of running them.
These will be shown by the slash. I.e. `run/r` means you can use either `run` or `r`.

- `run/r [arguments]` runs the program with the given arguments.
- `break/b [file.cpp:line number]` puts a breakpoint at the given line number in the given file.
  Note that if you only have one file (i.e., for `the_rani`), just break `[line number]` will suffice.
- `break/b [function name]` places a breakpoint at the start of the given function.
- `clear [file.cpp:line number]` clears a breakpoint at the given line number in the given file.
  Note that if you only have one file (i.e., for `the_rani`), just clear `[line number]` will suffice.
- `clear [function name]` removes the breakpoint on the given function.
  Note: function breakpoints will not work on functions that take strings as arguments (it's complicated) on your course VM due to an incompatibility between GCC and GDB.
  However, this will work properly on newer systems.
- `layout next` From the begining of GDB, entering 'layout next' once the program is running will show you source code around your current location in the program. 
  This view can be helpful to those who are new to gdb, and especially helpful when working with source code you are not farmiliar with. 
  Repeating 'layout next' shows your program in assembly language.
- `layout prev` Takes you back to the previous layout mode. 
- `bt` shows the function call stack, every function that you've run through since the line you've arrived at.
- `frame [number]` goes to the selected frame in the call stack.
- `continue/c` continues the program after being stopped by a breakpoint.
- `print/p [variable]` prints out the variable value.
  If you pass it a class/struct instance, it will print all the data members in the class.
- `display/d [variable]` is like print, but reprints the information after every instruction.
- `next/n` executes the current source line and moves it to the next one.
  Will skip over any function calls.
  If you were to have the line `x = getValue(y)` and used `n`, you would go to the next line in the current function call, ignoring what happens in `getValue(y)`.
  Useful for when you're testing out a function and you KNOW your helper functions work.
- `step/s` executes the current source line and moves it to the next one.
  Will step INTO any function calls.
  If you were to have the line `x = getValue(y)` and used `s`, you would go into the `getValue(y)` function.
- `finish/f` executes the rest of the current function.
  Will step OUT of the current function.
- `l/list` prints the area around the current line in the current source file.
- `where` displays the current line and the function stack of calls that got you there.
- `quit` exits GDB.

## Basic Examplee

```
# start GDB using an executable you compiled
#   use the run shorthand r to run the program with commandline arguments
#   use the backtrace shorthand bt to see where the segfault occured
#   use the breakpoint shorthand b to set a breakpoint at line 132
#   use the run command again to run the program again

$ gdb the_rani
(gdb) r input.txt output.txt

Program received signal SIGSEGV, Segmentation fault.
(gdb) bt
#0  0x000 in TheRani::some_function (this=xxxx, x=2, n=0, subject_history=xxxxx,
    temp_history=xxx, query_count=2) at the_rani.cpp:194
#1  0x000 in TheRani::execute (this=xxxx, line=...) at the_rani.cpp:132
#2  0x000 in TheRani::main (this=xxxxx) at the_rani.cpp:60
#3  0x000 in main (argc=3, argv=xxxxxx) at the_rani.cpp:281
(gdb) b 132
(gdb) r input.txt output.txt
The program being debugged has been started already.
Start it from the beginning? (y or n) y
```

## Advanced Example

```
# this is #5 from Lab 2
#   break right before the first control flow statement
#   continue until you get to the bugged output (i.e. the fourth line of output)
#   step through the code, making sure everything is working logically
#   print out any variables that are relevant to the current line
#   on line 115, despite protector->power > invader->power, protectorLost is true
#   this shows what the error was, a mistake in < > signs in control flow

$ gdb game_student2
(gdb) break 110
Breakpoint 1 at 0x401683: file game_of_pointers_student2.cpp, line 110.
(gdb) r input3.txt output3-stu2.txt
Starting program: /home/cs104/labs/lab2/game_student2 input3.txt output3-stu2.txt

Breakpoint 1, Skirmish (protectors=0x61a0a0, invaders=0x61a220, n=2, m=3, 
    skirmishLine=1, numReserves=@0x7fffffffd970: 1, outputFile=...)
    at game_of_pointers_student2.cpp:110
110	    bool protectorLost = false;
(gdb) c
Continuing.

Breakpoint 1, Skirmish (protectors=0x61a0a0, invaders=0x61a220, n=2, m=3, 
    skirmishLine=1, numReserves=@0x7fffffffd970: 1, outputFile=...)
    at game_of_pointers_student2.cpp:110
110	    bool protectorLost = false;
(gdb) c
Continuing.

Breakpoint 1, Skirmish (protectors=0x61a0a0, invaders=0x61a220, n=2, m=3, 
    skirmishLine=1, numReserves=@0x7fffffffd970: 0, outputFile=...)
    at game_of_pointers_student2.cpp:110
110	    bool protectorLost = false;
(gdb) c
Continuing.

Breakpoint 1, Skirmish (protectors=0x61a0a0, invaders=0x61a220, n=2, m=3, 
    skirmishLine=1, numReserves=@0x7fffffffd970: 0, outputFile=...)
    at game_of_pointers_student2.cpp:110
110	    bool protectorLost = false;
(gdb) n
111	    if (invader->weapon == protector->weapon)
(gdb) print invader->weapon
$3 = "axe"
(gdb) print protector->weapon
$4 = "axe"
(gdb) n
113	      if (invader->power < protector->power)
(gdb) print invader->power
$5 = 30
(gdb) print protector->power
$6 = 100
(gdb) n
115	        protectorLost = true;
(gdb) c
```
