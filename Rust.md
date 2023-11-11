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

By using `mut` keyword, Mutable variables will be stored in the heap.

## Read user input

You can use the `std::io` to scan the `stdin`.

```rust
use std::io;

fn main() {
    // Read user input
    println!("Enter your name:");
    let mut input_text = String::new();
    io::stdin().read_line(&mut input_text).expect("Failed to read line");
    let name = input_text.trim();

    // Display the input
    println!("Hello, {}! Nice to meet you!", name);
}
```

# See More

- [Cargo, the Rust package manager](Rust/Cargo.md)

# References

[Rust in 100 Seconds](https://www.youtube.com/watch?v=5C_HPTJg5ek)