# Common Collections

In contrast to scalar data types that contain a single value, collections contain multiple values. The collection types discussed here are also distinct from arrays and tuples because they point to data stored on the heap. This means that they can vary in size.

## Vectors

Vectors can store multiple values of the same type.

There are two ways to initialize vectors: by declaring an empty vector, or by declaring a vector with initial values.

```
let empty: Vec<i32> = Vec::new();

let halfFull = vec![1, 2, 3, 4, 5];
```

Notice how the non-empty vector created doesn't have its type declared because the compiler is able to infer from the data stored. Whereas the type for an empty vector must be declared. However it is possible to create an empty vector that could hold any type using the syntax `Vec<T>`.

### Updating Vectors

Elements are added to a vector using the `push` method.

```
let mut v = Vec::new();
v.push(5);
v.push(6);
v.push(7);
v.push(8);
```

Note that for this to work we have to make sure the vector is mutable.

### Reading Vector Elements

There are two ways to access vector elements: indexing syntax, and the `get` method. Consider the vector `let v = vec![1, 2, 3, 4, 5];`.

```
let third: &i32 = &v[2];
println!("The third element is {}", third);
```

```
match v.get(2) {
    Some(third) => println!("The third element is {}", third),
    None => println!("There is no third element."),
}
```

One thing to note is the `Some`/`None` syntax for the `get` method. This essentially counts as a form of error handling. If you try to access a non-existent element in the vector using indexing syntax, the program will simply crash out. However if you do the same thing with the `get` method it simply returns the `None` option. Thus, how you want the program to behave will have implications for which method you should use to access vector elements.

One has to be careful with borrowing values from a mutable vector. If you assign a variable to a vector reference, and then push a value to the vector, the vector will be reassigned in memory, and so your variable's reference will be lost. This is why the borrowing rules work the way they do.

### Iterating through Vectors

You can use `for..in` loops in Rust to iterate through vectors.

```
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}
```

If the vector is mutable, it is also possible to transform vector elements using this technique. This adds 50 to each element.

```
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

### Use Enums to Store Multiple Types in a Vector

Since enums can handle multiple types, but still count as the enum type, you can use enums when you need a vector collection that contains multiple types.

```
fn main() {
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
}
```

Of course this technique will only work if you know what types you need to work with. Techniques to handle this will be covered later.

## Strings

The operations that are available for vectors are largely available for strings. For example, you can create an empty string.

`let mut s = String::new();`

Data that has a `Display` trait can now be piped to our mutable string variable using the `to_string` method.

```
let hello = "Hello world";

let s = hello.to_string();
```

The function `String::from` also makes it possible to create a string from a string literal.

`let s = String::from("hello world");`

### Updating Strings

Strings can be concatenated by either using the `+` operator, or the `format!` macro.

It is also possible to append to a string using either the `push_str` method for string slices, or the `push` method for single characters. The `push_str` method does not take ownership of the string slice, so the variable that is pushed is still available for use. This is not the case for concatenation methods. For example:

```
let hello = String::from("hello ");
let world = String::from("world");
let hw = hello + &world;
```

In this example the `hello` variable is no longer available because the `hw` variable now has ownership.

### Indexing into Strings

In most other languages, you can use index operations to access string characters. Rust is not one of those languages. Writing something like `let c = &s[0];` produces an error. This is an ambiguous expression in Rust. What is it supposed to return? It could refer to characters, bytes, grapheme clusters, or a string splice. You can remove this ambiguity by specifying a range.

```
let hello = "hello";

let he = &hello[0..4];
```

Each char is 2 bytes so the `he` variable returns "he". This is dangerous though, if you write `&hello[0..1]` this will create a runtime panic.

There are other ways to access string elements however. For example, you can use the `char` method in a for loop to iterate through each character.

```
for c in "hello".chars() {
    println!("{}", c);
}
```

The bottom line though is strings are difficult in Rust.

## Hash Maps

A hash map handles a collection of key value pairs. You need to use the standard library to use a hash map.

```
use std::collections::HashMap;

let mut soccer_scores = HashMap::new();

soccer_scores.insert(String::from("Colombia"), 2);
soccer_scores.insert(String::from("Sweden"), 1);
```

Hash maps are like vectors, they need their keys and values to be of the same type. It's also possible to use the `collect` method on a vector of tuples to construct a hash map.

```
fn main() {
use std::collections::HashMap;

let teams  = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();
}
```

Data types that have the `Copy` trait are copied into the hash map, whereas those with owned values are moved into the hash map, and the hash map becomes the owner of that data.

### Accessing Hash Map Values

The `get` method is used to access hash map values.

`let score = soccer_scores.get("Colombia");`

You can also iterate over hash maps.

```
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

### Overwriting Values

Like in other programming languages, if you assign a key to a different value, the original value is overwritten. Rust also has a `or_insert` method that can be used to write a key value only if there is no key value pair that already exists in the hash map.

### Updating Values

Another common use case for hash maps is to update values, for example counting how many instances of a word exist in a document.

```
fn main() {
use std::collections::HashMap;

let text = "hello world wonderful world";

let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}

println!("{:?}", map);
}
```

### Hashing Functions

By default the `HashMap` collection uses a cryptographically strong hash function that can provide resistance to DoS attacks. The trade-off is that it is not the fastest algorithm available. You can implement a different hashing function if you need better performance.
