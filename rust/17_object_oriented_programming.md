# Object Oriented Programming Features of Rust

There is no consensus on what counts as an object oriented language. For example, sometimes people will claim Python is not an object oriented language because its classes lack public and private definitions.

Rust was not designed as an object oriented language, but like functional programming Rust has been influenced by OOP.

## Characteristics of Object-Oriented Languages

OOP languages share some common characteristics: objects, encapsulation, and inheritance.

### Objects Contain Data and Behavior

Objects package both data and procedures that operate on that data called *methods*. Rust provides this functionality with data packaged in structs and enums, and `impl` blocks provide methods.

### Encapsulation that Hides Implementation Details

*Encapsulation* means the implementation details of an object aren't accessible to code using that object. So the only way to interact with the object is through its public API. Rust supports encapsulation via the `pub` keyword.

### Inheritance as a Type System and as Code Sharing

*Inheritance* is a mechanism that allows a class of objects to inherit another class of object's definitions. These relationships are often referred to as parent-child relationships.

Rust lacks inheritance. The goal of inheritance is to reduce repeat code. The typical example you will see in OOP books is a vehicle class and a car and bike class. All of these different classes of objects have wheels. So you include that in your vehicle class, and then the car and bike class can inherit that property. The drawback of inheritance is that there is a risk of an oversharing more properties than necessary. This makes an existing codebase rigid and inflexible, and can make it difficult to rewrite classes that contain a lot of inheritance. Rust uses trait objects instead of inheritance, which enables *polymorphism*. Polymorphism is sometimes thought of as synonymous with inheritance, but the concept is more general, referring to code that can work with data of multiple types. Rust uses generics to abstract over multiple types, and then contrained by traits. The technical term for this is *bouinded parametric polymorphism*.

## Using Trait Objects that Allow for Values of Different Types

The type system being what it is means that a compound data type needs to contain elements of the same type. What if you want to design something that contains several diffetrent types? Enums are a nice solution so you could specify a variants that could be integers, floats, and strings. This would satisfy the type system.

However there are situations where we want the capacity to extend the set of valid types further depending on the scenario.

### Defining a Trait for Common Behavior

Imagine we are trying to write a GUI library. One feature we need to implement is the ability to draw, and different aspect of the GUI library can utilize this feature. We can define a `Draw` trait that has a method named `draw`.

```
pub trait Draw {
    fn draw(&self);    
}
```

Data can't be added to trait objects, but they allow abstraction across common behavior.

This trait can now be used by different structs that make up the GUI library.

```
pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,    
}
```

The `Screen` struct has a `components` vector of a `Box` type pointing to a `Draw` trait. The `Screen` struct can have a `run` method in its implementation block.

```
impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }    
    }
}
```

This implementation differs from a struct that uses a generic type parameter with trait bounds. Generic types can only be substituted with one concrete type at a time, whereas trait objects allow for multiple concrete types to fill in for the trait object at runtime.

### Implementing the Trait

The advantage of using trait objects is similar to *duck typing* as described in OOP. It means that there is no need to check what a particular instance is in our implementation methods, it just called the method on the relevant component.

### Trait Objects Perform Dynamic Dispatch

The code from monomorphization is *static dispatch* where the compuiler knows what method is called at compile time. The opposite of this is *dyanmic dispatch* where the compiler cannot tell at compile time what method is being called. In that instance, the compiler emits code that at runtime will figure out the method call.

Trait objects must use dynamic dispatch. Because the compiler doesn't know what to do with the method at compile time, this comes with the cost of missing out on some optimization work, but it improves code flexibility.

### Object Safety is Required for Trait Objects

The rules governing object safety are complex, but the two relevant rules for determining if a trait is object safe are:

1. The return type isn't `Self`
2. There are no generic type parameters

Since a trait doesn't know in advance what sort of data it is dealing with, it cannot safely return a `Self` type. The same is true for generic types.
