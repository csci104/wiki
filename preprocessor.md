# Preprocessor Cheat Sheet
By: Anthony Wiencko

The lines that start with **#** in your code are preprocessor directives. They run as the first step in the compilation process before your code is actually compiled. Common directives are:

 - [`#include`](#include) - [`#include <file>`](#include-file) or [`#include "file"`](#include-file-1)
 - [`#define`](#define)
 - [`#ifndef`](#ifndef-identifier)
 - [`#endif`](#endif)

Other links:

 - [Custom Include Path](#custom-include-path)
 - [Common Include Errors](#common-include-errors)
 - [Header guards](#header-guards)

# Include
Include takes the entire file specified and literally inserts it into your code where the include was. Any preprocessor lines in the inserted code will also be run when the proprocessor gets to them. This means you have to be careful where you include files to avoid circular dependancy and multiple declarations which can both cause compile errors.


### #include \<file>

With angled brackets, the preprocessor searches for the file in the C++ standard library PATH, as defined by your installation of C++. For example, on Mac, your standard C++ library path might be

    /Library/Developer/CommandLineTools/usr/include/c++/v1

So if the relative path of file is "String" , the absolute path would be 

    /Library/Developer/CommandLineTools/usr/include/c++/v1/String

This is the way you should include C++ standard library files like \<string>, \<vector>, \<iostream>, ect.

### #include "file"

With quotation marks, the preprocessor first searches for the file in the same directory as the .cpp or .h file thats calling #include. For example, if you are working on file1.cpp with the path

`/home/cs104/hw-student/hw1/src/file1.cpp` 

and you want to include file2.h with the path

   `/home/cs104/hw-student/hw1/src/file2.h`

you would `#include "file2.h"`.

The `file` path is relative to the path of the file calling the `#include`. If file1.cpp stayed in

`/home/cs104/hw-student/hw1/src/file1.cpp` 

but file2.h had a different path, like this:

`/home/cs104/hw-student/hw1/file2.h` you would `#include "../file2.h"`
or 
`/home/cs104/hw-student/hw1/other/file2.h` you would `#include "../other/file2.h"`
or
`/home/cs104/hw-student/hw1/src/src2/file2.h` you would `#include "src2/file2.h"`

If the file you are trying to include does not exist in your local folder the preprocessor then treats your `#include "file"` line like a `#include <file>` line, searching the C++ library files. However, you should not use `#include "file"` for library files to make your code more understandable.

## Custom Include PATH
If you use the compile argument `-I/(include_path)` for g++, you can specify additional folders for both `#include "file2.h"` and `#include "file"` to search for `file`.

include_path is the absolute path of the folder, meaning it starts from your computer's bottommost folder.

For `#include "file"`, it only checks the additional folder after it checks the local directory as explained above.

## Common Include Errors

#### Inclusion The Same File Multiple Times

If you include a file multiple times in one file, it can cause compilation errors. For instance, if you declare a class in a file and include it twice, your code won't compile since you can't declare the same class several times.

The same problem can occur if you include `file1.h` in `file2.h` and inlcude both `file1.h` and `file2.h` in another file like `file3.h`. In this case, `file1.h` will be included in `file3.h` twice potentially causing compile errors.

#### Circular File Dependency

If `file1.h` includes `file2.h` and `file2.h` includes `file1.h`, they will keep including each other until your compiler stops working. 

#### How to Avoid These Issues

Only include files when you know you need them. In `.h` files, only include what you absolutely need for the code to work. In `file.cpp` files, only include library headers and necessary `.h` files.

Use `.h` files when possible. Always use [header guards](#header-guards) for `.h` files.

Avoid circular dependancy by keeping structure in mind when designing an inheritance heirarchy.



# #Header Guards

## #Define
The `#Define` directive can be used as `#Define *identifier* *replacement*` or just `#Define *identifier*`.

#### #Define *identifier replacement*

Replaces every instance of *identifier* with replacement for the rest of the file. For instance `#Define MAX 100` will replace every instance of `MAX` with `100`. Remember, since `#Include` literally copies a file over, if `#Define MAX 100` is before an `#Include` statement in the code, the preprocessor will replace every instance of `MAX` in the included file with `100`.

#### #Undef *identifier*
Removes the current definition of *identifier* from the preprocessor. This also stops the *identifier* from being replaced by a *replacement* if one was defined.

#### #Define *identifier*

Declares the identifier for the scope of the preprocessor. This is a necessary component of header guards.


## #Ifndef *identifier*

If the *identifier* is already defined, the code block below the directive will not be compiled. 

## #Endif

Ends the code block that starts with `#Ifndef identifier`. If a code block is skipped over because it fails the `#Ifndef identifier` check, the code below the `#Endif` directive will be included in the compile step.

## Guarding Header Files

By using  

`#Ifndef headername_h`
`#Define headername_h`

`... program code ...`

`#Endif`

in your header files, you can make sure that even if a file is included more than once, it will only be added to the file a single time. You should always use a header guard for your `.h` files.


