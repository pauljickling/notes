# Error Handling

Rust groups errors into two categories: *recoverable* and *unrecorverable*. In most programming languages errors are treated the same, and it uses exception mechanics to handle errors. Rust by contrast doesn't have exceptions. It relies on the type `Result<T, E>` for recoverable errors and `panic!` for unrecoverable errors. We saw something similar to the result type with the vector's `get` method.

## panic!

When the `panic!` macro executes, the program prints a failure message, cleans up the stack, and quits. It is possible to change the behavior of `panic!` so it doesn't clean up the stack memory, leaving it to the OS to sort it out. You can specify this in the `Cargo.toml` file.

```
[profile.release]
panic = 'abort'
```

So if you are running the release version of a program it will simply abort instead of unwinding. This creates a smaller memory profile, but is otherwise generally undesirable.

You can also get a better understanding of an error in your program by running `RUST_BACKTRACE=1 cargo run`. This command runs through the steps the Rust program executes.

Generally speaking you should be looking for something like `panic::main` for the source of the error.

## Result

The `Result` type is an enum that has two variants, `Ok` and `Err`.

```
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`T` and `E` are generic type parameters (more on generics in the next chapter). `T` stands for the Type of value that will be returned in a success case with the `Ok` variant, and `E` stands for the type of error that will be returned in the failure case with the `Err` variant.
