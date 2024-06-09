# Reading a Bitstream

A bitstream is, well, a stream of bits, though typically bitstreams are understood as series of
bytes (that is, chunks of 8 bits).

```
ff 0a 7a 43 05 3b 01 00 ...
```

We need to unpack these bytes into bits. Let's write those in binary form:

```
11111111 00001010 01111010 01000011
00000101 00111011 00000001 00000000
...
```

Bitstreams are read *in byte order, lowest bits first*. So if we read 12 bits from the beginning, we
get `1 1 1 1` `1 1 1 1` `0 1 0 1`.

```
  1111 1111
.<---------- read in this direction
|___________
  0000 1010 |
.<---------- then this
| and so on...
```

But there's a twist: if we read multiple bits *at once* as an integer, bits read first become less
significant bits. Which means, in previous example, it's `0b_1010_1111_1111` as an integer, not
`0b_1111_1111_0101`. If we were to read three 4-bit integer, we would get `0b1111`, `0b1111`, and
`0b1010`.

This sounds a little bit confusing (pun not intended), but it has some advantage on modern CPUs.
Think about this: when we have `ff 0a`, what will happen if we read 16-bit integer? We will get
`0b_00001010_11111111`, or `0x0aff`. Sounds familiar? Yeah, we've read bytes in little endian order.
So bitstreams can be read like: load an integer in little endian order, do some bitwise operations,
and done. libjxl does it in (mostly) branchless way, and it's quite fast.
