## React Notes

React is an unopinionated Javascript library for building out the UI of your web app.

### Startup

`create-react-app {app name}` to create an app directory
`npm start` to build and run the app

### JSX

JSX is a preprocessor that adds XML syntax to Javascript. Although it isn't required, it is typically used with React.

### Pattern

```
class {Name} extends React.Component {
  render() {
    return (
      // your HTML with handlebars that accept any Javascript
    );
  }
}

ReactDOM.render(<App />, document.getElementById("app"));

```

### Miscellaneous

For your HTML tags use the phrase `className` instead of `class` since `class` is utilized for the ES6 syntax.

Also JSX is more strict than HTML about requiring closed tags so make sure single tags like an img end with `/>`.

### Redux

Redux is a tool for handling state that pairs well with React.
