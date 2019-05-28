# Interface

Interfaces act as type rules for object properties. As mentioned in the other chapter, most things in JavaScript are an object, so being able to easily define types of objects with specific properties is useful here.

```

enum Size {
    S,
    M,
    L
}

interface Label {
    name: string;
    barcode: string;
    size: Size;
}
```

## Optional Properties

Although the order of these properties is unimportant for an interface, actually having the specified properties is important. However sometimes it is desirable to have a optional properties that are not required. TypeScript supports this with `?` syntax.

```
interface SquareConfig {
    color?: string;
    width: number;
}
```

In this interface a width is necessary, but color is not.

## Readonly Properties

`readonly` can be used to make the value of a property immutable after it is instantiaited.

```
interface Point {
    readonly x: number;
    readonly y: number;
}
```

## Function Types

In addition to specifying the properties of JavaScript objects, interfaces are also capable of specifying function types with the use of a *call signature*.

```
interface Search {
    (source: string, subString: string): boolean;
}

let sampleSearch: Search;
sampleSearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
}
```

## Indexable Types

You can also use interfaces to specify types that can be indexed into an object, along with the corresponding return types when indexing. Here's what an interface would look like for the standard array that accepts the string type as values:

```
interface StringArray {
    [index: number]: string;    
}

let names: StringArray;
names = ["Alice Waters", "Michael Ruhlman"];
```

## Class Types

Interfaces can also enforce class rules with the `implements` syntax.

```
interface ClockInterface {
    currentTime: Date;
}

class Clock implements ClockInterface {
    currentTime: Date = new Date();
    constructor(h: number, m: number) { }
}
```

Interfaces are only concerned with the public side of the class, not the private.
