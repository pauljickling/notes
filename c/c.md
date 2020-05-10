# C

## Compiling Programs

`gcc foo.c -o foo`

## Types

- `char`
- `int`
- `short`
- `long`
- `float`
- `double`
- `long double`
- `void`

Use `unsigned` at the start of a variable to start the data types value range to begin from 0 instead of a negative number.

## Assigning Variables

`int mafia = 36;`

`char m;`

### Constants

`const int YEAR = 2020;`

`#define YEAR 2020`

## Logical Operators

- `!`

- `&&`

- `||`

C also supports ternary operators. JavaScript uses the exact same syntax that C uses.

`sizeof` is an operator that returns the size in memory of a variable or type.

`printf("%ld", sizeof(year));`

`printf("%ld", sizeof(int));`

## Control Flow

`if`, `else if`, `else`, `switch` for conditional logic.

`for`, `while`, and `do while` for loops.

### `do while`

Of these control flow concepts, most can be seen in other popular programming languages. `do while` is a while loop where the loop is always executed at least once, regardless of the condition check. It has the following format:

```
do {
/* loop */
i++;
} while (i < 10);
```

`break` can also always be used to break out of a loop.

## Arrays

Arrays in C will remind you of other statically typed languages. They are not dynamically sized, you need to use linked-lists to create dynamically sized arrays.

`int prizes[3];`

Initializing arrays: `int prizes[3] = { 1, 2, 3 };`

The variable name of an array is a pointer to th first element.

## Strings

Strings are `char` arrays.

`char name[5] = { 'P', 'a', 'u', 'l' };`

Luckily with strings there is a more convenient way to express them.

`char name[5] = "Paul";`

You can print a string variable in a print statement with `%s`.

All strings must end with a `0` value for terminating the string. This is why in the above `name` variable the array length is 5 and not 4. The C std lib includes a `string.h` library that abstracts away a lot of the more annoying aspects of working with strings. It includes, among others, the following functions:

- `strcpy()` copies a string to another string
- `strcat()` concatenates strings
- `strcmp()` compares two strings for equality
- `strlen()` calculates length of string

## Pointers and References

`*` is used to assign pointers, and `&` is used for references.
