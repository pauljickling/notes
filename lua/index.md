# Lua

Notes from [Programming in Lua 1e](https://www.lua.org/pil/contents.html)

## Chapter 1

`print("Hello world")`

Lua executes code in chunks. A chunk is a sequence of statements. Statements are your typical lines of code `a = 1` or `b = a*2`. Lua can use semicolons to separate statements, but this is just a convention. Line breaks also make no difference to the interpreter.

Variables that begin with underscores are reserved for special uses in Lua, so should not be used for naming variables.

`and` is a reserved word.

Lua has a standalone interpreter, but can also be compiled into C.

## Chapter 2: Types and Values

Lua has the following variable types: nil, bool, number, string, userdata, function, thread, and table. Variables can be identified with the `type()` function.

Global variables begin with a `nil` value.

Booleans in Lua are not the only values available for conditionals. Be careful, Lua uses truthy/falsy evaluations in that both `false` and `nil` are false conditions. Though it is unlike JavaScript/Python where `0`, empty strings, arrays, etc. are also false conditions.

All numbers in Lua are double-precision floating point. There are no int types.

Lua uses 8-bit strings. Strings are immutable. String memory allocation is managed by Lua's garbage collector. Lua strings support C-style escape-sequences.

Lua will attempt to do type conversions where possible at runtime.

Tables are Lua's associative arrays. They can function as indexed arrays or as hashtables.

Functions are first-class values in Lua, so they can be assigned as variables.

Userdata allows arbitrary C data to be stored in Lua variables.

Threads are for coroutines, covered in Ch. 9.

## Chapter 3: Expressions

In addition to standard arithmetic operations, Lua supports exponents using `^`.

Lua supports the standard relational operators `<`, `>`, `==`, `<=`, and `>=` although curiously the not equal operator is `~=` rather than `!=`.

Lua has the logical operators `and`, `or`, and `not`.

Lua can concatenate strings with the `..` operator, e.g. `print("Hello " .. "World")`

Lua uses constructors to create and initialize its tables.

An indexed array can be initialized like so, `pynchon_names = {"Teddy Bloat", "Tyrone Slothrop", "Jessica Swanlake", "Francisco Squalidozzi"}`. Note that indexed arrays are unusually 1-indexed. Using an index value out of range returns a value of `nil`.

Whereas for a hash table structure it would be indexed thus: `a = {x=0, y=0}`

Lua tables can have indexed and associative values. Associative values are not counted in the index. Thus the following `fruits = {"papaya", "banana", x="mango", "kiwi"}` would result in `fruits[3]` -> `kiwi`. Because of this convention, you can work around the 1-indexed array by including a `[0]="foo"` value in your table.

Associative values can be retrieved with square or dot notation, e.g. `fruits["x"]` or `fruits.x`.
