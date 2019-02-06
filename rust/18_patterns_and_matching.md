# Patterns and Matching

*Patterns* are a special syntax in Rust that match against the structure of types. Patterns can be used in combination with `match` expressions to control code control flow. Patterns consist of a combination of the following:

- Literals
- Destructured arrays, enums, structs, or tuples
- Variables
- Wildcards
- Placeholders

## All the Places Patterns Can Be Used

### `match` Arms

Patterns are used in the "arms" of `match` expressions. Arms are a pattern/expression pairing. The `match` expression for a particular value contains a list of possible patterns that result in different expressions. These expressions need to be *exhaustive*, that is, they must account for all possibilities for a value in a `match` expression. One way to solve this is to have a `_` pattern that matches anything as a final possible match arm.

```
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
```

### Conditional `if let` Expressions

The `if let` expression is a shorter way to write the equivalent `match` expression where there is only a single case/arm. It is also possible to combine `if let` with other expressions like `if let else`, `else if`, and `else if let`.

An example of the utility of this expression pattern is if a user specifies a favorite color, that color can become the background color, or if it is a particular day the background color becomes green, etc.

Unlike with the `match` case, the compiler does not check for exhaustiveness for `if let` expressions, so the programmer must carefully consider the cases being worked with.

### `while let` Conditional Loops

`while let` loops allow a `while` loop to run as long as a pattern continues to match.

```
let mut stack = Vec::new();

stack.push(1);
stack.push(2);
stack.push(3);

while let Some(top) = stack.pop() {
    println!("{}", top);
}
```

### `for` Loops

```
let v = vec!['a', 'b', 'c'];

for (index, value) in v.iter().enumerate() {
   println!("{} is at index {}", value, index); 
}
```

Here the enumerate method produces a tuple for the vector iterator that is used as a pattern to produce output.

### `let` Statements

A `let` statement assigning a variable is also a pattern. Formally stated:

`let PATTERN = EXPRESSION;`

Because this is how variables are assigned it allows for operations like tuple destructuing.

`let (x, y, z) = (1, 2, 3);`

### Function Parameters

Function parameters can also be patterns.

```
fn foo(x: i32) {
    // pass    
}
```

Here `x` is a pattern that matches for an `i32` type. Because this is a pattern-expression pair you can write fancy expressions like this in functions:

```
fn print_coordinates(&(x, y): &(i32, i32)) {
    println!("Current location: ({}, {})", x, y);    
}
```

The function will use the pattern of a referenced tuple containing a `i32` pair as a parameter.

## Refutability: Whether a Pattern Might Fail to Match

Patterns can either be refutable or irrefutable. Patterns that match for any possible value passed are *irrefutable*. For example, variable assignments are *irrefutable* since the variable matches anything, and therefore cannot fail to match. Patterns that can fail to match are *refutable*. For example, the `Some(x)` expression is refutable since its variable could be something, or it could be `None`. If the value is `None` then the pattern will not match.

Function parameters, `let` statements, and `for` loops can only accept irrefutable patterns. A program cannot do anything useful if there is no matching pattern in these instances.

`if let` and `while let` expressions only accept refutable patterns since they are intended to handle the possibility of failure.

The refutability distinction isn't important while you are writing code in Rust, but it's good to be aware of since it can come up in error messages.

## Pattern Syntax

The following is a list of syntax that is valid in patterns, and why you might use each one.

### Matching Literals

Patterns can be matched against literals directly. This syntax is useful when you want your code to take an action if it receives a specific value. In this example the code prints "one".

```
let x = 1;
match x {
   1 => println!("one"),
   2 => println!("two"),
   3 => println!("three"),
   _ => println!("something else"),
}
```

### Matching Named Variables

Named variables are *irrefutable* patterns that match any value. One complication of named variables is that variables declared as part of a mattern inside the `match` expression will shadow those with the same name outside the `match` construct. This can result in some behavior that is difficult to parse out. To create a `match` expression that compares outer named variables instead of shadowed variables requires using a *match guard conditional* instead.

### Multiple Patterns

`match` expressions can match multiple patterns using the `|` syntax (i.e. *or*). The following code prints `one or two`, and would do so even if the variable `x` was assigned to 2.

```
let x = 1;

match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("something else"),
}
```

### Matching Ranges of Values with `...`

`...` syntax allows a range of values to be matched. Ranges are only allowed for numeric and char types.

```
let x = 5;
match x {
   1 ... 5 => println!("one through five"),
   _ => println!("something else"),
}
```

### Destructuring to Break Apart Values

