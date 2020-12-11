# Go Lang

Go is a statically-typed, compiled programming language with a garbage collector. It has a concise syntax, and it is an effective general purpose language, but it shines in the development of concurrent networking application programming.

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

### Slices

Slices are variable length sequences with an underlying array that consist of three components:

1. a pointer
2. a length
3. a capacity

The pointer is the start of the slice, the length is the number of slice elements, and the capacity is is the number of elements between the start of the slice, and the end of the underlying array.

Slices are defined with two operators that define the scope of the slice. `my_slice[start:end]`.

### Maps

Go uses the `map` type to create hash tables. You can create a map using the `make()` function. Alternatively, you can construct a map literal when you have some initial key/value pairs you want in your map.

```
capitals := map[string]string {
    "japan": "Tokyo",
    "ghana": "Akkra",
    "italy": "Rome",
}
```

The built-in `delete()` function can remove key/value pairs. `delete(capitals, "laputa")`.

Map values are accessed with bracket notation.

### Structs

Structs are an aggregate of arbitrary data types with values called *fields*.

```
type Country struct {
    Name        string
    Capital     string
    Pop         int
    GDP         int
    Landmass    int
}
```

Struct fields are accessed with dot notation. Struct field names can also be imported if they are capitalized.

Struct literals can also be declared. `type Point struct{ X, Y int }`.

It is possible to embed structs as fields within other structs.

```
type Circle struct {
    Center  Point
    Radius  int
}
```

This would make accessing the fields of a struct instance more verbose though, so it is possible to declare *anonymous fields*, fields with no field name.

```
type Circle struct {
    Point
    Radius  int
}
```

This way, when trying to access the `Circle` instance's `Point` fields, `my_circle.X` can be accessed instead of `my_circle.Center.X`. This is especially useful if there are multiple layers of embedded structs.

## Functions

Functions have a name, parameters, an optional result list, and a body. If there is no optional result list provided, that means that there is no returned value. The result list does not have to have a returned variable name, it can just specify the data type, in which case it does not need to be enclosed in parenthesis. If there is a provided result list, then the function body must end in a return statement.
