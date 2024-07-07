# struct `ImageSize`

```
struct ImageSize {
    let is_multiple_of_8 = read!(Bool);
    pub let height = if is_multiple_of_8 {
        let height_div8 = 1 + read!(u(5));
        height_div8 * 8
    } else {
        1 + read!(U32(u(9), u(13), u(18), u(30)))
    };

    // Aspect ratio
    let ratio = read!(u(3));
    pub let width = match ratio {
        0 => {
            if is_multiple_of_8 {
                let width_div8 = 1 + read!(u(5));
                width_div8 * 8
            } else {
                1 + read!(U32(u(9), u(13), u(18), u(30)))
            }
        }
        1 => height,
        2 => height * 6 / 5,
        3 => height * 4 / 3,
        4 => height * 3 / 2,
        5 => height * 16 / 9,
        6 => height * 5 / 4,
        7 => height * 2,
    };
}
```

`ImageSize` uses some technique to encode common image dimensions more densely.
- Most images has some kind of well-known aspect ratio. JPEG XL skips encoding the width if aspect
  ratio is in the set of well-known ratio: 1:1, 6:5, 4:3, 3:2, 16:9, 5:4, 2:1.
- For small images up to 256x256, it can use more compact encoding if both sides are multiple of 8.
  Exact conditions may differ if aspect ratio is taken into account.

For example, encoding `3840x2160` takes 19 bits:
- Height is multiple of 8, but it's not small enough. `is_multiple_of_8 = false`. Takes 1 bit.
- For encoding height, 13-bit variant is used since encoding `2160` requires at least 12 bits. Takes
  2 + 13 = 15 bits.
- Aspect ratio is 16:9, which is well-known. Takes 3 bits.

Note that width or height cannot be zero.