Patterns can be used to destructure structs, enums, tuples, and references.

#### Destructuring Structs

```
struct Point {
    x: i32,
    y: 132,
}

fn main() {
   let p = Point { x: 0, y: 7 };
   let Point {x: a, y: b } = p;
   assert_eq!(0, a);
   assert_eq(7, b);
}
```

Note that destructuring is such a common pattern that there is an abreviated syntax that can be used `let Point { x, y } = p;` when you want variables that use the same name as the name of the struct fields.

#### Destructuring Enums

The pattern to destructure an enum should correspond to the way data stored within the enum is defined.

#### Destructuring Nested Structs & Enums

Nested Enums and structs can be destructured as well. For example, if you have a Color enum with Rgb and Hsv variants, you could have a Message Enum with a ChangeColor variant that accepts the Color enum as a parameter. This could be assigned as `let msg = Message::ChangeColor(Color::Hsv(0, 160, 255));`

#### Destructuring References

When a value that is matching to a pattern contains a reference, a destructure of the reference from its value needs to take place. That is, you need to access a variable holding the value that the reference points to instead of getting a variable that holds a reference.

#### Destructuring Structs and Tuples

Destructuring patterns can be mixed, matched, and nested in more complex ways. For example:

`let ((feet, inches), Point {x, y}) = ((3, 10), Point { x: 3, y: -10 });`

This breaks apart the complex types into their component parts so the values you are interested in can be accessed separately.

### Ignoring Values in a Pattern

There are several ways to ignore values, either partially or entirely, in a pattern.

#### Ignoring an Entire Value with `_`

Using the `_` as a wildcard pattern is useful as the last arm of a `match` expression, but it can be used in any pattern, for example in function parameters.

```
fn foo(_: i32, y: i32) {
    println!("The y parameter is {}", y);
}

fn main() {
    foo(3, 4);
}
```

This function doesn't need its first parameter, and typically you would simply not include it in the function signature. However there are instances when implementing a trait you need to pass a certain type to the function even if you don't need it so this acts as a workaround from the compiler complaining.

#### Ignoring Parts of a Value with a Nested `_`

Values can be partially ignored with a `_` as well.

```
let numbers = (2, 4, 8, 16, 32);

match numbers {
    (first, _, third, _, fifth) => {
        println!("Some numbers: {}, {}, {}", first, third, fifth);    
    }    
}
```

#### Ignoring an Unused Variable By Starting Its Name with `_`

The Rust compiler complains about unused variables because that could be a bug. However unused variables are extremely common in prototyping or the early stages of a project. The underscore can be used so the Rust compiler won't complain.

The difference between the `_` on its own and as a prefix is important. On its own it doesn't bind any values which is why it can act as a wildcard. As a prefix however the assignment is bound to a value.

#### Ignoring Remaining Parts of a Value with `..`

Sometimes you may be dealing with a compound type that has many parts you want to ignore, and the `..` syntax is available for those instances. Using `..` must be unambiguous. If it isn't the compiler will complain.

```
struct Point {
    x: i32,
    y: i32,
    z: i32,
}

let origin = Point { x: 0, y: 0, z: 0 };

match origin {
    Point { x, .. } => println!("x is {}", x),    
}
```

### Extra Conditionals with Match Guards

A *match guard* is an additional `if` condition specified after the pattern in a `match` arm that must also match, along with the pattern matching, for that arm to be chosen. This allows for the expression of more complex ideas than what is possible with patterns alone.

```
let num = Some(4);

match num {
    Some(x) if x < 5 => println!("less than five: {}", x),
    Some(x) => println!("{}", x),
    None => (),
}
```

The `|` operator can also be used for match guards.

```
let x = 4;
let y = false;

match x {
    4 | 5 | 6 if y => println!("yes"),
    _ => println!("no"),
}
```

### `@` Bindings

The `@` operator creates a variable that holds a value at the same time that value is tested to see if it matches a pattern. It tests a value and saves it in a variable within one pattern.

```
enum Message {
    Hello { id: i32 },
}

let msg = Message::Hello { id: 5 };

match msg {
    Message::Hello { id: id_variable @ 3...7 } => {
        println!("Found an id in range: {}", id_variable)
    },
    Message::Hello { id: 10...12 } => {
        println!("Found an id in another range")
    },
    Message::Hello { id } => {
        println!("Found some other id: {}", id)
    },
}
```

### Legacy patterns: `ref` and `ref mut`

Rust used to work differently, and to accomplish certain goals sometimes the `ref` and `ref mut` keywords were used. These aren't needed anymore, but you might see them in older codebases.
