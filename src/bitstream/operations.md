# Bitstream Operations

Thourghout this book, `read!(Operation)` means "read bitstream as specified in `Operation`".
`Operation` can be one of the following:

- `u(N)`: read `N` bits as an integer. If `N == 0`, the result is zero.
- `U32(d0, d1, d2, d3)`:
  ```
  match read!(u(2)) {
      0 => read_u32!(d0),
      1 => read_u32!(d1),
      2 => read_u32!(d2),
      3 => read_u32!(d3),
  }

  read_u32!(M)        := M as u32
  read_u32!(u(N))     := read!(N) as u32
  read_u32!(M + u(N)) := (M + read!(N)) as u32
  ```
- `Bool`: `read!(u(1)) != 0`
- `U64`:
  ```
  match read!(u(2)) {
      0 => 0u64,
      1 => 1 + read!(u(4)) as u64,
      2 => 17 + read!(u(8)) as u64,
      3 => {
          let mut value = read!(u(12));
          let mut shift = 12;
          while read!(Bool) {
              if shift == 60 {
                  value |= read!(u(4)) << shift;
                  break;
              }
              value |= read!(u(8)) << shift;
              shift += 8;
          }
          value as u64
      }
  }
  ```
- `F16`: `f16::from_bits(read!(u(16)))`. Assert that the value is not `NaN` or infinities. \
  Most programming languages don't have half precision floats; you might need to convert bits to
  single precision float first.
- `struct Ty(name = value, ...)`, `enum Ty`: See [Structs and Enums](./struct-enum.md).
