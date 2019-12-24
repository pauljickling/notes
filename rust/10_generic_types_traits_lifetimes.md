# Generic Types, Traits, and Lifetimes

Generics are abstract stand-ins for concrete types. This allows you to write functions that accept parameters of unknown values instead of something specific like `i32` or `String`. The `Option<T>` from chapter 6 is an example of a generic.

*Traits* allow you to further constrain how generics work, restricting the sort of behavior beyond types.

*Lifetimes* are a variety of generics that provide compiler information about how references relate to each other. This allows for borrowing in situations where the compiler would otherwise find the reference invalid.

## Generic Data Types

Generics can be used to create definitions for items like function signatures or structs that will allow the use of multiple concrete data types.

### Generics in Functions

In functions, generics are placed in the "signature" where data types of the parameters and return value would normally be specified.

Suppose you have two functions. One finds the largest interger in a vector of interger, and the other is a function that finds the largest char value in a string. Generics can be used to combine these two functions into one function that can handle either situation.

```
fn largest<T>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list.iter() {
    	if item > largest {
	    largest = item;
	}
    }

    largest
}
```

Note that this code creates a compiler error. You still need to specify the traits, which will be covered later.

### Generics in Structs

Structs can contain generic field types.

```
struct Point<T> {
    x: T,
    y: T,
}
```

Note that the way this is defined, whatever type the struct ends up being, x and y will have to be of the same type. If you want multiple generic types you can specify that however.

```
struct Point<T, U> {
    x: T,
    y: U,
}
```

This configuration of generics will handle all possibile types permitted by your generic's traits. That is, it can handle x and y belonging to the same type, or differing types. So the following code would be valid:

```
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x : 1.0, y: 4.0 };
    let integer_and_float { x: 5, y: 4.0 };
```

### Generics in Enums

Here's a review of the `Option` enum:

```
enum Option<T> {
    Some(T),
    None,
}
```

Through generics this expresses the idea of an optional value. There could be a value there, but if there isn't there is a value of `None`. We also saw this logic expressed with the Result enum for error handling.

```
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

The Result enum handles two types of Generics, `T` and `E`, and has the variants `Ok` that holds a value of type `T`, and `Err` that holds a a value of type `E`.

### Generics in Method Implementations

When creating methods for structs and enums generic types can be used in these definitions as well.

```
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
```

This method returns a reference of the Point value for x, regardless of its type. Note that the `<T>` must be declared after `impl` so that it can be specified that we are using the type from `Point<T>`, that way the Rust compiler is aware that this is a generic rather than a concrete type. Put another way, if our `Point` struct used a concrete type like `f32` then there would be no need to type something like `impl<f32> Point<f32>` it would instead be sufficient to type `impl Point<f32>`.

It is not necessary for methods to use the generics of the struct's signature. Consider the following method:

```
fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
    Point {
        x: self.x,
        y: other.y,
    }
}
```

This method acts on the the initial struct instance, and takes a second instance as its argument, and returns a new struct instance that has a value of x from the first instance, and a value of y from the instance provided in the argument. It is able to combine multiple types thanks to providing the generics `<V, W>` which do not belong in struct `Point<T, U>`, but are provided in the `Point<V, W>` parameter. So you can have cases where the `impl` block provides generic parameters, and method blocks provide separate generic parameters.

## Traits

Traits inform the compiler about the functionality of a particular type has, and that it shares with other types. This is a very abstract concept so it helps to illustrate with an example. Any Rust type that you can perform a comparison operation on (`>`, `<`, `==`, etc.) has a `PartialOrd` trait. This trait lets the compiler know that Types with this trait have certain logical properties, for example that if a is greater than b, then b cannot also be greater than a. Because this trait exists for this type, it means we can perform comparison operations on this type of data and get results that make sense. If you try to perform a comparison operation on a type that lacks the `PartialOrd` trait then the compiler will complain that it cannot be applied to that type.

This means that different types that share the same traits can call the same methods on those types, and exhibit the same set of behaviors from those method calls.

Suppose you are building a media aggregator tool. One of the things it will do is summarize media content. There will be various types of media: `NewsArticles`, `Tweets`, etc. each of which will be expressed as structs. We can create a `Summary` trait for these differnt media structs that will define a type of function that they all should be able to perform (`summarize`).

```
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Our `Summary` trait establishes a `summarize` method behavior that takes an instance reference as a parameter, and returns a string. Now any `summarize` method we create that has this trait will have to have these rules regarding what it accepts as a parameter, and what it returns.

Here is how those methods would be implemented:

```
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

One important thing to keep in mind with traits is the implementation will only work if it is local to this crate. In other words you can't implement external traits on external types. This rule exists to make sure other people's code won't break your code, and vice versa.

### Default Implementations

It is also possible to create default method behavior for your traits. Lets say with our `Summary` trait we want to have a specific string that is always returned when a `summarize` method is called.

```
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

Now when a `summarize` method is called, it always returns `(Read more...)`.

## Lifetimes

Every reference has a lifetime, that is, a scope for when the reference is valid. They exist to prevent what are called dangling references which cause a program to reference data that was not intended by the reference.

## Lifetime Annotation Syntax

Lifetime annotations describe the relationships of the lifetimes of multiple references without affecting the lifetimes. The syntax is unusual. The names of lifetime parameters must start with an apostrophe, and are usually all lowercase and very short. Within the Rust community `'a` is frequently used. Here are some examples:

```
&i32 // a reference
&'a i32 // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```
