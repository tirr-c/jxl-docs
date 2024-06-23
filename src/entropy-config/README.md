# Reading Entropy Decoder Configuration

This chapter describes how to read struct `EntropyDecoder` from a bitstream.

`read!(struct EntropyDecoder(num_dist, lz77_allowed))` reads *entropy decoder configuration*, in the
following order:

1. (Optional) LZ77 stream configuration.
1. Distribution clustering.
1. Which entropy coding type to use (prefix code or ANS).
1. Hybrid integer configuration for each post-clustered distribution.
1. Post-clustered symbol probability distributions.

Subchapters will explain each topic in detail.

Also, `EntropyDecoder` has a helper method `parse`:

```
/// Read an entropy decoder configuration with LZ77 allowed.
pub fn EntropyDecoder::parse(num_dist: u32) -> EntropyDecoder {
    read!(struct EntropyDecoder(num_dist, true))
}
```
