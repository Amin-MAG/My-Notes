# Cargo 

## Create new project

```bash
cargo new hello_world
```

in the `src/main.rs`

```rust
fn main() {
	println!("Hello, world!");
}
```

## Build & Run a project

To generate the binary file in the `./target/debug/`

```bash
cargo build
cargo run # To build and run in one step
```