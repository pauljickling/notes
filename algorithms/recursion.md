## Recursion

In computer science recursion is a way to solve a problem that relies on smaller instances of the same problem. Most programming languages allow functions to call themselves, and by doing this it makes recursive solutions possible. Recursive solutions are often less efficient than running for or while loops to solve a problem because of the memory demands they place on the call stack, however they can be easier to reason about, so for complex pieces of code they can be desirable for that purpose.

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

### Divide & Conquer

Divide & conquer is a common technique for approaching problems that are solved recursively. Divide & Conquer consists of two steps:

1. Establish the base case.
2. Divide or decrease the problem until it becomes the base case.

By reasoning about problems this way it can be easier to think through the solution. The base case is going to be the smallest possible condition of a problem to solve, and then you use the recursive base to reduce the conditions of the problem down to that base case. For example, if you are trying to perform some sort of operation for an array, the simplest thing to solve would be an empty array or an array with a single item, so the goal of the recursive case should be to reduce the length of the array.
