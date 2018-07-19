Cargo is Rust's package manager. In addition to managing dependencies, it is the easiest way to get started writing and running scripts written in Rust. To start a new project type the following in the terminal:

`cargo new {project name} --bin`

This will create a cargo.toml file to manage the dependencies and other configurations, a .gitignore file, and a src directory where the code exists.

`cargo build` is used to compile an executable, and `cargo run` to run the executable. Use `cargo build --release` to create an optimized compilation. The trade-off is that the compiler will take longer to run.

Other useful cargo commands:

`cargo update` Updates dependencies
`cargo doc --open` Opens up documentation for dependencies
