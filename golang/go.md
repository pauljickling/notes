# Go Lang

Go is a statically-typed, compiled programming language with a garbage collector. It has a concise syntax, and seems to generally operate in the spirit of the Zen of Python. Although it is very effective as a general purpose language, Go shines in the ease of use, and performant development of concurrent and networkng application programming.

## `Printf` Reference

To include values in a print statement use the following format:

`fmt.Printf("Hello %s", name)`

These conversions are called *verbs*. Here are some of the more commonly used ones:

`%d` decimal integers
`%x, %o, %b` integers in hexadecimal, octal, and binary
`%f, %g, %e` floats
`%t` boolean
`%c, %q` rune (i.e. a Unicode code point, use `%q` when you want to include the quotation marks)
`%s` string
`%v` any value
`%T` type of any value
`%%` how to escape to include a percentage sign

## Quirks

Coming from other languages, these are the features that stand out:

### Variable Declaration

Variables can be declared with an implicit type `var foo := "bar"` which will determine `foo` to be a string.

Variables always have an assigned value when initialized. If a variable is declared without an explicit assignment, the value will be set to a "zero" value. So `var foo string;` is equal to an empty string, i.e. `""`, and `var bar bool;` is equal to `false`. When declaring variables without an explicit assignment, the type must be declared.

The former method of declaring variables is the most common since it is the most concise.

Constants can be declared with the `const` keyword. They are immutable, and known at compile time. It is common convention to declare various `const` values together e.g.

```
const (
    japan = "Tokyo"
    ghana = "Accra"
    italy = "Rome"
)
```

### Integer Types

There are various signed and unsigned `int` types that can be declared. Typically you will simply declare a signed `int` that the compiler will determine is the optimal sized `int` to use for the current operating system (note that this type might vary depending on the compiler you use).

Signed integers are almost always used even in cases where not strictly necessary. Unsigned is typically only used for specific types of arithmetic operations.

### String Types

String slices use the same sort of syntax as in Python, only it does not accept negative integers, so instead you would need to reference the `len()` of the string if you only wanted the last 3 characters of a string (common for example, in cases where you are only looking for files that have a particular extension type).

Go has *raw string literals* where you can write strings that do not interpret any escape characters, which is obviously useful for documentation and instructions. These use the backtick characters.

Converting other data types to strings and vice versa is a common type of operation, and is handled with the `strconv` package.

### Unicode

Go calls unicode code points "runes". Go encodes string using UTF-8.

### Arrays

Arrays in Go are fixed length, and contain a single data type. Typically the length of the array is assigned when the variable is declared.

`var importantDates [3]int = [3]int{1791, 1848, 1905}`

However you can also use an ellipsis to specify that the length of the array is specified by the initial values.

`var nums := [...]int{4, 6, 13}`

If you provide an array length that exceeds the initially supplied values, the subsequent values will be whatever the `0` value is for that type.

It is also possible to specify the index with `const`s that are an `int` type.

```
type Country int

const (
    japan Country = iota
    ghana
    italy
)

capitals := [...]string{japan: "Tokyo", ghana: "Accra", italy: "Rome"}
fmt.Println(capitals[ghana]) // "Accra"
```

Furthermore it is also possible to create a numbered index that is not sequential.

`q1Months := [...]string{1: "January", 2: "February", 3: "March"}`
