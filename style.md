---
layout: asides
toc: true
tasks: false
---

# Code Style

The complete 104 code-style is defined by our `clang-format` [configuration file](https://github.com/csci104/hw/blob/master/.clang-format). 
If you'd like to experiment with it to get a feel for the rules, try setting up `clang-format`, writing some code how you would normally, and formatting it to see what changes.
If you're having issues installing `clang-format` natively, you can always use our [Docker image](https://github.com/csci104/docker), which provides it out of the box.

**Adhering to our style guidelines will greatly benefit you**.
Maintaining clean and readable code will **help us debug and give feedback** on your programs.
It also **enable us to give you more points** back while doing code review.
Formatting your code is as easy as calling `clang-format` from the command line, which we've included [instructions](#clang-format) for below.

## Style Guide

Below we'll sum up what we're expecting your code to look like in a few sections.
Note that this is not exhaustive, and that your best bet is to get used to simply use `clang-format`.

### Indentation

Indentation is **four spaces** and everything should be indented normally.
The contents of namespaces **should not** be indented.
Labels, i.e. `public`, `protected`, `private`, and `case`, **should not** be indented but **their contents should be**.

```c++
// Good
namespace geometry {

class Polygon {
public:
    std::string describe() {
        switch(this->sides) {
        case 3:
            return "triangle";
        case 4:
            return "square";
        default:
            return "polygon";
        }
    }
private:
    int sides;
};

}
```

### Braces

Curly braces are **always** required around control statement bodies.
In other words, **single line `if` statements, `for` loops, etc. are not permitted**.
There should be a space between the keyword and the opening paren.
Additionally, the first statement in curly-braced bodies should start on the line after the opening brace.

```c++
// Good
if (this->sides < 3) {
    throw invalid_argument("too few sides!");
}

// Bad 
if(this->sides) throw invalid_argument("too few sides!");
```

The exception to this rule is functions; **single-line functions are still acceptable if they are sufficiently clear and concise**.
For example, the following is still allowed:

```c++
// Fine...
bool is_odd(int n) { return n % 2 == 1; }
```

It's worth mentioning that we accept both same-line and K&R open brace positioning, but you'll have to modify the corresponding `.clang-format` options to enable the latter.
If you want to do so, there are comments in the `.clang-format` file explaining what to change.
Regardless of what you choose, we expect that you **use it uniformly in all of your code**.

```c++
// Good
bool is_odd(int n) {
    return n % 2 == 1;
}

// Also good, just be consistent
bool is_odd(int n)
{
    return n % 2 == 1;
}
```

### Naming

While not syntactical style per se, naming is still super important to readability.
We ask that you adhere to the following rules:

- Classes should be named using `UpperCamelCase`
- Constant variables should use `UPPER_SNAKE_CASE`
- Methods, variables, and parameters should use `lower_snake_case`
- Class and struct member variables must be either suffixed with `_` or be prefixed `this->` to disambiguate them from local variables.
  Whichever you choose, use it solely and consistently throughout your code.

In general, using short variables such as `n`, `i`, `j`, and `it` for iterators and short functions is fine.
However, you should **avoid using super short variable names if they serve complex uses**.
Names should be self-documenting; **you should be able to glance at variable and immediately infer what it's being used for**.

## Clang Format

`clang-format` is a utility built on top of the `clang` tooling that, among other things, visually restructures code based on a set of style guidelines.
Based on a collection of settings that may be specified in a `.clang-format` file, it manages things like spacing, newlines, brackets, and indentation.
We have already put a `.clang-format` file in your homework repository, so any files in or under the root directory will be formatted our ruleset.

You can invoke `clang-format` from the command line, and it comes installed out of the box on our [Docker container](https://github.com/csci104/docker).
Running the utility within your homework repository is as easy as:

```shell
clang-format -i directory/file.cpp
``` 

If you want to format multiple files in a directory, you can use wildcard operators:

```shell
# Format all .cpp files in directory/
clang-format -i directory/*.cpp

# Format all .cpp and .h files in directory/ 
clang-format -i directory/*.{h,cpp}
```

Lastly, you can use the double asterisk to recursively glob.
This is necessary when you also want to format files in subdirectories.

```shell
# Format all .cpp and .h files in directory/ and subdirectories
clang-format -i directory/**/*.{h,cpp}
```
