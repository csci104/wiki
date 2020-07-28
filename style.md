# Code Style

The canonical 104 code style is mostly summed up by our `clang-format` [configuration file](https://github.com/csci104/hw/blob/master/.clang-format). 
If you'd like to experiment with it to get a feel for the rules, try setting up `clang-format`, writing some code how you would normally, and formatting it to see what changes.
If you're having issues installing `clang-format` natively, you can always use our [Docker image](https://github.com/csci104/docker) which provides it out of the box.

Please note that **adhering our style guidelines will greatly benefit you**.
Maintaining clean and readable code will help us debug and give feedback on your programs faster.
It also enable us to give you more points back while doing code review.
Plus, using `clang-format`, which we've already set you up with and included [instructions for below](#tooling), you can automatically format your code from the command line.

## Style Guide

Below we'll sum up what we're expecting your code to look like in a few sections.
Note that this is not exhaustive, and that your best bet is to get used to using `clang-format` as described below.

### Indentation

Indentation is four spaces and everything should be indented normally.
The contents of namespaces should not be indented.
Labels, i.e. `public`, `protected`, `private`, and `case`, should not be indented but their contents should be.

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
}

}
```

### Braces

Curly braces are always required around control statement bodies.
In other words, single line `if` statements, `for` loops, etc. are not permitted.
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

The exception to this rule is functions; single-line functions are still fairly clear and so we'll accept them if they are sufficiently short column-wise.
In other words, the following is still allowed:

```c++
// Fine...
bool is_odd(int n) { return n % 2 == 1; }
```

It's worth mentioning that we accept both same-line and K&R open brace positioning, but you'll have to modify the corresponding `.clang-format` options to enable the latter.
Regardless of what you choose, we expect that you use it uniformly in all of your code.
If you have no idea what that means, then don't worry about it, but here's an example:

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

While not syntactical style per se, naming is still super important to readability, and so we ask that you adhere to the following rules:

- Classes should be named using `UpperCamelCase`
- Constant variables should use `UPPER_SNAKE_CASE`
- Methods, variables, and parameters should use `lower_snake_case`
- Class and struct member variables must be either suffixed with `_` or be prefixed `this->` to disambiguate them from local variables.
  Whichever you choose, use it solely and consistently throughout your code.

In general, using short variables such as `n`, `i`, `j`, and `it` for iterators and short functions is fine.
However, you should avoid using nondescriptive variable names if they serve nontrivial uses.

## Tooling

`clang-format` is a utility built on top of the `clang` tooling that, among other things, visually restructures code based on a set of style guidelines.
**`clang-format` does not change what your code does or how it works**, instead managing things like spaces, newlines, brackets, and indentation.

You can invoke `clang-format` from the command line, and it comes installed out of the box on our [Docker container](https://github.com/csci104/docker).
We have already provided you with a set of rules, so running the utility within your homework repository is as easy as:

```
$ clang-format -i directory/file.cpp
``` 

If you want to format multiple files in a directory, you can use wildcard operators:

```
# Format all .cpp files in directory/
$ clang-format -i directory/*.cpp

# Format all .cpp and .h files in directory/ 
$ clang-format -i directory/*.{h,cpp}
```

Lastly, you can use the double asterisk to recursively glob.
This is necessary when you also want to format files in subdirectories.

```
# Format all .cpp and .h files in directory/ and subdirectories
$ clang-format -i directory/**/*.{h,cpp}
```

Please make sure you **format your code before submitting**. 
This ensures that graders can easily read and give you points for your work.
It will also prevent you from losing points for formatting in code review. 

