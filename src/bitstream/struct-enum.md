# Structs and Enums

## Structs

Struct is a collection of fields, defined with how to parse them from a bitstream. Struct
definitions look like this:

```
struct ImageHeaders {
    let signature = read!(u(16));
    assert!(signature == 0x0aff);

    pub let size: ImageSize = read!(struct ImageSize);
    pub let metadata: ImageMetadata = read!(struct ImageMetadata);
}
```

- `let variable` makes a temporary variable used within struct definition.
- `pub let field: Type` defines a public field of struct.
- `assert!(condition)` checks whether the given condition holds true.

### Parse context

Structs can optionally receive *context* used during parsing:

```
struct FrameHeader(image_headers: ImageHeaders) {
    // ...
    pub let do_ycbcr: bool = if image_headers.metadata.xyb_encoded {
        false
    } else {
        read!(Bool)
    };
    // ...
}

read!(struct FrameHeader(headers));
```

### Default implementation

Structs typically have *default implementation*. They are marked with `default struct` and invoked
with `Ty::default(context, ...)`.

```
struct ImageSize {
    // ...
    pub let height: u32 = /* ... */;
    pub let width: u32 = /* ... */;
    // ...
}

default struct ImageSize {
    pub let height: u32 = 0;
    pub let width: u32 = 0;
}

default struct ImageMetadata {
    // ...
    pub let intrinsic_size: ImageSize = ImageSize::default();
    // ...
}
```

Default implementation of a struct defines the same set of fields as in normal struct definition and
doesn't have any bitstream operation.

## Enums
