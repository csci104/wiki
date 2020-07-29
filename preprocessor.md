---
layout: asides
toc: true
tasks: false
---

# Preprocessor Cheat Sheet

By Anthony Wiencko

The lines that start with `#` in your code are preprocessor directives.
They run as the first step in the compilation process before your code is actually compiled.

## Include

Include takes the entire file specified in the brackets or apostrophes and literally inserts it into your code where the include was.
Any preprocessor lines in the inserted code will also be run when the proprocessor gets to them.
This means you have to be careful where you include files to avoid circular dependancy and multiple declarations which can both cause compile errors.

### #include \<_file_>

With angled brackets, the preprocessor searches for the file in the C++ standard library `PATH`, as defined by your installation of C++.
For example, on macOS, your standard C++ library `PATH` might be

```
/Library/Developer/CommandLineTools/usr/include/c++/v1
```

So if the path of file you'd like to include from a directory on your C++ `PATH` is `string`, the first place it would look is the directory above, and it might find:

```
/Library/Developer/CommandLineTools/usr/include/c++/v1/string
```

In general, this is the way you should `include` standard library headers, and while we mention the `PATH` several times, you most likely won't need to fiddle with it too much.

### #include "_file_"

With quotation marks, the preprocessor first searches for the file in the same directory as the `.cpp` or `.h` file thats calling `#include`.
For example, imaging you're working on project `hw1`:

```
/home
  /cs104
    /hw-student
      /hw1
        /file1.cpp
        /file2.h
```

If you want to include `file2.h`, all you would do is add `#include "file2.h"`.
If the file is inside quotations, it's first searched for from wherever the `#include` is, i.e. `hw1/`.

However, if `file2.h` had a different path and was not adjacent to `file1.cpp`, you might do something different:

- `/home/cs104/hw-student/file2.h` would require `#include "../file2.h"`
- `/home/cs104/hw-student/hw2/file2.h` would require `#include "../hw2/file2.h"`
- `/home/cs104/hw-student/hw1/extra/file2.h` would require `#include "extra/file2.h"`

If the file you are trying to include does not exist in your local folder the preprocessor then treats your `#include "file"` line like a `#include <file>` line, searching the C++ library files.
However, you should not use `#include "file"` for library files to make your code more understandable.

## Custom Include Path

If you use the compile argument `-Iinclude_path` with `g++`, you can specify additional folders for both `#include "file"` and `#include <file>` to search for `file`
Note that `include_path` must be the absolute path of the folder, meaning it starts from your computer's root directory, `/`.
If you do use this, `#include "file"` will still only check the additional folder after it checks the local directory as explained above.

## Common Include Errors

While it might seem that a bug is coming from faulty code, in some cases it can be the result of incorrect or circular preprocessing.
Here are a couple common cases of this.

### Inclusion The Same File Multiple Times

If you include a file multiple times in one file, it can cause compilation errors.
For instance, if you declare a class in a file and include it twice, your code won't compile since you can't declare the same class several times.

The same problem can occur if you include `file1.h` in `file2.h` and inlcude both `file1.h` and `file2.h` in another file like `file3.h`.
In this case, `file1.h` will be included in `file3.h` twice potentially causing compile errors.

### Circular File Dependency

If `file1.h` includes `file2.h` and `file2.h` includes `file1.h`, they will keep including each other until your compiler stops working. 

### How to Avoid These Issues

Only include files when you know you need them.
In `.h` files, only include what you absolutely need for class and function definitions to work.
Use corresponding `file.cpp` files to include everything else you might need.
Additionally, avoid circular dependancy by aiming for a tree-like structure; if you find yourself needing circular dependencies, you might need to separate out parts of your definitions.

## Definitions

The `#define` directive can be used as `#define identifier replacement` or just `#define identifier`.

### #define _identifier_ _replacement_

Replaces every instance of *identifier* with replacement for the rest of the file.
For instance `#define MAX 100` will replace every instance of `MAX` with `100`. Remember, since `#include` literally copies a file over, if `#define MAX 100` is before an `#include` statement in the code, the preprocessor will replace every instance of `MAX` in the included file with `100`.

### #undef _identifier_

Removes the current definition of `identifier` from the preprocessor.
This also stops the `identifier` from being replaced by a `replacement` if one was defined.

### #define _identifier_

Declares the identifier for the scope of the preprocessor.
This is a necessary component of header guards.

### #ifndef _identifier_

If the `identifier` is already defined, the code block below the directive will not be compiled. 

### #endif

Ends the code block that starts with `#ifndef identifier`.
If a code block is skipped over because it fails the `#ifndef identifier` check, the code below the `#endif` directive will be included in the compile step.

## Guarding Header Files

In order to prevent a file from being defined multiple times if it `#include`-ed more than once, we can put together what we've learned to write what's called a "header guard".
Traditionally, we use the name of the file replacing a `.` with an `_`, but really the identifier set by the `#define` just has to be unique.
For a header `filename.h`, we might do the following:

```c++
#ifndef FILENAME_H
#define FILENAME_H

// ... program code ...

#endif
```

Always make sure to guard your `.h` files.
Even if you are sure you don't include something twice, this will ensure that any additions you make to the code will still be safe.
That said, don't worry about writing guards for normal `.cpp` files, as they shouldn't be included anywhere!

## More Preprocessor.

The preprocessor also has some useful features not discussed on this page. 
You can read more in the C++ standard, but MSDN also has [great docs](https://docs.microsoft.com/en-us/cpp/preprocessor/preprocessor-directives?view=vs-2019).
As a sidenote, we don't use `#pragma` in this class because it's not officially in the C++ specification at the time of writing.
