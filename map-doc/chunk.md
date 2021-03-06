# Concepts
## Storage
- Chunk Data: an array, either 2048 or 4096 bytes
- Chunk: a 16x16x16 area, logically made up of multiple Chunk Data arrays, storing things like block ids and skylight
- Chunk Column: 16 chunks aligned vertically (totalling 16x256x16). Chunks can be uninitialised (e.g. set to null)
- World: a collection of chunk columns

## Bitmasks
You will sometimes get a blob of data and a bitmask. Each bit in the 16-bit short represents a chunk. The least significant bit represents the chunk from Y=0 to Y=15, and so forth. If it's 0, the chunk is entirely air and there's no data to read. You need a loop like this:

```
chunks = array[16];
for (i=0; i<16; i++) {
    if (bitmask & (1 << i)) {
        // read a chunk data array
    }
}
```

# Format
## Chunk Column Metadata
The packet contains important metadata which you'll need to pull out. For bulk packets, you'll need to loop over the metadata area. Specifically:
- Chunk X
- Chunk Z
- Section Bitmask
- Continuous? (only in 0x21 - assumed true in 0x26)
- Skylight? (only in 0x26 - only if overworld in 0x21)

## Data
In 0x21 the data describes of a single chunk column in the format below. In 0x26, the format below is repeated for each chunk column.
- Repeat for each chunk specified in the section bitmask:
  - Block types, two bytes per block (LE, type = x >> 4, meta = x & 15)
  - Block light, half byte per block
  - If skylight: Sky light array, half byte per block

- If continuous: biome array, byte per XZ coordinate, 256 bytes total.

Each section (except for biome) contains packed chunk data for zero or more 16x16x16 chunks. You need to refer to the primary bitmask for the chunk column to see which chunks are sent.

Chunks are sent bottom-to-top, i.e. the first chunk, if sent, extends from Y=0 to Y=15. Blocks are ordered Y, Z, X, i.e. the 'X' coordinate changes fastest.

In half-byte arrays, two values are packed into each byte. Even-indexed items are packed into the high bits, odd-indexed into the low bits.

The 'biome' array is always 256 bytes when sent. Values above 22 will cause the client to crash.
