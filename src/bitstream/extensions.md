# Reading Extensions

JPEG XL defines a special struct called `Extensions` in order to provide forward compatibility.
Extension may appear as a field in some struct definitions.

```
struct Extensions {
    // This implies that there are at most 64 extensions.
    let active_extension_bitset = read!(U64);

    let mut extension_bit_length = Vec::new();
    for extension_id in 0..64 {
        let mask = 1 << extension_id;
        if active_extension_bitset & mask != 0 {
            // Extension with the ID `extension_id` is active.
            let bit_length = read!(U64);
            extension_bit_length.push((extension_id, bit_length));
        }
    }

    // Read data for each active extension.
    pub let mut extension_data = HashMap::new();
    for (extension_id, bit_length) in extension_bit_length {
        let bits = read_bits_to_bitstream!(bit_length);
        extension_data.insert(extension_id, bit_length);
    }
}
```

In this definition, `read_bits_to_bitstream!(N)` reads `N` bits from the bitstream and saves those
bits as another bitstream.
