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
Ultimately, you don't need to do a whole lot to make your code available to C.  In fact,
here's a simple `hello world` example.

{% gist e3e99f733add2830f89a %}

The code snippet above, you'll see a couple things different from the typical
[Rust][Rust] function.

- `#[no_mangle]`  This tells the [Rust][Rust] compiler to make this a global function and don't
mangle it with an internal name
- `extern "C"` This exposes the function as if it was a `C` function via the `C ABI`
- `*const libc::c_char` Our function returns a `C` "string"

Outside that, our function is a regular [Rust][Rust] function that does it's thing.

# Compile in a slightly different way
Now that the source code is ready to be exposed, we need to ensure our binary is compiled
in a manner Ruby can accept, specifically a dynamic library.  You can do this two ways, below
I'm doing it through Cargo and the `toml` file.

{% highlight toml %}

[package]
name = "library"
version = "0.1.0"
authors = ["Anthony Broad-Crawford <anthony@anthonybroadcrawford.com>"]

[lib]
crate-type = ["dylib"]

[dependencies]
libc = "0.1.10"

{% endhighlight %}

# Including your Rust library in Ruby
If there's an icky part to this whole process, this is it right here.  You'll need a one-to-one
mapping of the functionality you expose in your [Rust][Rust] dynamic library to your `Ruby` application.

Here's an example of how this is done leveraging `ffi`

{% highlight ruby %}

require 'ffi'

module Rust
  extend FFI::Library
  ffi_lib '../rust-library/target/debug/liblibrary.dylib'

  attach_function :hello_world, [], :string
end

{% endhighlight %}

Please note, the `path` reference above in the `ffi_lib` method call is specific to a sample project I've
included below.

# Test drive in IRB
At this point you are able to take it for a test drive in irb.  

{% highlight bash %}



{% endhighlight %}

# Complete working example
I've put together a complete working example that will allow you from irb to call into
a [Rust][Rust] module.  It's a `hello world` but it works.

It's found [here](https://github.com/AnthonyBroadCrawford/example_rust_called_from_ruby)

[Fooda]:    http://www.fooda.com
[Rust]:     https://www.rust-lang.org
