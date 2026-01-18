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

## Chapter 4: Statements

The following are all valid assignments in Lua:

`a = 10`
`a = 10; b = 20`
`a, b = 10, 2*x`

Swapping the values of variables is possible.

`x, y = y, x`
`a[i], a[j] = a[j], a[i]`

Lua also supports local variables with the keyword `local`. `local` variables are more tightly scoped and performant, and so it is good practice to default to using them.

Lua has the standard `if`, `elseif`, and `else` conditionals. Lua uses the keywords `then` and `end` to structure the conditionals.

Lua supports `while` conditions. Lua also makes use of a conditional called `repeat`.

```lua
repeat
  line = io.read()
  until line ~= ""
  print(line)
```

Both `while` and `repeat` conditionals execute a sequence of commands until a condition becomes true, however `repeat` conditionals are guaranteed to execute at least once.

Lua also has C style `for` loops. `for i=1,10,1 do print(i) end` will print the numbers 1 through 10. The initial `i` variable here is always locally scoped to the for loop. The second expression is the terminating expression, and the third (optional) expression is the increment.

Lua also has for loops that act as iterator functions, able to iterate through tables for example. `for k in pairs(t) do print(k) end`.

`break` and `return` statements will jump out of an inner block.

## Chapter 5: Functions

Function structure:

```lua
function add(a)
  local sum = 0
  for i,v in ipairs(a) do
    sum = sum + v
  end
  return sum
end
```

Functions in Lua may return multiple results. This can be done by including comma separated values.

```lua
function maximum (a)
  local mi = 1          -- maximum index
  local m = a[mi]       -- maximum value
  for i,val in ipairs(a) do
    if val > m then
      mi = i
      m = val
    end
  end
  return m, mi
end

print(maximum({8,10,23,12,5}))     --> 23   3
```

Lua supports functions that have a variable number of arguments. This is done by placing `...` in the function parameters. When the function is called all the arguments will be collected in a single table.

Functions can also have fixed parameters, and a variable number of parameters. e.g. `function g (a, b, ...) end`.

Lua also supports named arguments instead of positional arguments for when it is desirable to have a function that receives arguments in an arbitrary order. The parameter list uses squiggly brackets in this case e.g. `rename{old="temp.lua", new="temp1.lua"}`. This function would be defined in this way:

```lua
function rename (arg)
  return os.rename(arg.old, arg.new)
end
```

This is naturally especially useful for terminal or GUI interfaces that might accept dozens of optional flags.

## Chapter 6: More About Functions

All functions in Lua are anonymous. Functions have names by having variables assigned to them. This also means that you can assign functions to other variables. Functions can also be passed as arguments to other functions.

Functions in Lua use closure, so a function always contains everything it needs to access values correctly. This is useful for functions calling other functions, and callback functions.

Since functions are first-class, it means that functions can be global or local in scope, just like any other variable. Locally scoping functions typically involves assigning them to table fields. In addition to the standard method to defining the contents of tables, Lua offers a syntax shortcut:

```lua
Lib = {}
function Lib.foo (x,y)
  return x + y
end
function Lib.goo (x,y)
  return x - y
end
```

When using functions recursively, by default Lua will try to call a function globally, which will result in undesirable behavior. The solution to this is to declare the local variable outside the scope of the function.

## Chapter 7: Iterators and the Generic `for`

Lua supports Python-style iterative `for` loops.

```lua
for i, v in ipairs(arr) do
  print(v)
end
```

`ipairs` is a built-in iterator that iterates over the elements of a an array. There is also the `pairs` function for iterating over elements in a table, using the `next` function to iterate through a table in arbitrary order. It is also possible to invoke this function directly.

```lua
for k, v in next, t do
  // do something
end
```
