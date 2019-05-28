# TypeScript Types

## Scalar Types

TypeScript has the following scalar types:

- `boolean`
- `number`
- `string`

## Compound Types

### Arrays

There are two ways to assign array types in TypeScript.

1. `let goals: number[] = [0, 2, 1, 1, 0, 3];`

2. `let goals: Array<number> = [0, 2, 1, 1, 0, 3];`

If you want an immutable array, you can use the `ReadonlyArray<T>` type.

### Tuples

Although JavaScript does not have tuples, in TypeScript you can define an array to not dynamically resize, essentially making it a tuple.

```
let person: [string, number];
person = ["Hayao Miyazaki", 78];
```

Note that unlike the `ReadonlyArray<T>` type, tuples are not immutable. They simply cannot be resized.

### Enums

Arguably one of the coolest features of TypeScript is it supports enums.

```
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

Underneath the hood these enum values are zero base index numbers that have been assigned names. This means you can access an enum value via number or name.

### Object

Because of how JavaScript is implemented, any non-primitive type is an `object` type. That is, the only non-objects in JavaScript are `number`, `string`, `boolean`, `symbol`, `null`, and `undefined`.

## Additional Types

### Any

You can also bypass your static checking with the `any` type.

### Void

Void is the opposite of `any` and has no type. You can use `void` for functions that do not return a value.

### Null and Undefined

Unlike `void`, `null` and `undefined` are subtypes of other types.

### Never

The `never` type represents values that never occur.
