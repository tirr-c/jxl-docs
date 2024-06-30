# Distribution Clustering

Entropy coding used by JPEG XL is *adaptive*, which means it has multiple distributions with varying
symbol probability, and chooses appropriate one depending on the current context (hence adaptive).
The number of contexts can be one or two, or as many as thousands. However we can't provide
distribution of every single context when there are thousands of those; it would be too costly.

Fortunately, distributions tend to form a cluster with similar symbol probability. Instead of
signalling every single distribution, JPEG XL first reduces the number of distributions using
*distribution clustering*.

```
// struct EntropyDecoder(num_dist: u32, lz77_allowed: bool) {
// ...

pub let cluster_map = read_clustering(num_dist);

// ...
```

## Reading cluster map

We define a function `read_clustering` which reads a cluster map:

```
pub fn read_clustering(num_dist: u32) -> Vec<u32> {
    if num_dist == 1 {
        // Trivial case; no need to cluster
        return vec![0];
    }

    let is_simple = read!(Bool);
    let cluster = if is_simple {
        read_simple(num_dist)
    } else {
        read_complex(num_dist)
    };
    validate_cluster(&cluster);
    cluster
}
```

Clustering is represented as a list of cluster indices. You can think it as a function; it maps
pre-clustering context `[0, num_dist)` to cluster index `[0, num_cluster)`, where `num_cluster` is
defined implicitly from the largest element of the list. There's no "hole" in the mapping, meaning
every integer in range `0..num_cluster` is present.

If `is_simple == true`, distribution clustering is in simple encoding.

```
fn read_simple(num_dist: u32) -> Vec<u32> {
    let mut cluster = Vec::new();
    let nbits = read!(u(2));
    for _ in 0..num_dist {
        cluster.push(read!(u(nbits)));
    }
    cluster
}
```

If `is_simple == false`, it's rather complex:

```
fn read_complex(num_dist: u32) -> Vec<u32> {
    let mut cluster = Vec::new();

    let use_move_to_front = read!(Bool);

    // Recursive entropy-coded stream
    let lz77_allowed = num_dist > 2;
    let mut decoder = read!(struct EntropyDecoder(1, lz77_allowed));
    decoder.init();
    for _ in 0..num_dist {
        cluster.push(decoder.read_uint(0));
    }
    decoder.validate();

    if use_move_to_front {
        // Move-to-front encoding
        inverse_move_to_front(&mut cluster);
    }
    cluster
}
```

Note that we're reading another entropy-coded stream recursively. `lz77_allowed` is there to prevent
additional recursion; if we allowed LZ77 there, the number of distribution would increase to 2,
which might trigger another recursive entropy-coded stream.

### Move-to-front encoding

Complex cluster encoding may optionally use *move-to-front encoding*. It keeps the most recently
used indices in lower value.

```
fn inverse_move_to_front(encoded_map: &mut [u32]) {
    let mut mru: Vec<u32> = (0..256).collect();
    for slot in encoded_map {
        let idx = *slot as usize;
        assert!(idx < 256);

        let cluster_idx = mru[idx];
        *slot = cluster_idx;

        if idx > 0 {
            // Move last used index to front
            mru.copy_within(0..idx, 1);
            mru[0] = cluster_idx;
        }
    }
}
```

This helps encoding cluster maps with runs of same index.

### Cluster map validation

Finally, we validate that there's no hole in the cluster map:

```
fn validate_cluster(cluster_map: &[u32]) {
    let num_clusters = *cluster_map.iter().max().unwrap() + 1;
    let cluster_indices: HashSet<_> = cluster_map.iter().copied().collect();
    assert_eq!(num_clusters, cluster_indices.len() as u32);
}
```
