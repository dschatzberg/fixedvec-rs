fixedvec
========

[![Build Status] (https://img.shields.io/travis/nastevens/fixedvec-rs.svg)](https://travis-ci.org/nastevens/fixedvec-rs)
[![Version](https://img.shields.io/crates/v/fixedvec.svg)](https://crates.io/crates/fixedvec)
[![License](https://img.shields.io/crates/l/fixedvec.svg)](https://github.com/nastevens/fixedvec-rs/blob/master/README.md#license)

- [API Documentation](http://nastevens.github.io/fixedvec-rs/)

`fixedvec` is a Rust library/crate providing a heapless version of the Rust
vector type. Although more limited than the libstd version, fixedvec provides a
much-needed "managed" array type for embedded systems or other projects that
cannot rely on the heap.

Install/Use
-----------

You must be using the nightly Rust release to use `fixedvec`, as it relies on
the unstable libcore feature. If you're building for an embedded system, you're
probably on the nightly anyway though.

To use `fixedvec`, add the following to your `Cargo.toml`:

```toml
[dependencies]
fixedvec = "*"
```

Then add the following to your crate root:

```rust
#[macro_use] extern crate fixedvec;
```

Example
-------

Buffering and mutating a list of bytes:

```rust
#![feature(core)]
extern crate core;

#[macro_use] extern crate fixedvec;

use fixedvec::FixedVec;

fn main() {
    let mut preallocated_space = alloc_stack!([u8; 10]);
    let mut vec = FixedVec::new(&mut preallocated_space);
    assert_eq!(vec.len(), 0);

    vec.push_all(&[1, 2, 3]).unwrap();
    assert_eq!(vec.len(), 3);
    assert_eq!(vec.as_slice(), &[1, 2, 3]);

    vec.map_in_place(|x: &mut u8| { *x *= 2 });
    assert_eq!(vec.as_slice(), &[2, 4, 6]);
}
```

License
-------

```
Copyright (c) 2015, Nick Stevens <nick@bitcurry.com>

The MIT License (MIT)

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
