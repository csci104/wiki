#***GDB CHEAT SHEET***
By Spencer Davis

GDB is a debugging program that will save your life in this class and beyond.
This file aims to make it more accessible for beginner's use.

##***WHY USE GDB?***
There are essentially two methods of debugging you'll use in this class: GDB and couts.
If you're anything like me, the first time you run your program, it's probably going to segfault. If and when it does that, you're gonna have to backtrace it with GDB anyway. From there, you may as well test your variable values from that line instead of placing the couts, recompiling, and rerunning. Obviously this doesn't replace couts entirely, but it's a tool you need to be comfortable using.

##***GDB COMMANDS***
Most commands have 2 ways of running them. These will be shown by the slash. Ie run/r means you can use either run or r. It doesn't make a difference.
| **run/r [arguments]** | runs the program with the given arguments. |
| **break/b [file.cpp:line number]** | puts a breakpoint at the given line number in the given file. Note that if you only have one file (ie, for the rani), just break [line number] will suffice. |
| **break/b [function name]** | Places a breakpoint at the start of the given function. |
| **clear [file.cpp:line number]** | clears a breakpoint at the given line number in the given file. Note that if you only have one file (ie, for the rani), just clear [line number] will suffice. |
| **clear [function name]** | Removes the breakpoint on the given function. Note: function breakpoints will not work on functions that take strings as arguments (it's complicated) on your course VM due to an incompatibility between GCC and GDB. However, this will work properly on newer systems. |
| **bt** | shows the function call stack, every function that you've run through since the line you've arrived at |
| **frame [number]** | goes to the selected frame in the call stack |
| **continue/c** | continues the program after being stopped by a breakpoint |
| **print/p [variable]** | prints out the variable value. If you pass it a class/struct instance, it will print all the data members in the class. |
| **display/d [variable]** | like print, but reprints the information after every instruction |
| **next/n** | Executes the current source line and moves it to the next one. Will skip over any function calls. |
	| If you were to have the line 'x = getValue(y)' and used n, you would go to the next line in the current function call, ignoring what happens in getValue(). 
	|Useful for when you're testing out a function and you KNOW your helper functions work
| **step/s** | Executes the current source line and moves it to the next one. Will step INTO any function calls. |
	| If you were to have the line 'x = getValue(y)' and used s, you would go into the getValue() function. |
| **finish/f** | Executes the rest of the current function. Will step OUT of the current function. |
| **l/list** | Prints the area around the current line in the current source file |
| **where** | Display the current line and the function stack of calls that got you there |
| **quit** | Exits GDB. |