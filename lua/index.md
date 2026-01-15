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
