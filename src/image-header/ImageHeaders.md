# struct `ImageHeaders`

```
struct ImageHeaders {
    let signature = read!(u(16));
    assert!(signature, 0x0aff); // ff 0a

    pub let size = read!(struct ImageSize);
    pub let metadata = read!(struct ImageMetadata);

    // Optional ICC profile, if the color encoding requires it
    pub let embedded_icc = if metadata.color_encoding.has_embedded_icc {
        Some(read_icc_profile())
    } else {
        None
    };
}
```

`ImageHeaders` is quite straightforward. It reads magic signature (`ff 0a`), [dimension], metadata,
and optional ICC profile. Note that ICC profile is read only when required.

[dimension]: ./ImageSize.md
