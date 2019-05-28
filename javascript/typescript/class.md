# Class

The class syntax has existed in JS since ES5 so these notes will forego that discussion, and focus only on the features that TypeScript brings along for the ride.

## Public, Private, and Protected Modifiers

Although you can specify that a class member is `public`, this is unnecessary since members are public by default. Logically it follows that to make a member private, you must specify that it is `private`. The following code will generate an error because name is a private member of the Animal class.

```
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

new Animal("Cat").name;
```

When a member is marked `private` it cannot be accessed from outside of its containing class. Only objects that belong to that class can access those members.

There is also the `protected` modifier for class members. It is similar to `private` but members declared `protected` can be accessed within deriving classes.

## Readonly Modifier

It is also possible to mark class properties as `readonly` so that the values cannot be modified after an instance of the class object is created. *Parameter properties* provide a non-clunky syntax for expressing this:


```
class Octopus {
    readonly numberOfLimbs: number = 8;
    constructor(readonly name: string) {
    }
}
```

## Accessors

TypeScript has `get` and `set` keywords, to allow for finetuning access controls to class properties. They are inferred to be `readonly`.

## Static Properties

Use the `static` keyword for when you want to specify properties that belong to the class itself rather than object members of the class (those objects will inherit those values).

## Abstract Classes

Abstract classes are base classes that other classes can be derived from, and they may not be directly instantiated. They use the `abstract` keyword.

```
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log("roaming the earth...");   
    }
}
```

Methods marked as `abstract` do not contain an implementation and *must* be implemented in the derived class. 
