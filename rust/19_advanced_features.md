# Advanced Features

## Unsafe Rust

By default Rust is safe, however there are times when you want to run unsafe code, for example, when you need to do low-level systems programming.

To perform unsafe code, use the `unsafe` keyword, and then start a new block that holds the unsafe code. There are four things you can do in unsafe Rust that are not normally possible:

1. Dereference a raw pointer
2. Call an unsafe function or method
3. Access or modify a mutable static variable
4. Implement an unsafe trait

It's best practice to enclose unsafe code within a safe abstraction and provide a safe API.

### Dereferencing a Raw Pointer

In unsafe Rust there are two types called *raw pointers* that are similar to references. They can be mutable or immutable, and are written as `*const T` and `*mut T`. The astericks acts as part of the name instead of as the dereference operator in this case. An immutable raw pointer can't be directly assigned after being dereferenced.

Raw pointers differ from references and smart pointers because:

- They are allowed to ignore the borrowing rules by having both immutable and mutable pointers, or multiple mutable pointers to the same location
- They aren't guaranteed to point to valid memory
- They are allowed to be null
- They don't implement any automatic cleanup

The lack of safety guarantees comes in exchange for potential greater performance, as well as being able to interface with languages like C and C++, or hardware where Rust's guarantees wouldn't apply.

Raw pointers are implemented as follows:

```
let mut num = 5;

let r1 = &num as *const i32;
let r2 = &mut num as *mut i32;
```

Note that raw pointers are valid expressions in regular, safe Rust, so you wouldn't need to include the `unsafe` keyword to implement this code. `unsafe` is needed to dereference these pointers.

The raw pointers are implemented using `as` to cast an immutable and mutable reference. Since these raw pointers are created from references, they are guaranteed to be valid. But it is possible to create raw pointers that don't have these guarantees.

```
let address = 0x012345usize;
let r = address as *const i32;
```

Here we have a raw pointer pointing to an arbitrary location in memory. There might be data there or there might not be.

To dereference raw pointers:

```
unsafe {
    println!("r1 is {}", *r1);
    println!("r2 is {}", *r2);
}
```

### Calling an Unsafe Function or Method

Unsafe functions/methods have requirements that Rust can't guarantee are met.

```
unsafe fn dangerous() {}

unsafe {
    dangerous();
}
```

Unsafe functions are declared using the `unsafe` keyword, and must be called in an unsafe block.

### Creating a Safe Abstraction over Unsafe Code

```
use std::slice;

fn split_at_mut(slice: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = slice.len();
    let ptr = slice.as_mut_ptr();

    assert!(mid <= len);

    unsafe {
        (slice::from_raw_parts_mut(ptr, mid),
         slice::from_raw_parts_mut(ptr.offset(mid as isize), len - mid))
    }
}
```

### Using `extern` Functions to Call External Code

You can use the `extern` keyword to interact with code written in another language.


```
extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("Absolute value of -3 according to C: {}", abs(-3));
    }
}
```

Functions declared in the `extern` block are always unsafe to call from Rust code since no other language has the safety guarantees provided by Rust. Therefore they belong in `unsafe` blocks.

### Accessing or Modifying a Mutable Static Variable

Global variables can create problems because if two threads are accessing the same mutable global variable it can create a data race.

In Rust, globl variables are called *static* variables.

```
static HELLO_WORLD: &str = "Hello world";

fn main() {
    println!("{}", HELLO_WORLD);    
}
```

Static variables are similar to `const` variables. Static variables must have an annotated type, and they can only store references with the `'static` lifetime.

Static variables differ from `const` variables because they have a fixed address in memory. Constants, on the other hand, are allowed to duplicate their data when they are used. Also static variables can be mutable. However that is unsafe behavior.

### Implementing an Unsafe Trait

A trait is unsafe when at least one of its methods has some invariant that the compiler can't verify. As with functions, traits are declared unsafe with the `unsafe` keyword.Similarly, any method that implements the unsafe trait will also include the `unsafe` keyword.

### When to Use Unsafe Code

There's nothing wrong with using unsafe code, it just is more complicated to code correctly compared to code that has safety guarantees. Unsafe code is useful for instances when you necessarily can't uphold memory safety. If you run into issues, thanks to the `unsafe` blocks you will know where to look.

## Advanced Lifetimes

Most of the time you can ignore lifetimes. However there are some features that allow you to adjust how lifetimes work in Rust.

