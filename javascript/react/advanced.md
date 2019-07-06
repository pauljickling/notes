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
