---
layout: post
title:  "Calling Rust from Ruby"
date:   2015-08-27 21:11:51
categories: Rust Ruby
---
I've falling in love with [Rust][Rust] over the last year and a half.  I like it
for a lot of reasons, a few being the compiler, seperation of noun/verb,
and it's deeply integrated testing.   However, recently been thinking through ways
to introduce it to the team and the technology portfolio at [Fooda][Fooda].

One idea we had, was move some of our computational intensive tasks to [Rust][Rust]
given that is one of it's "sweet spots" as a technology.  However, this left us wondering
how exactly would we mix it in with Ruby.  Specifically, how do we call [Rust][Rust]
from within Ruby.

As it turns out, it's actually fairly easy to call [Rust][Rust] from within Ruby
so long as you understand a few simple things, namely

- Rust
- C and ABI
- FFI

With those, and a few lines of code to begin calling Rust from within your Ruby code.

# Preparing Rust to be "callable" from Ruby
Ultimately, you don't need to do a whole lot to make your [Rust][Rust] code available to C.  
In fact, here's a simple `hello world` example.

{% gist e3e99f733add2830f89a %}

With the code snippet above, you'll see a couple things different from your typical
[Rust][Rust] function.  So lets break it down ...

- `#[no_mangle]`  This tells the [Rust][Rust] compiler to expose this a global function
and don't mangle it with an internal name
- `extern "C"` This exposes the function as if it was a `C` function via the `C ABI`
- `*const libc::c_char` Our function returns a `C` "string"

Outside that, our function is a regular [Rust][Rust] function that does it's thing.

# Compile in a slightly different way
Now that your [Rust][Rust] code is exposed prepared properly, we need to ensure
we compile in a manner Ruby can accept, specifically a dynamic library. You can do this two ways, the command line or a Cargo toml file.  I'm doing it
through Cargo and a `toml` file below

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

When you execute `Cargo build` you will now have a dynamic library `.dylib` in your
targets folder.

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

ABC-194:rust-library anthony$ Cargo build
   Compiling libc v0.1.10
   Compiling library v0.1.0 (file:///Users/anthony/example_rust_called_from_ruby/rust-library)
ABC-194:rust-library anthony$ cd ..
ABC-194:example_rust_called_from_ruby anthony$ ls
.            ..           .git         ruby-library rust-library
ABC-194:example_rust_called_from_ruby anthony$ cd ruby-library/
ABC-194:ruby-library anthony$ ls
.              ..             ruby-client.rb
ABC-194:ruby-library anthony$ irb
2.0.0-p247 :001 > require "./ruby-client"
 => true
2.0.0-p247 :002 > Rust::hello_world
 => "hello world"
2.0.0-p247 :003 >



{% endhighlight %}

That's it.  I was incredibly surprised at how easy it was.  The only thing I don't like,
is maintaining that one-to-one mapping in via `ffi` in Ruby.  A small price to pay, I guess.

# Complete working example
I've put together a complete working example that will allow you from irb to call into
a [Rust][Rust] module.  It's a `hello world` but it works.

It's found [here](https://github.com/AnthonyBroadCrawford/example_rust_called_from_ruby)

[Fooda]:    http://www.fooda.com
[Rust]:     https://www.rust-lang.org
