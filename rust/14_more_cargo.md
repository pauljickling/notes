# More About Cargo and Crates.io

## Customize Builds with Release Profiles

*Release profiles* are predefined, customizable profiles with different configurations that allow a programmer to have more control over various options for compiling code. Each profile has an independent configuration.

There are two main profiles: *dev* and *release*. Both of these profiles have default configurations, however they can be customized and changed in the `Cargo.toml` file.

```
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

`opt-level` controls the number of optimizations the compiler applies to your code with a range from 0 to 3. In the example above, the dev and release profiles are using the default `opt-level` settings. An increase in `opt-level` increases compiling time, but the code will run faster. For a full list of configuration options check the [Cargo documentation](https://doc.rust-lang.org/cargo/).

## Publishing a Crate to Crates.io

There are several features a developer should be aware of if they have a crate they would like to publish to the Crates.io repository.

### Useful Documentation Comments

Like most other languages, Rust uses `//` to create inline commentary in your code. However Rust also has what is known as a *documentation comment* that will generate HTML documentation. This type of documentation is created using three slashes, `///`, and supports Markdown notation.

This documentation is generated when you run `cargo doc`, and you can view it with `cargo doc --open`.

Common Markdown headings used in Rust documentation include:

- **Examples**
- **Panics**
- **Errors**
- **Saftey**

`cargo test` will also test any code snippets you include in documentation blocks to make sure they work as intended.

### Commenting Contained Items

Another type of comment in Rust is the container comment. The purpose of this comment style is to describe the purpose of a particular file. Like the doucmentation comment, it is also automatically included in the cargo documentation, and supports Markdown notation as well. You can add container comments with the `//!` syntax.

### Export a Convenient Public API with `pub use`

Sometimes the way you organize your API is cumbersome for the general public. You can re-export items so they differ from your private structure by using the `pub use` syntax. It takes a public item in one location, and makes it public in another location.

### Creating an Account

You need to login via GitHub at crates.io, and then you will be provided a public API key that you can use to login. Then you can login via `cargo login {API key}`.

### Adding Metadata to a New Crate

You can add metadata to your crate by providing the information in the `Cargo.toml` file. If you try to run `cargo publish` without providing useful metadata information you'll get warning and error. Sample metadata will look like this:

```
[package]
name = "guessing_game"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"
description = "A fun game where you guess what number the computer has chosen."
license = "MIT OR Apache-2.0"
```

### Publishing 

Publishing is permanent, and non-deletable, so make sure you are actually ready to publish a crate before you run `cargo publish`

### Publishing a New Version of an Existing Crate

When updating a crate follow [these guidelines](https://semver.org/) for the value you specify for a new version of your crate.

### Removing versions with `cargo yank`

Although crates are non-deletable, you can use `cargo yank --vers {version no}` to make certain versions unavailable for future projects that try to use your crate. This is useful for versions that suffer from important flaws. You can use the `--undo` flag to reimplement a yanked version as well.

## Cargo Workspaces

*Workspaces* are a way to manage a library crate that grows in complexity. A workspace is a set of packages that share the same *Cargo.lock* and output directory.

### Creating a Workspace

To create a workspace, start by creating a new directory, and then adding a *Cargo.toml* file that will configure that space. Unlike in the root directory, this toml file will not have any metadata or a `[package]` section. Instead it starts with `[workspace]` and you can add members to the workspace by specifying the path to the binary crate in the root directory.

```
[workspace]

members = [
    "adder",
]
```

Once you have configured this you can run `cargo new adder` to get the workspace up and running.

### Creating a Second Crate in a Workspace

You can keep adding members to a crate workspace. Lets say you want to add a new library crate called `add-one`. First you would adjust your Cargo.toml file.

```
[workspace]

members = [
    "adder",
    "add-one",
]
```

Then you would generate the library by running `cargo new add-one --lib`

If your crate depends on other crates, you need to specify that in the correct Cargo.toml file. For example, lets say the adder crate depends on the add-one crate. Then the adder Cargo.toml file should have the following:

```
[dependencies]

add-one = { path = "../add-one" }
```

### External Crate Dependencies

If you include external crates, you will need to specify that external dependency in every Cargo.toml file for every crate that uses the external crate.

### Adding a Test to a Workspace

Tests can be run for a particular crate in a workspace from the top-level directory by using the `-p` flag and specifying the name of the crate you want to test.

## Installing Binaries from Crates.io with `cargo install`

By default any external crate you install with `cargo install` will be installed in the *$HOME/.cargo/bin* directory.

## Extending Cargo with Custom Commands

You can view custom commands with `cargo --list`

