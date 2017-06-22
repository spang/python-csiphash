csiphash.py
===========

[SipHash][] is a hash function and message authentication code that is secure,
fast and simple. It accepts a 128-bit secret key and a variable-length message,
and returns a 64- or 128-bit hash. It performs better than most cryptographic
hash function-based MACs (especially for short inputs), and offers much better
security than non-cryptographic hash functions, providing resistance against
hash-flooding DoS attacks. As a result, it is now used as the hash function of
choice for hash tables in Python, Ruby, Rust and Redis.

This Python package is the SipHash-2-4 [C reference implementation][siphash-c],
wrapped with [CFFI][]. It's incredibly fast: amortized hashing times tested as
follows on a mid-2015 MacBook Pro:

| Message Size | Time per Hash (Amortized over 1M) |
| ------------ | --------------------------------- |
| 43 bytes     | 3.17 microseconds                 |
| 256 bytes    | 3.39 microseconds                 |
| 1500 bytes   | 4.40 microseconds                 |
| 1 MiB        | 0.74 milliseconds                 |

  [siphash]: https://131002.net/siphash/
  [siphash-c]: https://github.com/veorq/SipHash
  [cffi]: http://cffi.readthedocs.io


Installation
------------

You can get this library with pip:

    pip install csiphash


Usage
-----

Currently there's just a single function, `siphash24()`, that accepts a secret
key bytestring of length 16 and an arbitrary length bytestring for the message,
and returns an 8-byte digest bytestring:

```pycon
>>> from csiphash import siphash24
>>> siphash24(b'\x00' * 16, b'hello, world!\n')
b'\xf1G4\x95\xa5\xaa\xc2b'
```

If you want to specify keys in hexadecimal, use `.decode('hex')`:

```pycon
>>> siphash24('abcdef01234567899876543210fedcba'.decode('hex'), b'hello, world!\n')
'\xd3\xd4N\x1dk\x1f$='
```

If you want digests in hexadecimal, use `.encode('hex')`:

```pycon
>>> siphash24(b'\x00' * 16, b'hello, world!\n').encode('hex')
'f1473495a5aac262'
```

If you want 128-bit output, use ``output_bytes=16``::

```pycon
>>> siphash24(b'\x00' * 16, b'hello, world!\n', output_bytes=16)
'\x8a\x03uf\x0b\x02\x06=\xfdI\x0fs\xb1\x14a&'
```

License
-------

The reference C implementation of SipHash is bundled with this library. It was
written by Jean-Philippe Aumasson and Daniel J. Bernstein, and is released
under the [CC0][].

[Six][] is bundled with this library. It was written by Benjamin Peterson, and
is licensed under the [MIT License][].

All other software in this library is released under the [UNLICENSE][]:

> This is free and unencumbered software released into the public domain.
>
> Anyone is free to copy, modify, publish, use, compile, sell, or
> distribute this software, either in source code form or as a compiled
> binary, for any purpose, commercial or non-commercial, and by any
> means.
>
> In jurisdictions that recognize copyright laws, the author or authors
> of this software dedicate any and all copyright interest in the
> software to the public domain. We make this dedication for the benefit
> of the public at large and to the detriment of our heirs and
> successors. We intend this dedication to be an overt act of
> relinquishment in perpetuity of all present and future rights to this
> software under copyright law.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
> EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
> MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
> IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
> OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
> ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
> OTHER DEALINGS IN THE SOFTWARE.
>
> For more information, please refer to <http://unlicense.org/>

  [cc0]: https://creativecommons.org/publicdomain/zero/1.0/
  [six]: https://pythonhosted.org/six/
  [mit license]: https://bitbucket.org/gutworth/six/raw/ca4580a5a648fc75abc568907e81abc80b05d58c/LICENSE
  [unlicense]: https://unlicense.org/
