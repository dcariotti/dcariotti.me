---
title: first code in Rust 
date: 2020-04-13
---

Today I woke up with a thought: learn Rust. I bought a book about 3 months ago and I started to read it, read code on GitHub etc but I never wrote a code by myself.
After 1 hour (yeah, I suck) I implemented a fibonacci sequence. I think I used every Rust tips, but idk, It's just an augury.

<img src="https://media.tenor.com/images/b58a6eb5e09d1c9d99151d90db671217/tenor.gif" alt="kinda suck" class="center">

---
This is the bad code I wrote but I'm very proud of it.

```rust
use std::mem;

fn fib(n: &mut i32) -> i32 {
    let mut a = 1;
    let mut b = 1;
    while *n > 1 {
        a += b;
        mem::swap(&mut a, &mut b);
        *n -= 1;
    }

    b
}
```

<p>This prints the first <span>\(N\)</span> numbers from the fibonacci code.</p>

```rust
println!("{}", fib(&mut N));
```

Maybe one day I'll see again this code and I will thought:"WTF is that?!".

<img src="https://media.tenor.com/images/a8db208ebc4a1cfb5390c26e676c34de/tenor.gif" alt="wtf" class="center">

