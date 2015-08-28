---
layout: post
title:  "Calling Rust from Ruby"
date:   2015-08-27 21:11:51
categories: Rust Ruby
---
I've falling in love with [Rust][Rust] over the last year and a half.  I like it
for a lot of reasons, but I've recently been thinking through ways to introduce
it to the team and the technology portfolio at [Fooda][Fooda].

One idea we had, was move some of our computational intensive tasks to [Rust][Rust]
given that is one of it's "sweet spots".  However, this left us wondering on how exactly
to do that.  Specifically, how do we call [Rust][Rust] from within Ruby.

As it turns out, it's actually fairly easy so long as you understand

- Rust
- C (the runtime language of Ruby)
- FFI

and a few lines of code to begin calling Rust from within your Ruby code.

# Preparing Rust to be "callable" from Ruby
Without going to deep, you'll need to do a few things to make your code available to C.  looking at
the code snippet below, you'll see a couple things different from the typical [Rust][Rust] function.

- `No mangle`  This tells the [Rust][Rust] compiler to keep the function name as is post compilation
- `extern "C"`

{% gist e3e99f733add2830f89a %}


[Fooda]:    http://www.fooda.com
[Rust]:     https://www.rust-lang.org
