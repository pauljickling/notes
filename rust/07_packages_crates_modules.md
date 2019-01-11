## Crates and Packages

A *crate* is a binary or library. Its *root* is a source file that is used to build the crate. This will be the `src/main.rs` file, and perhaps also a `src/lib.rs` file.

A *package* has a Cargo.toml file that describes how to build the crate or crates contained in the package. Thus, anytime `cargo new {project name}` is run, it can be said that you are creating a new package.

## Modules

When you create a new Rust package, you will typically have a source file like this:

```
fn main() {
    println!("Hello world!");
}
```

You can also create modules to organize your code.

```
mod drinks {
    fn macchiato() {
        // function stuff
    }
}

fn main() {
    println!("Hello world!");
}
```

You can also create a hierarchy of modules by nesting modules inside of one another.

```
mod drinks {
    mod coffee {
        fn macchiato() {
	    // function stuff
	}
    }

    mod aguaFresca {

    }
}
```

Module structures can resemble a computer directory system.

## Paths

To call a function you need to know its path. There are *absolute paths* that are from a crate root, and *relative paths* that start from the current module, and use `self`, `super` or an identifier in the current module. Both types of paths are followed by one or more identifiers separated by `::`.

Here's how the macchiato function from the above example is called:

```
// mod structure

fn main() {
    // Absolute path
    crate::drinks::coffee::macchiato();

    // Relative path
    drinks::coffee::macchiato();
}
```

There's one problem with this code though. By default, modules are private (also functions, structs, enums, and constants). To fix that we need to utilize the `pub` keyword.

```
mod drinks {
    pub mod coffee {
        pub fn macchiato() {
	    // function stuff
	}
    }
}
```

This code makes the coffee module public, but not the drinks module. Thus, any functions that might exist in the aguaFresca module wouldn't be available. Note that just because the module is public doesn't mean you have access to all the functions in that module. It just means that module is publically available. You might have less luck with your espresso() function.

One interesting thing to note is that with structs each field needs to be declared public, whereas with enums if you declare the enum public then all of its variants are public.

## The use Keyword

Typing out the full path for whatever things you are using from a module is tedious, but the `use` keyword lets you abbreviate what you are trying to do. This concept should be familiar to anyone that has worked in C++.

```
// modules stuff

use crate::drinks::coffee;

fn main() {
    coffee::macchiato();
}
```

For relative paths the syntax changes slightly.

`use self::drinks::coffee;`

Note that this implementation might change in future versions of Rust. Also note that it is considered good manners to include some sort of module reference for functions. In other words, this is bad:

```
// modules stuff

use crate::drinks::coffee::macchiato;

fn main() {
    macchiato();
}
```

This code works but is bad manners because you are hiding away the fact that you are using a non-local function. This rule doesn't apply when you are importing data structures like structs, enums, and hash maps. There isn't some strong principle to any of this, it's just how the Rust communtiy ended up writing code.

You can also rename things you import from modules.

`use std::io::Result as IoResult;`

### use and Public scope

When you use a name in the current scope, the available name becomes private, so if you want to make it available elsewhere you need to use the `pub` keyword for that as well.

```
mod sound {
    pub mod instrument {
        pub fn clarinet() {
            // Function body code goes here
        }
    }
}

mod performance_group {
    pub use crate::sound::instrument;

    pub fn clarinet_trio() {
        instrument::clarinet();
        instrument::clarinet();
        instrument::clarinet();
    }
}

fn main() {
    performance_group::clarinet_trio();
    performance_group::instrument::clarinet();
}

```

In the examples so far the different modules are all in the same `main.rs` file, but the real advantage of modules of course is you can break up a file into smaller files. Amazingly, the code really doesn't look much different when you do that.

```
mod drink;

fn main() {
    crate::drink::coffee::macchiato();
}
```
