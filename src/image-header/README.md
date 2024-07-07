# Image Header

Image header is at the very beginning of a JPEG XL codestream. It contains the so-called magic
signature, dimension (width and height) of the image, what color space the image is in, how many
channels are in it, the (preferred or actual) bit depth, and so on. Basically it describes global
attributes that apply to the whole image.

Decoder reads [`ImageHeaders`] at the beginning of bitstream. It contains 16-bit signature,
[`ImageSize`], and `ImageMetadata`. Following subchapters will explain those in detail.

[`ImageHeaders`]: ./ImageHeaders.md
[`ImageSize`]: ./ImageSize.md
