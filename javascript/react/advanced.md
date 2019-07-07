# Advanced Concepts

## Context

*Context* is a global state in React. Context is useful in instances where there is data you need to pass to lots of different components. An example is language selection. In that case, we shouldn't care where the language selector component is within the tree of components, what's important is that there is a simple way to update the text content of all the components at once without a very complicated scheme of passing props throughout the tree. Using context to manage data does come at the cost of making your components slightly less flexible and reusable.

### Creating Context

`const languageContext = React.createContext("EN");`

When creating a Context object you supply it with a default parameter value.

### Providing Context

```
<languageContext.Provider value={"IT"}>
  <someComponent />
</languageContext.Provider>
```

The Context object has a `.Provider` method that allows React components to subscribe to context changes. Every component that consumes a provider will re-render whenever a context value is changed, and will not need to explicitly pass props to its children.

### Consuming Context

```
<languageContext.Consumer>
  {value => /* render something based on the context value */}
</languageContext.Consumer>
```

Context consumers require a function as a child. They receive the current context value, and return a React node with it.

### Context Type

```
class SomeComponent extends React.Component {
  const language = this.context;
  const english = "hello";
  const spanish = "hola";

  getText() {
    if (language === "EN") {
      return english;
    } else {
      return spanish;
    }
  }
  render() {
    return (
      <p>{this.getText}</p>
    );
  }
}

SomeComponent.contextType = languageContext;
```

By assigning the context type for a component the component will consume the value of the current context using `this.context`.

### Putting Context All Together

1. Define your context using the `React.createContext(defaultValue)` method, and providing a default value in the parameter.

2. Components that need context can subscribe to changes using the `.contextType` method. Those changes can be utilized by assigning a variable to `this.context`.

3. The component(s) that need to manage the context value should be surrounded by `<contextName.Provider value={this.state.value}><someComponent /></contextName.Provider>`

4. The component(s) that need to consume the context value should be surrounded by the `<contextName.Consumer>{props => { // return value here }}</contextName.Consumer>`
