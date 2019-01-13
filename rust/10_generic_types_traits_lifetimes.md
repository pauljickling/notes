# Generic Types, Traits, and Lifetimes

Generics are abstract stand-ins for concrete types. This allows you to write functions that accept parameters of unknown values instead of something specific like `i32` or `String`. The `Option<T>` from chapter 6 is an example of a generic.

*Traits* allow you to further constrain how generics work, restricting the sort of behavior beyond types.

*Lifetimes* are a variety of generics that provide compiler information about how references relate to each other. This allows for borrowing in situations where the compiler would otherwise find the reference invalid.

## Generic Data Types

Generics can be used to create definitions for items like function signatures or structs, allowing for multiple concrete data types.

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

This configuration of generics will handle all possibilities. That is, it can handle x and y belonging to the same type, or differing.

## Generics in Enums

Here's a review of the option enum:

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

Here any Error types produce the `Err` variant, and any `T` types produce the `Ok` variant.

## Traits

The methods that can be called on a type are that type's behavior. Trait definitions are a way to group method signatures together. In that way, they are similar to interfaces in object oriented programming. Suppose you have a `NewsArticle` and `Tweet` struct that have various properties. One method you might want them to share is `summarize`. We can create a `Summary` trait to help define this behavior.

```
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Our `Summary` trait establishes that the behavior for a `summarize` method is that it should take an instance reference as a parameter and return a string. Now any `summarize` method we create that has this trait will have to have these rules regarding what it accepts as a parameter, and what it returns.

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
