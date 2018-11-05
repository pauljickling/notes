# Hello World

```
#include <iostream>

using std::cout; using std::endl;

int main() {
  cout << "Hello world" << endl;
}
```

Observations:

1. When including modules from the standard library use angular brackets. For other scripts use double quotation marks. This uses the iostream library which handles input and output. C++ has many libraries that need to be explicitly included that you would get "for free" in many other programming languages.

2. The `using` notation allows us to save us from the more verbose notation in our actual script.

3. We wrap our script up in the main function. Note that the function has a type, in this case int.

4. `cout` is used for output, followed by the print statement, followed by the `endl` keyword.

5. We end on a final note of caution: C++ allows you to do a lot of things you shouldn't do. For example, we could skip the `endl` section of our output statement, but this would result in some undesirable behavior in your terminal when you compiled and ran the resulting program. C++ is very much a language that will provide you with enough rope to hang yourself.
