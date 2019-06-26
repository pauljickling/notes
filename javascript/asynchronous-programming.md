# Asynchronous Programming

Under an asynchronous programming model multiple steps can happen at once. JavaScript supports several techniques to enable asynchronous programming. You can read more about asynchronous programming [here](http://eloquentjavascript.net/11_async.html)

## Callbacks

Callbacks are when a function takes an extra argument, known as the callback function, which is a function that takes a slightly longer time to complete that the first function depends upon. A `setTimeout` function works this way.

`setTimeout(() => console.log("Tick"), 500);`

This used to be the only way to handle asynchronous programming in JavaScript, and when chaining together callback functions you would end up with code known as *callback hell* that is very hard to read and understand. As a result there was a desire in the JS community to have alternative ways to handle asynchronous programming.

## Promises

A `Promise` is a JavaScript object that resolves to some sort of value. You can assign variables to that `Promise`, and then do something with that value using the `.then` method.

Promises also provide an interface for error handling with the `.reject` and `.catch` methods where `.reject` is for when you want the error to happen, and to stop the chain of asynchronous calls, and `.catch` is for when you want the error to be handled, and for your program to continue uninterrupted.

`Promise.all` is a useful method for resolving an array of Promises.

## Async/Await

`async` functions allow you to write code the way you would write synchronous programming events, more or less. When you declare an `async function` it will rely on an `await` block of code to execute before completing the rest of the code.

```
async foo() {
  let someValue = "";
  await bar() => { // some network thing that modifies someValue }
  return someValue;
}
```
