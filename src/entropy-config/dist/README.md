# Symbol Probability Distributions

```
// struct EntropyDecoder(num_dist: u32, lz77_allowed: bool) {
// ...

pub let mut dists = Vec::new();
for _ in 0..num_clusters {
    let dist = if use_prefix_code {
        read!(struct PrefixDist(log_alphabet_size))
    } else {
        read!(struct AnsDist(log_alphabet_size))
    };

    dists.push(dist);
}

// ...
```

The decoder reads a set of prefix code distribution or ANS distribution, depending on the flag
`use_prefix_code` [read previously](../coding-types.md).
