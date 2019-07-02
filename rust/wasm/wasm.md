# WebAssembly

WebAssembly (WASM) is an executable format that is comprised of two formats: a text format, `.wat` files, and a `.wasm` binary format that uses a linear memory format.

## Setup and Configuration

Unfortunately, WebAssembly requires quite a bit of setup.

Use of WebAssembly in Rust requires v 1.30 or higher, `wasm-pack`, and npm.

In your *Cargo.toml* file you need the following specifications:

```
[lib]
crate-type = ["cdylib", "rlib"]

[dependencies]
cfg-if = "0.1.2"
wasm-bindgen = "0.2"
```

The `src/lib.rs` file is treated as the root file, and it utilizes code with the `#[wasm_bindgen]` attribute to interface with JS.

The `wasm-pack build` CLI will create a `pkg` directory that contains a `package.json` file, the `.wasm` file, and some generated TypeScript and JavaScript.

To build the web page you need to use npm.

`npm init wasm-app www`

In the newly created `www/` directory you will then need to run `npm install` to install necessary dependencies.

Then, back in the `pkg` directory you should run `npm link` so that the WASM package can be used as a dependency instead. Then back in the `www/` directory you run `npm link {package name}`.

Next, the `www/index.js` file should be modified to import the wasm file.

Finally, you can run your page locally with `npm run start`

Any changes that are made require you to rerun the `wasm-pack build` command.