### Ensuring one Lifetime Outlives Another with Lifetime Subtyping

*Lifetime subtyping* specifies that one lifetime should outlive another. This is specified with the syntax `'b: 'a` where a lifetime would be specified. This syntax is saying that lifetime `'b` lives at least as long as `'a`, that is, `'b` uses `'a`.

### Lifetime Bounds on References to Generic Types

Traits act as bounds on generic types. Lifetime parameters can further constrain generic types, and these are called *lifetime bounds*. They verify that references in generic types won't outlive the data they are referencing.

`struct Ref<'a, T: 'a>(&'a T);`

The `T: 'a` syntax specifies that `T` can be any type, but if it contains any references, the references must live at least as long as `'a`.

Another way to fix this sort of problem is with static lifetimes.

`struct StaticRef<T: 'static>(&'static T);`

With a `'static` lifetime the reference must live as long as the entire program.

### Inference of Trait Object Lifetimes

What happens if the type implementing the trait in the trait object has a lifetime of its own? These are the rules for working with lifetimes and trait objects:

- The default lifetime of a trait object is `'static`.
- With `&'a Trait` or `&'a mut Trait` the default lifetime of the trait object is `'a`.
- With a single `T: 'a` clause, the default lifetime of the trait object is `'a`.
- With multiple clauses like `T: 'a` there is no default lifetime, and therefore more explicit instructions are required.

In instances that need explicit lifetimes specified, a lifetime bound on a trait object like `Box<dyn Red>` can be added using the syntax `Box<dyn Red + 'static>` or `Box<dyn Red + 'a>` depending on whether the reference lives for the entire program or not. As with the other bounds, the syntax adding a lifetime bound means that any implementor of the `Red` trait that has references inside the type must have the same lifetime specified in the trait object bounds as those referenced.

### The anonymous lifetime

Specifying lifetimes can result in a lot of syntactical clutter, so anonymous lifetime syntax `'_` can improve this situation. So something like this:

```
fn foo<'a>(string: &'a str) -> StrWrap<'a> {
    StrWrap(string)    
}
```

becomes this instead:

```
fn foo(string: &str) -> StrWrap<'_> {
    StrWrap(string)
}
```

Essentially the anonymous lifetime says "use the elided lifetime here". This works with `impl` headers too.

## Advanced Traits

### Specifying Placeholder Types in Trait Definitions with Associated Types

*Associated types* connect a type placeholder with a trait so that the trait method definitions can use these placeholder types in their signatures. That way a trait can be defined that used some types without needing to know exactly what those types are until the trait is implemented.

Associated types tend to get used more often then some of the other details that appear in this chapter.

A good example of this is the `Iterator` trait provided by the standard library. The associated type is called `Item` and stands in for the type of the values the type implementing the `Iterator` trait is iterating over. As a reminder, it looks like this:

