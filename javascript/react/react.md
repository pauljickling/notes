## React Notes

React is an unopinionated Javascript library for building out the UI of your web app.

### Startup

`create-react-app {app name}` to create an app directory
`npm start` to build and run the app
`npm run build` to create an optimized build of the app in the build folder

### JSX

JSX is a preprocessor that adds XML syntax to Javascript. Although it isn't required, it is typically used with React.

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

Lets say you have some card component. The card consists of an image, and then some text. The text portion might look something like this:

```
<p>Name: {this.props.name}</p>
<p>Role: {this.props.role}</p>
```

Then when your card component is used elsewhere in your app, you would invoke like so:
`<Card name="Paul" role="Software Engineer" />`

Props are read only. In this way, it makes the app easier to reason about because data flows in a single direction, and they help make components reusable.

### Parent Child Component Relationships

When thinking about how your app is structured it is important to think about what information a component is passing from one to the other. These relationships will need to be defined.

### Miscellaneous Component Notes

For your HTML tags use the phrase `className` instead of `class` since `class` is utilized for the ES6 syntax.

Also JSX is more strict than HTML about requiring closed tags so make sure single tags like an img end with `/>`.

Similarly, when leaving comments in your code, it is best to include them this way:

`{/* this is a comment in a JSX file */}`

Otherwise your linter is likely to complain.

### Props

When a component is rendered, it can access its props using `this.props`. Prop values can be defined as a parameter in the ReactDOM.render method.

### Redux

Redux is a tool for handling state that pairs well with React.
