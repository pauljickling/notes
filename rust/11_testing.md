# Testing

Rust uses test functions to verify the behavior of non-test code. The body of a test function performs three actions:

1. Set up any needed data or state
2. Run the code you want to test
3. Assert the results are what you expect

Some of the features Rust provides for writing tests include the `test` and `should_panic` attributes, and some macros. By attributes we mean metadata about pieces of Rust code. To change a function into a test function, add `#[test]` on the line before the function.

```
#[test]
fn test_code() {
    // test body
}
```

From the command line you can run `cargo test` to run your test functions. Anytime you generate a new library using cargo it will create a `src/lib.rs` file that looks like this:

```
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}

```

The `assert_eq!` macro is a conventional test format that checks if the two parameters are equivalent, and if so, the test function will pass when you run `cargo test`. The test attribute data above the function tells the `cargo test` command to run this particular function.

In addition to the `assert_eq!` macro, there is also the `assert!` macro. The `assert!` macro checks to see if some condition in a test evaluates to `true`. If the value is `false` then it will call the `panic!` macro, which is used to cause a test to fail.

A complement macro to `assert_eq!` is `assert_ne!` (i.e. assert not equal).

Often in testing we want to create custom failure messages. You can see how this is implemented for a test function that is checking to see if a function produces a name:

```
#[test]
fn greeting_contains_name() {
    let result = greeting("Carol");
    assert!(
        result.contains("Carol"),
        "Greeting did not contain name, value was `{}`", result
    );
}
```

## should_panic is an unreliable attribute

In addition to the `#[test]` attribute, you can also further define behavior with the `#[should_panic]` attribute. This attribute will fail any test that should panic, but does not. However there is a nuance to this. That condition will only pass when it is from the resultant code. If you were to test a function that used a `panic!` macro, the test would fail even though a panic did take place. We can add more specificity to this type of test by including an optional `expected` parameter to the attribute.

```
impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 {
            panic!("Guess value must be greater than or equal to 1, got {}.",
                   value);
        } else if value > 100 {
            panic!("Guess value must be less than or equal to 100, got {}.",
                   value);
        }

        Guess {
            value
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic(expected = "Guess value must be less than or equal to 100")]
    fn greater_than_100() {
        Guess::new(200);
    }
}

```

## Using Result<T, E> in Tests

Instead of relying on panics, we can also use the Result enum to check test behavior.

```
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 2 == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
}
```

## Controlling How Tests Are Run

By default tests are run in parallel so they can return results faster. However there are some instances where you may not want to do that. In those cases you can run `cargo test -- --test-threads=1` to force the program to run in a single thread. This is useful if you have code that shares state.

Usually you only see printed statements for failed tests, if you want to see if for passing tests as well use the `--nocapture` flag.

You can also use the name of any test function to just use that test. So if you had a test function called `one_hundred()` you would type `cargo test one_hundred`. You can also create a filter to run multiple tests. So if you ran `cargo test add` that would run any test function with `add` in the name. Therefore it is a good idea for tests that share a common purpose to have a common name among them.

You can also add a `#[ignore]` attribute to tests that are time-consuming to run. `cargo test` will now ignore those tests. If you run `cargo test -- --ignored` it will *only* run those tests.

## Test Organization

The Rust community tends to group tests into two categories: *unit tests* and *integration tests*.

### Unit Tests

Unit tests test a unit of code in insolation to quickly identify if a particular function is behaving as intended. Conventionally these are placed in a module named tests and the module is annotated with `[cfg(test)]`.

### Integration Tests

Integration tests are external to your library. They can only call functions that are part of your library's public API. They check to see if different parts of your library work together correctly. Conventionally these are placed in a separate `tests/` directory.

To run all the tests in a particular integration test file use the `--test` flag along with the name of the integtration test file.
