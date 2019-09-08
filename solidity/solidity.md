# Solidity

## Versioning

Solidity always specifies a version for the compiler to avoid future versions that may otherwise introduce breaking changes.

`pragma solidity ^0.4.25;`

## Creating a Contract

Solidity has a `contract` keyword for creating contracts.

```
contract Employment {
    // Terms
}
```

## Basic Types

Solidity has the following types available:

- `uint` for unsigned, 256-bit default, integers
- `int` for signed, 256-bit default, integers
- `ufixed` unsigned fixed point number
- `fixed` signed fixed point number
- `string` for strings
- `bool` can either be `true` or `false`
- `address` a 20 byte value that can hold an Ethereum address
- `address payable` is like an address, but with `transfer` and `send` members
- `enums`
- `functions`
- `struct`
- `mapping(keyType => valueType)` analogous to a hash table


### Typecasting

Solidity follows the Python typecasting syntax of specifying the type with a parameter containing the differing type.

```
uint8 num1 = 5;
uint num2 = 32;
uint8 num3 = num1 * uint8(num2);
```

## Compound Types

Solidity has the following compound types:

- `struct` to create new types
- arrays can be created with `[]` and specifying the type contained in the array. By default arrays are fixed size, but you can create dynamically sized arrays as well.

## Functions

Function parameters always specify the type, and are public by default. For parameters that are not a global value the expectation is to write those parameter names with an undercore at the beginning. Functions can also be specified as private. It is customary to declare the names of private functions with an underscore.

You can also mark functions as pure if they do not modify state. There are also `view` functions that view, but do not modify data. You can also specify what the return value type is for a function.

```
function _createPerson(string _name, uint _age) private pure returns (Person) {
    return person Person(_name, _age);
}
```

### Built-in Functions

Solidity has a built in hash function `keccak256` that takes a `bytes` parameter and returns a 256-bit hexidecimal number.

## Events

Events are handled with an `event` type that takes action when an `emit` type specifies the `event`.

```
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
    uint result = _x + _y;
    emit IntegersAdded(_x, _y, result);
    return result;
}
```
