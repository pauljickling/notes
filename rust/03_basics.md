# Rust Basics

After creating a project using Cargo, you will typically create a main.rs file in your src directory. Like in C++, Rust files typically have a main function wrapper.

```
fn main() {
    // Rust code goes here
    let name = "Paul";
    println!("Hello, {}!", name);
}
```

## Declaring Variables

Rust uses the `let` keyword to define variables.

`let x = 5;`

By default variables in Rust are immutable. In the previous line, if we added a line of code that read `x = 1;` the compiler would raise an error. If you want a mutable variable you will need to declare it with the `mut` keyword.

```
let mut x = 5;
x = 1;
```

In addition to mutable variables, Rust has a concept known as *shadowing* where you redeclare the value of a variable, and the first variable becomes shadowed by the second. Shadowing even allows you to change the type of the variable, which is not something you can do with a mutable variable.

```
let x = 5;
let x = x + 5; // x is 10
let mut four = "four";
four = four.len(); // This will raise an error
```

In addition to variables, Rust has constants, which can be declared using the `const` keyword, and cannot be declared mutable. The naming convention for constants is all uppercase, with underscores between words.

`const TWO_WORDS = "Two words";`

## Data Types

Rust is a statically-typed language, so that means that when a variable is declared the Rust compiler must be able to determine what type a variable is. Sometimes this can be inferred, however in instances where multiple types are possible the compiler will throw an error. The syntax for annotating a type looks like this:

`let number: u32 = 9;`

In this example, the number variable has the unsigned 32-bit integer as its type. Note that when declaring functions the function's parameter types must be annotated.

Rust has various data types that are broadly divided into two categories: scalar, and compound.

### Scalar Data Types

Scalar types represent a single value. Examples of scalar types include integers, floating-point numbers, booleans, and characters.

##### Integers

There are two types of integers: signed and unsigned. This refers to whether or not an integer has the potential to be a negative number (i.e. whether it requires a positive or negative sign to properly assign its value).

Integers have different potential lengths: 8-bit, 16-bit, 32-bit, 64-bit, and "arch" (short for computer architecture, so either 32 or 64).

Signed integers can store from -(2<sup>n - 1</sup>) to 2<sup>n - 1</sup> - 1 numbers where n is the number of bits used. Unsigned integers can store from 0 to 2<sup>n</sup> - 1 numbers.

When annotating integers in a variable they are declared using `u` for unsigned integer or `i` for signed integer and the bit number or arch for the size.

`let five: i32 = 5;`

Rust defaults to the `i32` type, and it is best practice to default to that barring specific needs and considerations for your program.

#### Floating-Points

Floating-points are numbers with decimals. There are the `f32` and `f64` types. Unlike with integers it is best to to default to 64-bit numbers since they are more precise.

`let short_pi: f64 = 3.14;`

#### Booleans

Booleans are expressed with the words `true` and `false` in lowercase. The type annotation is `bool`.

`let t: bool = true;`

#### Characters

The character type is for single characters, and is annotated by using single quotes.

`let c = 'c';`

### Compound Data Types

Compound types group multiple values of other types into one type. Rust has two primitive compound types: tuples and arrays.

#### Tuples

Tuples group some number of other values with a variety of types together into one compound type. They are declared by writing a comma separated list within a parenthesis.

`let tup: (i32, f64, u8) = (500, 3.14, 1);`

Using the tuple pattern with `let` to assign variables to items in the tuple is called *destructuring*.

`let (t1, t2, t3) = tup;`

Tuple elements can also be assigned and accessed via dot notation.

`let five_hundred = tup.0;`

#### Arrays

Arrays must contain elements of the same type. Once an array is declared, it has a fixed length.

`let arr = [1, 2, 3, 4, 5];`

## Functions

As mentioned earlier, function parameters must have their type annotated. Functions contain *statements* and *expressions*. A statement is an instruction that performs an action. An expression is an instruction that returns a value.

```
fn main() {
    let x = 1;
    let y = {
        let x = 2;
        x + 1;
    }
}
```

In this example the declaration of the x variable is a statement, and the declaration of the y variable is an expression where (within the block scope of the y declaration) x has been shadowed, and then an expressive operation returns the shadowed x + 1.

When using functions to return values you use an arrow `->` to annotate the return value.

```
fn ten() -> i32 {
    10
}
```

## `if` Statements

If statements in Rust must evaluate a `bool` type.

## Loops

Rust has several types of loops.

### loop

`loop` is used to repeat a set of instructions until the `break` keyword is encountered.

### `while`

Rust also has `while` loops.

### `for`

Rust uses the `for {var} in {compound type}` structure to iterate through compound types. The concise and safe nature of the for loop makes it the preferred loop method in Rust.

```
for number in (0..9) {
    println!("{}", number);
}
```
