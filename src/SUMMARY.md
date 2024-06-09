[Introduction](./README.md)

# Bitstream Syntax

- [Reading a Bitstream](./bitstream/README.md)
  - [Bitstream Operations](./bitstream/operations.md)
  - [Structs and Enums](./bitstream/struct-enum.md)
  - [Reading Extensions]()
- [Container Format]()
  - [JPEG XL Codestream Boxes]()

# Entropy Decoding

- [Reading Entropy Decoder Configuration]()
  - [LZ77 Stream]()
  - [Symbol Probability Distributions]()
    - [Prefix Code]()
    - [Asymmetric Numeral System]()
  - [Distribution Clustering]()
  - [Hybrid Integer]()
- [Decoding Entropy-coded Stream]()
  - [Entropy-coded Symbol]()
  - [Hybrid Integer]()
  - [LZ77-enabled Stream]()

# Image Header

- [Image Header]()
  - [struct `ImageHeaders`]()
  - [struct `ImageSize`]()
  - [struct `ImageMetadata`]()
- [Color Encoding]()
  - [struct `ColorEncoding`]()
  - [struct `ToneMapping`]()
  - [struct `OpsinInverseMatrix`]()
  - [Decoding ICC Profile]()
