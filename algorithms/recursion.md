## Recursion

In computer science recursion is a way to solve a problem that relies on smaller instances of the same problem. Most programming langauges allow functions to call themselves, and by doing this it makes recursive solutions possible. Recursive solutions are often less efficient than running for or while loops to solve a problem because of the memory demands they place on the call stack, however they can be easier to reason about, so for complex pieces of code they can be desirable for that purpose.

### Structure of the Recursive Function

A recursive function has two parts: the *base case*, and the *recursive case*. The base case is the part of the function that does not call itself, and similarly the recursive case is the part of the function that does. The base case checks for solutions, and/or handles edge cases that the recursive case isn't equipped to handle.

### Example: a factorial function

Here's an example of a factorial function written in Javascript using recursion.

```
function factorial(x) {
  if (x === 1) {
    return 1;
  } else {
    return x * factorial(x - 1);
  }
}
```

In this example, the first part of the if statement is the base case, and the else statement is the recursive case.
