# TypeScript Basics

TypeScript is a typed superset of JavaScript that compiles to JS. This gives the benefits of programming in a statically typed language while compiling down to useable code for the web.

## Compiling Code

`tsc file.ts`

One interesting aspect of the TypeScript compiler is it will still compile code with errors.

You can also compile an entire directory with the project directory flag `-p`.

`tsc -p {file directory}`

This is a recursive command.

## Type Declarations

```
function hello(name: string) {
  return "Hello, " + name;    
}
```

## Interface

TypeScript also offers up interfaces for creating struct or schema type objects.

```
interface Person {
  firstName: string;
  lastName: string;
  age: number;
}
```
