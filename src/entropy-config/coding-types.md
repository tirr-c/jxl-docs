# Entropy Coding Types

JPEG XL supports two types of entropy coding methods: prefix code, and asymmetric numeral systems.

## Prefix Code

Prefix code reduces the amount of encoded bits by assigning shorter code to more frequent symbols.
Each code can be as long as 15 bits. Distributions are parsed in the exact same way as [Brotli]
does; see [Prefix Code] chapter for details.

[Brotli]: https://www.rfc-editor.org/info/rfc7932

## Asymmetric Numeral Systems

Asymmetric numeral systems (ANS) achieves compression by encoding symbols into an integer using,
well, *asymmetric* numeral systems. Think of decimal numeral system we all know. In this system, we
have 10 symbols, 0 to 9, and each appended symbol increases the magnitude of value equally -- it's
symmetric. On the other hand, each symbol in ANS can affect the encoded value completely
asymmetrically; for example, in an extreme case where there are frequent symbol 0 and rare symbol 1,
symbol 0 could only increase the value by 0.02%, but the value would grow about 4096 times larger if
symbol 1 is appended.

JPEG XL uses streaming rANS variant with alias mapping; see [Asymmetric Numeral Systems] chapter for
details.
