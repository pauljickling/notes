## Problem

Describe and give an example of each of the following type of polymorphism:

- Ad-hoc polymorphism
- Parametric Polymorphism
- Subtype Polymorphism

## Types of 

### Ad-Hoc Polymorphism

Ad-hoc polymorphism is a a type where functions can be applied to arguments of different types. It is sometimes known as function overloading or operator overloading. A simple example would be a function that replicates existing syntax. That is, if you had an `add()` function, it would perform addition for integer types, and concatenation for strings, and other collection types.

### Parametric Polymorphism

Parametric polymorphism maintains full static type-safety where a data type is written genrically so that it can handle values identically without depending on their type. An example would be a function that combines two lists without caring about the type of elements. That is, it consists of arrays of generic types. Parametric polymorphism is sometimes referred to as generic programming.

### Subtype Polymorphism

Subtype polymorphism is where a subtype is a datatype related to another datatype (the supertype). Elements written to operate on the supertype can also operate on the subtype, but not vice versa. This is typically how the term *polymorphism* is used in the context of object oriented programming. Note that subtype polymorphism differs from inheritance since it is a relationship between types rather than a relationship between types of classes/objects. Rust's enums perform a subtype polymorphism as each variant of the enum is a subtype of the enum supertype.