```
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

`Item` is a placeholder type, and the `next` method returns an `Option` containing a value of that concrete type.

The concept of associated types is not dissimilar to generics. However generics must annotate the types in each implementation. With associated types there is no need to annotate types because we can't implement a trait on a type multiple times.

### Default Generic Type Parameters and Operator Overloading

When using generic type parameters a default concrete type for the generic type can be specified. The syntax is `<PlaceholderType=ConcreteType>`. This is useful with *operator overloading*, that is, customizing the behavior of an operator (such as `+`) in particular situations.

While it isn't possible to create your own operators or overload arbitrary operators, it is possible to overload the operations and corresponding traits found in `std::ops` by implementing the traits associated with the operator.

Default type parameters are mainly used in two ways:

1. To extend a type without breaking existing code
2. To allow a customization in specific cases most users won't need

The `Add` trait is a good use case. Usually you want to add two types together, but there are instances where you might need to customize beyond that.

### Fully Qualified Syntax for Disambigutation: Calling Methods with the Same Name

Different traits can have methods with identical names, and both traits can be implemented on one type. Furthermore it is possible to implement a method with an identical name on the type directly. Disambiguation is needed here.

The compiler will default to the method implemented on the type directly. For the traits more explicit syntax is required to disambiguate.

```
let person = Human;
Pilot::fly(&person);
Wizard::fly(&person);
person.fly();
```

In this example there are three different `fly` methods: one on the `Human` type, one for the `Pilot` trait, and one for the `Wizard` trait, and you see how they all can be invoked separately by specifying the trait name.

One issue is with associated functions that are a part of traits that lack a `self` parameter. In cases where two types implement the same trait the compiler will not be able to determine which type is called for without *fully qualified syntax*. Lets say you have a `Dog` struct and `Animal` trait with a `name` method that returns a `String` without reference to `self`. To use the `Animal` `name` method you would have to use the following syntax:

`println!("The dog's name is {}", <Dog as Animal>::name());`

### Using Supertraits to Require One's Trait Functionality Within Another Trait

If you need one trait to use another trait's functionality then you need to rely on the dependent trait being implemented. The relied on trait is the *supertrait* of the trait you are implementing.

### Using the Newtype Pattern to Implement External Traits on External Types

A trait can be implemented on a type as long as either the trait or the type are local to our crate. This restriction can be circumvented with the *newtype pattern* which involved creating a new type in a tuple struct. The tuple struct acts as a thin wrapper around the type that we want to trait implementation for. *Newtype* is a term originating from the Haskell language. There is no runtime performance penalty for this pattern.

## Advanced Types

### Using the Newtype Pattern for Type Safety and Abstraction

Beyond statically enforcing that values are never confused, and indicating the units of a value, the newtype pattern is useful for several tasks like abstracting away the implementation details of a type. The new type can expose a public API that is different from the API of the private inner type. New types can also hide internal implementationsof the type that is wrapped.

### Creating Type Synonyms with Type Aliases

Rust provides the ability to declare a *type alias* to an existing type of another nameby using the `type` keyword.

`type Kilometers = i32;`

Now `Kilometers` are a synonym for `i32`. Type synonyms are useful for reducing repetition that can result in lengthy function signatures. For example, a smart pointer pointing to a container type of some other type can have a simple, short synonym that will greatly improve readability.

Type aliases are also frequently used with the `Result<T, E>` type.

### The Never Type that Never Returns

The `!` type is a special type that in type theory is known as an *empty type* because it has no values. In the Rust community this is called a *never type* because it stands in the place of the return type when a function will never return.

`fn bar() -> ! {}`

Functions that return never are called *diverging functions*. It is impossible to return values of the type `!` so this `bar` function can never return. The value of a *never type* is that it can be used to force type coercion in a match pattern. This is useful for the `panic!` macro. `continue` and `loop` are two expressions that belong to the `!` type. These functions never end without a `break`.

### Dynamically Sized Types and the `Sized` Trait

*Dynamically sized types*, also referred to as *DSTs* or *unsized types* are types that contain values whose size can only be known at runtime.

The `str` type is a dynamically sized type, and so it isn't possible to create a variable of the `str` type, and it won't be accepted in arguments either. Instead references are created that point to the `str` type. A `&str` type stores two values: the address of the `str` in memory, and its length.

The way `str` and `&str` is used applied to dynamically sized types in general. That is, they have some metadata associated with the size of the dynamic information, and the values of a dynamically sized type are placed behind some sort of pointer.

The `Sized` trait exists specifically to work with dynamically sized types. This trait is used to determine if a type's size is known at compile time. By default, generic functions will only work on types that have a known size at compile time, however there is special syntax to relax this restriction.

`fn generic<T: ?Sized>(t: &T) {}`

A trait bound on `?Sized` is the opposite of a trait bound on `Sized`. It communicates that `T` may or may not be `Sized`. This syntax is only available for the `Sized` trait, not traits in general.

## Advanced Functions and Closures

### Function Pointers

In addition to passing closures to functions, regular functions can be passed to functions. This is useful when you want to pass an already defined function rather than defining a new closure. Using function pointers to do this will allow the use of functions as arguments to other functions. Functions coerce to the `fn` type (not to be confused with the `Fn` trait). The `fn` type is a function pointer.

```
fn add\_one(x: i32) -> i32 {
    x + 1
}

fn do\_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);

    println!("The answer is: {}", answer);
}

```

One place where you would want to use function pointers instead of closures is when interfacing with a language like C that lacks closures.

Function pointers implement all three of the closure traits (`Fn`, `FnMut`, and `FnOnce`) so function pointers can always be passed as an argument for a function that expects a closure. It's best to write functions to accept a generic type and one of the closure traits so that functiojns can accept either.

Another useful trick is with tuple structs, and tuple-struct enum variants. These use `()` which look like function calls, and actually are implemented as functions returning an instance constructed from their arguments. They can also be called as a function pointer implementing the closure traits.

Some people prefer this style of passing function to using closures. Either is considered idiomatic Rust.

### Returning Closures

Since closures are represented by traits that can't be returned directly. In instances where you would like to return a trait, you can use a concrete type that implements that trait as the return value of the function. However closures don't have a concrete type that is returnable. The solution is to use a trait object.

```
fn return\_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}
```

## Macros

You encounter macros as early as Rust's Hello World program. Macros refer to a collection of features in Rust.

- *Declarative* macros with `macro_rules!`
- *Procedural* macros, which come in three kinds:
1. Custom `#[derive]` macros
2. Attribute-like macros
3. Function-like macros

