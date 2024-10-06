# Hybrid Integer

Hybrid integer is a mechanism to insert raw, non-entropy-coded bits into entropy decoded symbol.
It can be useful when there are some bits that can't be encoded efficiently using entropy coding,
e.g. the signal is noisy and have almost equal probability in a range of value.

```
struct HybridIntegerConfig(log_alphabet_size: u32) {
    let exp_bits = add_one_log2_ceil(log_alphabet_size);
    pub let split_exponent = read!(u(exp_bits));

    pub let (msb_in_token, lsb_in_token) = if split_exponent == log_alphabet_size {
        // Special case: no raw bits.
        (0, 0)
    } else {
        let msb_bits = add_one_log2_ceil(split_exponent);
        let msb_in_token = read!(u(msb_bits));
        assert!(msb_in_token <= split_exponent);

        let lsb_bits = add_one_log2_ceil(split_exponent - msb_in_token);
        let lsb_in_token = read!(u(lsb_bits));
        assert!(msb_in_token + lsb_in_token <= split_exponent);

        (msb_in_token, lsb_in_token)
    };
}

/// The number of bits needed to read a value up to `x`.
fn add_one_log2_ceil(x: u32) -> u32 {
    // NOTE: This can be done using integer types only.
    (x + 1).log2().ceil()
}
```

Hybrid integer config splits symbol ("token") from entropy coded stream into three parts if the
symbol is larger than or equal to `1 << split_exponent`.

```
+-----------------------+
|         token         |
+---------+-------+-----+
| raw_exp |  msb  | lsb |
+---------+-------+-----+
     ^- split_exponent -^
```

Then it reads `raw_bits` according to `raw_exp` value, and constructs new symbol value.

```
+-+-------+------------+-----+
|1|  msb  |  raw_bits  | lsb |
+-+-------+------------+-----+
```

See [dedicated chapter] for details.
