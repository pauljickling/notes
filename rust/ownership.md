## Ownership

Ownership is what sets Rust apart from other programming languages. The way Rust handles ownership is how it avoids problems that routinely arise when programming in C and C-derivative languages.

### Rules of Rust Ownership

1. Each value in Rust has a variable that's called its *owner*
2. There can only be one owner at a time
3. When the owner goes out of scope, the value will be dropped.

### Demonstration

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

Use the `&` sign to create references for borrowing. Note that for mutable variables you can only create a single reference. You also cannot combine mutable and immutable references.
