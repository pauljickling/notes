## React Notes

React is an unopinionated Javascript library for building out the UI of your web app.

### Startup

The simplest way to get started using React is to install and use the create-react-app npm package.

`create-react-app {app name}` to create an app directory
`npm start` to build and run the app
`npm run build` to create an optimized build of the app in the build folder

### JSX

JSX is a preprocessor that adds XML syntax to Javascript. It is not required, but typically used with React.

### Component Pattern

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

Lets say you have some card component. The card consists of an image, and then some text. The text portion might look something like this:

```
<p>Name: {this.props.name}</p>
<p>Role: {this.props.role}</p>
```

Then when an instance of your card component pops up elsewhere in your app, you would declare the property values this way:

`<Card name="Paul" role="Software Engineer" />`

Props are read only. In this way, it makes the app easier to reason about because data flows in a single direction, and they help make components reusable.

### Parent Child Component Relationships

When thinking about how your app is structured it is important to think about what information a component is passing from one to the other. These relationships will need to be defined. This relationship gets especially tricky when trying to manage state within nested components.

### Event Handlers

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

### React Flowchart

1. `ReactDOM.render()` is called to render the element that is passed as a parameter
2. Components are called with their declared props
3. Components return their elements
4. ReactDOM updates the actual DOM

### Miscellaneous Component Notes

For your HTML tags use the phrase `className` instead of `class` since `class` is utilized for the ES6 syntax. Also JSX is more strict than HTML about requiring closed tags so make sure single tags like an img end with `/>`. Similarly, when leaving comments in your code, your linter is likely to complain unless you leave them this way:

`{/* this is a comment in a JSX file */}`

A component must never modify its own props. Thus, it should follow the patterns associated with composing pure functions.

### Redux

Redux is a tool for handling state that pairs well with React.
