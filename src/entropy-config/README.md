# Reading Entropy Decoder Configuration

This chapter describes how to read struct `EntropyDecoder` from a bitstream.

When pseudocode says `EntropyDecoder::parse(num_dist)`, it reads *entropy decoder configuration*, in
following order:

1. (Optional) LZ77 stream configuration.
1. Distribution clustering.
1. Which entropy coding type to use (prefix code or ANS).
1. Hybrid integer configuration for each post-clustered distribution.
1. Post-clustered symbol probability distributions.

Subchapters will explain each topic in detail.
