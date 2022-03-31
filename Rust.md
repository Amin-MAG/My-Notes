# Rust

It is a choice for systems where performance is critical. For example, game engines, databases, or Operating Systems. Rust doesn’t have a garbage collector.

Hello world in Rust

```rust
fn main() {
	let mut hello: Vec<i32> = (0..10).collect();

	fn do_stuff(val: &Vec<i32>) {
		println("{}", val.len());
	}

	do_stuff(&hello);
}
```

To compile the code

```bash
rustc main.rs
```

The variables are immutable by default and it allows them to be used in stack memory that has the minimum performance overhead.

```rust
let hello = "hi mom"
```

By using `mut` keyword, Mutable variables will be stored  in heap.

## Package Manager

Cargo is the Rust package manager.

# References

[Rust in 100 Seconds](https://www.youtube.com/watch?v=5C_HPTJg5ek)