### The Difference Between Macros and Functions

It isn't obvious at first glance what purpose macros serve that cannot be accomplished with function.

Macros are a way of writing code that writes other code, i.e. *metaprogramming*. The `derive` attribute generates an implementation of various traits, and the `println!` and `vec!` macros are all examples of macros that produce more code than what you've written manually.

This still leaves the question of how macros and functions differ. Functions also reduce the amount of code that has to be written and maintain. However macros have some capabilities that functions do not.

A function signature must declare the number and type of parameters the function has. Macros, by contrast, can take a variable number of parameters. `println!` is capable of `println!("hello");` and `println!("hello {}", name);`. Macros are also expanded before the compiler interprets the meaning of the code, so it makes it possible to implement a trait on a given type, whereas a function gets called at runtime and a trait needs to be implemented at compile time.

Macros are more complex to implement compared to functions though because it involves Rust code writing Rust code.

One other important difference is that macros need to be defined or brought into scope before they are called in a file, whereas functions can be defined just about anywhere.

### Declarative Macros with `macro_rules!` for General Metaprogramming

By far the most common form of macro is the *declarative macro*. They allow you to write something similar to a `match` expression. 

The `macro_rules!` construct allows the definition of macros. To make it available outside the crate use the `#[macro_export]` annotation. Valid pattern syntax in macro definitions differs from general pattern syntax because macro patterns are matched against Rust code structure instead of values.

A parentheses set is used to encompass the entire pattern, followed by a `$`, followed by an additional parentheses set that captures values that match a pattern for use in the replacement code. Within `$()` is `$x:expr` which matches any Rust expression and gives the expression the name `$x`.

There is some work being done to deprecate `macro_rules!` because at the moment there are a lot of edge cases to watch out for. 

### Procedural Macros for Generating Code from Attributes

*Procedural macros* are a bit more like functions. They accept some Rust code as an input, operate on that code, and produce some Rust code as an output rather than matching against patterns and replacing the code with other code as declarative macros do.

There are three types of procedural macros, but they all work more or less the same. The definitions must reside in their own crate with a special crate type (this may not be necessary in future versions of Rust). The format for procedural macros is:

```
use proc_macros;

#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {}
```

Where `#[some_attribute]` is replaced with the name of the specific macro. Note the use of the `TokenStream` type. That is the source code the macro is operating on. The attribute specifies what kind of procedural macro is being created.

### How to Write a Custom `derive` Macro

If you were to create a crate called `hello_macro` that defines a `HelloMacro` trait associated with a `hello_macro` function, a user would normally need to implement the trait for each of their types. However we can provide a procedural macro to users can annotate their type with `#[derive(HelloMacro)]` to get a default implementation. They would write some code like this:

```
use hello_macro::HelloMacro;
use hello_macro_derive::HelloMacro;

#[derive(HelloMacro)]
struct Paul;

fn main() {
   Paul::hello_macro(); 
}
```

The macro would produce "Hello, Macro! My name is Paul!"

To enable this sort of feature the Cargo.toml file would need the following:

```
[lib]
proc-macro = true
```

Then you would need to define it in your src/lib.rs file.

```
extern crate proc_macro;

use crate::proc_macro::TokenStream;
use quote::quote;
use syn;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();

    impl_hello_macro(&ast);
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> tokenStream {
    let name = &ast.ident;
    let gen = quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}", stringify!(#name));    
            }    
        }    
    };
    gen.into()
}
```

### Attribute-like macros

Attribute-like macros are similar to custom derive macros, but instead of generating code for the `derive` attribute, they allow you to create new attributes. They are also more flexible, `derive` only works for structs and enums, but attributes can go on other items like functions.

### Function-like macros

Function-like macros are for macros that look like function calls. A `sql!` macro might be called like this:

`let sql = sql!(SELECT * FROM posts WHERE id=1);`

This would parse the SQL statement checking for syntactical correctness. It would be defined in this way:

```
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {}
```
