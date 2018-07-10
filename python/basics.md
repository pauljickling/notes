## Python Basics

Python's syntax is minimal, but to achieve it's minimalism it does enforce certain style guidelines.

### Running Python

Running default Python (2.7.12 on Ubuntu 16.04):
`python {filename}.py`

Running a particular version of Python:
`python3.6 {filename}.py`

Running Python on Windows:
`py {filename}.py`

### Variables

There is no keyword to declare variables in Python.

`hello_world = "Hello world!"`

Strings accept either double or single quotes.

Null values use the keyword `None`.

`nada = None`

Booleans are capitalized.

```
is_earth_flat = False
is_liquid_doing_it = True
```

Variables names are almost always lower case with underscores separating words. Class names however are capitalized.

### Expressing Scope

In Python scope for things like functions and if statements is established by the use of the colon, and 4 space indentations in the new line. This convention is incredibly important to follow.

```
if number < 0:
    print("Number is less than 0")
elif number > 0:
    print("Number is greater than 0")
else:
    print("Number is 0")

def difference(a, b):
    return a - b
```

### Lists, Dicts, Tuples, Sets

A list works approximately the same as arrays in Javascript, and other dynamic languages.

`numbers = [1, 2, 3, 4, 5]`

Dicts resemble Javascript objects, although in Javascript objects keys are always strings, whereas in Python dicts keys can be any immutable type is a valid key.

Tuples are common in many programming languages, but they are not present in Javascript. A tuple is an immutable list. They are expressed this way:

`x = (1, 2, 3)`

Common tuple methods include `count()` which returns the number of times a value appears, and `index()` which returns the index where a value is found.

Sets work like other languages where they are arrays that do not contain duplicative values.

`s = set({1, 2, 3, 4, 5, 5, 5}) # outputs 1, 2, 3, 4, 5`

### Loops

Unlike Javascript, Python does not make use of iterators. So loops in Python are typically expressed as while loops or using the for.. in pattern.

```
i = 0
while i < 5:
    print(i)
    i += 1
for num in [1, 2, 3, 4]:
    print(num)
```

Python also has a built-in range function that is useful for loops.

```
for num in range(30, 100):
    if num % 5 == 0:
        print("fizz")
    else:
        print(num)
```
