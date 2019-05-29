# Functions

TypeScript supports standard function behavior in JS. TypeScript assumes every parameter in a function is used, although in JS parameters are optional. This functionality can be supported in TypeScript with the `?` syntax.

```
function name(first: string, last?: string) {
    if (last)
        return `${first} ${last}`
    else
        return first
}
```

It is also possible to provide default parameter values.

```
function name(first: string, last = "Jickling") {
    return `${first} ${last}`    
}
```

You can also use *rest parameters* that take an arbitrary number of arguments.

```
function name(first: string, ...restOfName: string[]) {
    return first + " " + restOfName.join(" ");    
}
```
