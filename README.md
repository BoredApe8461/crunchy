# Crunchy

The crunchy unroller - deterministically unroll constant loops. For number
"crunching".

The Rust optimizer will unroll constant loops that don't use the loop variable,
like this:

```rust
for _ in 0..100 {
  println!("Hello!");
}
```

However, using the loop variable will cause it to never unroll the loop. This is
unfortunate because it means that you can't constant-fold the loop variable, and
if you end up stomping on the registers it will have to do a load for each
iteration. This crate ensures that your code is unrolled and const-folded. It
only works on literals, unfortunately, but there's a work-around:

```rust
debug_assert_eq!(MY_CONSTANT, 100);
unroll! {
  for i in 0..100 {
    println!("Iteration {}", i);
  }
}
```

This means that your tests will catch if you redefine the constant.
