# React Hooks

*React Hooks* are a feature added in React version 16.8 that allow developers to use state in React components without writing a class (i.e. in functional components).

Hooks get their name from how they *hook into* React features like state and lifecycle from functional components.

React has several built-in hooks, but it is also possible to write your own hooks.

## Rules of Hooks

Hooks are JS functions that impose two additional rules:

1. Hooks are only called at the top level. i.e. they are not called inside loops, conditions, or nested functions.

2. Hooks are only called from React function components, or from custom hooks, not from regular JS functions.

There is an [ESLint plugin available](https://www.npmjs.com/package/eslint-plugin-react-hooks) to enforce these rules.

## State Hooks

State can be managed with hooks using the built-in `useState` function.

`import React, { useState } from 'react';`

Here is a basic example of a hook tracking state:

```javascript
function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Counter</button>
    </div>
  );
}
```

In this example, we have an Example component that keeps track of how many time the user has clicked a button.

Note how `useState` returns a pair: the current state value, and a function that allows an update to State. `useState` takes a single argument that provides the initial state value. In contrast to `this.state` in class based components, the state in `useState` does not have to be an object (although it can be that as well).

### Declaring Multiple State Variables

State hooks can be used in multiple instances in a single component.

```javascript
const [age, setAge] = useState(2666);
const [fruit, setFruit] = useState('slime mold');
const [todos, SetTodos] = useState([{ text: 'Learn React Hooks' }]);
```

## Effect Hooks

*Effect Hooks* are for operations that produce side-effects like data fetching, manual DOM manipulation, etc.

Effects can be managed with the built-in `useEffect` function. It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in class based components.

The following could be included in the example from `useState`:

```javascript
useEffect(() => {
  document.title = `You clicked the button ${count} times`;
});
```

By default `useEffect` runs the effect after every render to the DOM. Effects are declared inside a component, so they have access to props and state. Effects may specify how to clean up after returning a function. Like with `setState`, `useEffect` can be used multiple times in a single component.

## Custom Hooks

TODO

## Other Hooks

### `useContext`

### `useReducer`
