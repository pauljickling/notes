# Functional Language Features

## Closures

Closures in Rust are anonymous functions that can save a variable or pass as arguments to other functions. Unlike functions, closures can capture values from the scope where they are defined. This allows for code reuse and behavior customization.

Closure definitions occur after the `=` used to assign it to a variable, enclosed within a pair of verticle pipes i.e. `let x = |closure| { // fn here }`

Imagine a function that has an expensive big O cost associated with it. Because of its slow performance, you want to run it as infrequently as possible. A closure allows thisperformance to only happen in a single place within your code base. And so even if in the business logic you have many places where this function might need to be called, you only have to face the costs once.

Note that you don't need to specify the type for your closures. The compiler will be able to infer the type.

## Iterators

Iterators are a pattern that allow you to perform a task on a sequence of items in turn. The iterator is responsible for performing that task and determining when the sequence is complete. They save programmers from having to implement this logic on their own.

In Rust iterators are *lazy* so they have no effect until they are called. For example, the following code does nothing:

```
let v1 = vec![1, 2, 3];

let v1_iter = v1.iter();
```

On the other hand, now that you have an iterator for this vector, it is easy enough to implement a *for...in* loop.

```
for i in v1_iter {
    println!("{}", i);
}
```

Without the iterator you have to write an old school C loop i.e. `var i =0; i < v1.len; i++ {// loop}`

In addition to reducing syntactical clutter, iterators allow you to use this logic on many different kinds of sequences.

### The `Iterator` Trait and the `next` Method

All iterators have a trait named `Iterator` defined in the standard library. This definition specifies a `type Item` for the trait, and a next function that takes a mutable reference of self as the parameter and returns an Option enum. This defines an *associated type* with this trait. The `next` method returns one item of the iterator at a time wrapped in `Some`m and when the iteration is over, returns `None`.

The `next` method can be called directly.

```
#[test]
fn iterator_demonstration() {
    let v1 = vec![1, 2, 3];

    let mut v1_iter = v1.iter();

    assert_eq!(v1_iter.next(), Some(&1));
    assert_eq!(v1_iter.next(), Some(&2));
    assert_eq!(v1_iter.next(), Some(&3));
    assert_eq!(v1_iter.next(), None);
}
```

Note that the `v1_iter` variable needs to be mutable because the next method changes the internal state that the iterator uses to keep track of where it is in the sequence. By contrast, a for loop takes ownership of `v1_iter`, making it mutable in the process. Also note that the `asser_eq!` macro is comparing a numeric reference because what `next` returns are immutable references to the values in the vector. If you want an iterator to take ownership of v1 then you would call `into_iter` instead, and if you wanted to iterate over mutable references you would call `iter_mut` instead of `iter`.

### Combining Iterators and Closures

The map method shows how these two concepts work together.

```
let v1: Vec<i32> = vec![1, 2, 3];

let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

assert_eq!(v2, vec![2, 3, 4]);
```


