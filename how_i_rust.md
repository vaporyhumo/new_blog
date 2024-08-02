# Rust

<h2><code> rustfmt </code></h2>

```toml
group_imports = "One"
hex_literal_case = "Upper"
imports_granularity = "One"
match_block_trailing_comma = true
reorder_impl_items = true
tab_spaces = 2
trailing_comma = "Always"
use_field_init_shorthand = true
```

## Clippy
https://rust-lang.github.io/rust-clippy/master/index.html

```rust
#![warn(clippy::complexity)]
#![warn(clippy::expect_used)]
#![warn(clippy::nursery)]
#![warn(clippy::panic)]
#![warn(clippy::pedantic)]
#![warn(clippy::perf)]
#![warn(clippy::unwrap_used)]
```

```
cargo clippy --message-format short
```


## Unit tests

## Test coverage

https://github.com/xd009642/tarpaulin

Use `tarpaulin`. Install with `cargo install tarpauling`, use with `cargo
tarpaulin --out Html --fail-under 100`.
