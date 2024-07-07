[Introduction](./README.md)

# Bitstream Syntax

- [Reading a Bitstream](./bitstream/README.md)
  - [Bitstream Operations](./bitstream/operations.md)
  - [Structs and Enums](./bitstream/struct-enum.md)
  - [Reading Extensions](./bitstream/extensions.md)
- [Container Format]()
  - [JPEG XL Codestream Boxes]()
- [Codestream Overview]()

# Image Header

- [Image Header](./image-header/README.md)
  - [struct `ImageHeaders`](./image-header/ImageHeaders.md)
  - [struct `ImageSize`](./image-header/ImageSize.md)
  - [struct `ImageMetadata`]()

# Entropy Decoding

- [Reading Entropy Decoder Configuration](./entropy-config/README.md)
  - [LZ77 Stream](./entropy-config/lz77.md)
  - [Distribution Clustering](./entropy-config/clustering.md)
  - [Entropy Coding Types](./entropy-config/coding-types.md)
  - [Hybrid Integer](./entropy-config/hybrid-integer.md)
  - [Symbol Probability Distributions]()
    - [Prefix Code]()
    - [Asymmetric Numeral System]()
- [Decoding Entropy-coded Stream]()
  - [Entropy-coded Symbol]()
  - [Hybrid Integer]()
  - [LZ77-enabled Stream]()

# Color Encoding

- [Color Encoding]()
  - [struct `ColorEncoding`]()
  - [struct `ToneMapping`]()
  - [struct `OpsinInverseMatrix`]()
  - [Decoding ICC Profile]()
