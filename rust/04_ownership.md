## Ownership

Ownership is what sets Rust apart from other programming languages. The way Rust handles ownership is how it avoids memory issues that routinely arise when programming in C and C-derivative languages.

Certain kinds of data are either stored on the stack or the heap. Data on the stack has a known size, and accessing data on the stack is fast because the data exists in a particular order, and there isn't any ambiguity to where new data goes.

On the other hand, data that has an unknown size at compile time goes on the heap. The heap is less organized, and relies on pointers to locate items.

Ownership addresses what data is on the heap, minimizing duplicate data, and cleaning unused data. By following Rust's rules for ownership, the programmer doesn't really need to worry about these memory allocation issues.

### Rules of Rust Ownership

1. Each value in Rust has a variable that's called its *owner*
2. There can only be one owner at a time
3. When the owner goes out of scope, the value will be dropped.

### Demonstration of Ownership

```
let s1 = "hello";
let s2 = s1;
println!("{}", s1); // this raises an error
```

This is an error because s2 is the new owner of the "hello" value. If you wanted to have s1 and s2 have the same value you would want to use the `.clone` method.

```
let s1 = "hello";
let s2 = s1.clone;
println!("{}, {}", s1, s2); // prints "hello hello"
```

Note that `.clone` is used for data types that go to the heap (strings, vectors, etc.) but this is not necessary for scalars or data types that go to the stack (integers, Booleans, char types, floating points, tuples that contain items that don't require clone, etc.)

Remember, if a piece of data is immutable, that means it can be allocated to the stack, whereas if it is mutable then it means that it must get allocated to the heap since there is no way for the compiler to know in advance what the size of the data will be. Consider two ways to declare string variables:

```
let s1 = "hello";
let s2 = String::from("world");
```

The first variable is what is called a string literal. These strings are immutable, so `s1` is allocated to the stack. `s2` on the other hand uses a special kind of syntax to use the String method to create a mutable variable, therefore this variable is allocated to the heap. If you tried to write an additional line of code that stated `let s3 = s2;`, and then tried to invoke `s2` you would raise an error because at that point `s2` is no longer the owner for that value (ownership rule 2). Essentially, we have *moved* `s2` to `s3`.

### References & Borrowing

Use the `&` sign to create references for borrowing. A reference refers to some data without becoming its owner. Referenced data is immutable by default.

It is possible to create a mutable reference (assuming the owner is a mutable variable) using the `&mut` syntax.

Note that for mutable variables you can only create a single reference. You also cannot combine mutable and immutable references. These restrictions prevent data races at compile time, which prevents undefined behavior.

### Slices

Slices are a data type that does not have ownership and references a contiguous sequence of elements in a collection instead of the whole collection (strings, arrays, etc.). Slices are common in many programming languages, but they are particularly useful in Rust to avoid running afoul of ownership rules.
