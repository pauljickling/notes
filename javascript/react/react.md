# React Notes

React is an unopinionated Javascript library for building out the UI of your web app. You can learn more about React [here](https://reactjs.org/docs/getting-started.html).

## Startup

The simplest way to get started using React is to install and use the create-react-app npm package.

`npx create-react-app {app name}` to create an app directory
`npm start` to build and run the app
`npm run build` to create an optimized build of the app in the build folder

## JSX

JSX is a preprocessor that adds XML syntax to Javascript. It is not required, but typically used with React.

## React Flowchart

1. `ReactDOM.render()` is called to render the element that is passed as a parameter
2. Components are called with their declared props
3. Components return their elements
4. ReactDOM updates the actual DOM

## Components

### Component Patterns

#### Functional Pattern:

`const {component} = () => ({JSX rendering});`

#### Class Based Pattern:

```
class {Name} extends React.Component {
  render() {
    return (
      // your HTML with handlebars that accept any Javascript, for example:
      <div>{this.props.exampleComponent}</div>
      <{component name} /> // different component using this same pattern
    );
  }
}

ReactDOM.render(<App />, document.getElementById("app"));

```

### Props

Props is short for object properties. Think of single element objects where the key is defined, and its value is derived from a parent component.

Lets say you have a class based card component. The card consists of an image, and then some text. The text portion might look something like this:

```
<p>Name: {this.props.name}</p>
<p>Role: {this.props.role}</p>
```

Note that for a functional component you would omit `this` during the assignment.

Then when an instance of your card component pops up elsewhere in your app, you would declare the property values this way:

`<Card name="Paul" role="Software Engineer" />`

Props are read only. In this way, it makes the app easier to reason about because data flows in a single direction, and they help make components reusable.

It can be useful to define default properties that are used if no parameters are provided when the component is invoked. For example:

```
Card.defaultProps = {
  name: 'default name'
};
```

React also provides a package that allows for typechecking values passed to props. This package can be used in applications with the following line: `import PropTypes from 'prop-types';`

Here is how it is used:

`Card.propTypes = { name: PropTypes.string }`

Note that in Javascript there is a number type, not an integer. Also the PropTypes module uses `func` instead of function, and `bool` is used instead of boolean.

### Parent Child Component Relationships

When thinking about how your app is structured it is important to think about what information a component is passing from one to the other. These relationships will need to be defined. This relationship gets especially tricky when trying to manage state within nested components.

As a rule, you should avoid creating inheritance in your props. It is better to just create neutral props where you can declare a separate component as its value. Lets say that our card component might have a button component nested inside of it. In the Card component we would write:

`<div className="button>{this.props.button}</div>"`

Then, when calling an instance of the Card component, you could declare the button inside it by writing:

`<Card button= { <Button /> } />`

### Miscellaneous Component Notes

For your HTML tags use the phrase `className` instead of `class` since `class` is utilized for the ES6 syntax. Also JSX is more strict than HTML about requiring closed tags so make sure single tags like an img end with `/>`. Similarly, when leaving comments in your code, your linter is likely to complain unless you leave them this way:

`{/* this is a comment in a JSX file */}`

A component must never modify its own props. Thus, it should follow the patterns associated with composing pure functions.

React also doesn't like to have components with multiple sibling elements. Thus, if you need multiple siblings you will either need to wrap these elements up within a div tag, thus making them child elements of a single div element, or else you will need to break up your component into other components.

Components can be functional or class based. Any component that manages state will need to be class based.

#### Inline Styling

Inline styling for React components is common. Unlike CSS properties, in JSX a camel case format is used since hyphenations are not valid syntax.

`<div style={{fontSize: 22}}>Lorem ipsum</div>`

## State

Providing state to class based components requires several steps:

1. Replacing `this.props.{property name}` with `this.state.{state name}`

2. Add a class constructor that assigns the initial `this.state`:

```
constructor(props) {
  super(props);
  this.state = {
    {state name}: {state value}
  };
}
```

3. Remove the props assignment from the rendering element: `<{Component} />`

4. Create a `componentDidMount()` method that runs after the component output has been rendered to the DOM.

5. Create a `componentWillUnmount()` method that removes a state instance from the lifecycle hook.

6. Create any additional methods needed, for instance perhaps a method that is called from within the `componentDidMount()` method.

Do not modify state directly, instead use `this.setState()`. The `setState()` method takes an anonymous object with the changed state values as its parameter. Also remember that state updates may be asynchronous like everything else in Javascript, so code accordingly.

## Event Handlers

Events work similarly to events that handle DOM elements, but the syntax is slightly different. As an example, here is an example for a button component that does something when clicked:

```
class Button extends React.Component {
  handleButton() {
    {/* do something with the function here /*}
  }
  render() {
    return (
      <button onClick={this.handleButton}>Button</button>
    );
  }
}
```

Another difference from standard Javascript event handlers is that you cannot return `false` to prevent default behavior. Instead you would have to call the `preventDefault()` method on the event.

```
function handleClick(e) {
  e.preventDefault();
}
```

Finally, it is important to remember that in Javascript class methods are not bound by default, therefore you will need to call the `bind()` method so the provided `this` value will pass its context along.

Typically the constructor function will contain a line of code like this for every component method:

`this.{method name} = this.{method name}.bind(this)`
