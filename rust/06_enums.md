# Enums and Pattern Matching

## Enums Definition

Enums enumerate all possible values. They list out what are known as *variants*. Variants are a custom data type.

```
enum IpAddrKind {
    V4,
    V6,
}
```

The `IpAddrKind` enum has two variants: `V4` and `V6`.

### Enum Values

Once the enum has been defined you can create instances of an enum's variants.

```
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

Enums are useful when you want to handle multiple types in a struct or function. In fact, integrating custom enums into structs is a common Rust design pattern.

It is also possible to specify data within an enum.

```
enum IpAddr {
    V4(String),
    V6(String),
}
```

Furthermore you can mix and match various types of data with an enum.

```
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

This `Message` enum contains four differing variants:

1. The `Quit` variant is a type with no associated data
2. `Move` is a variant with an anonymous struct inside it
3. `Write` is a variant that contains a `String` value
4. `ChangeColor` is a variant that contains three `i32` values

Like structs, it is also possible to add custom methods to enums using `impl` blocks.

### The `Option` Enum and Its Advantages Over Null Values

`Option` is an enum from the standard library that you will see all over the place in Rust, so it's very important to understand how it works.

Rust doesn't have a *null* value. It's a source of headaches in other langauges. But it's a useful concept. In Rust the whole issue is circumvented with the `Option` enum.

```
enum Option<T> {
    Some(T),
    None,
}
```

This enum specifies that it accepts a generic type (that is, any type), and it has two variants: `Some(T)` containing that type, or `None`. The `Result` type in error handling follows a similar logic.

## The `match` Control Flow Operator

The natural question is if you have an `Option` instance with a `Some` variant, how do you work with the data contained in `Some`?

Rust provides a control flow operator called `match` that allows the comparison of a value against a series of patterns, and then executes code based on the pattern that matches. Patterns can consist of literal values, variable names, wildcards, and other things.

One important distinction from a control flow operator like `if` is `if` requires a boolean type for comparison, but `match` can evaluate many different types.

### Patterns that Bind to Values

A useful feature of `match` is that you can bind the parts of the values that match the pattern. This means you can extract from various enum variants as needed.

### Matching with `Option<T>`

This gets us to the earlier question of what to do about the inner value of a `Some` case. A `match` expression is an excellent way to unpack a value.

```
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }    
}

let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
```

### Matches Are Exhaustive

When you use `match` you need to account for every variant of the enum you are matching against.

### The `_` Placeholder

`_` acts as a wildcard for when you don't want to enumerate all possible values of your `match` expression.

Like an `if` statement, the order of `match` arms matters. After a pattern is matched the `match` is done, so typically wild cards appear at the end of a `match`.

## Concise Control Flow with `if let`

Another way to handle `Some` values is with the `if let` pattern, which is more succinct than the `match` expression.

```
let some_u8_value = Some(0u8);
if let Some(3) = some_u8_value {
    println!("three");
}
```

`if let` is a convenient expression for cases where you are only concerned about a single pattern, whereas `match` will be needed for instances where you require multiple arms.

It's also possible to include an `else` statement with the `if let` pattern, and that sort of acts as an alternative to the `_` wildcard.
