# Map Internals Deep Dive

## Hash Table Structure

Since Go 1.24, maps are Swiss tables. A map is a directory of tables; each table is an open-addressed array of groups, and each group holds 8 key/value slots plus an 8-byte control word:

- **Control word** — one byte per slot: empty, deleted (tombstone), or the low 7 bits of the key's hash (H2)
- **Lookup** — the upper 57 bits of the hash (H1) pick the starting group; the 8 control bytes are compared against H2 at once (SIMD on amd64) before any full key comparison, and probing moves to the next group (quadratic probing) until it reaches a group with an empty slot
- **Small maps** — a map with at most 8 entries is a single group, with no table or directory

## Memory Growth and Capacity

- **Load factor threshold**: a table grows once it is 7/8 full
- **Growth is per table**: a table doubles until it holds 1024 slots; past that it splits in two, and the directory (indexed by the high bits of the hash, i.e. extendible hashing) grows instead. Each grow rehashes a single table, which bounds the cost of any one insert
- **No overflow chains**: collisions are resolved by probing within the table
- **Deletes leave tombstones** in full groups; inserts reuse them and a grow clears them
- **No `cap()` function**: Capacity depends on hash distribution and load factor, not a fixed limit. Preallocation (`make(map[string]int, expectedSize)`) is worthwhile for large maps to avoid repeated growth cycles

## Preallocation

```go
// Without preallocation — multiple growths as entries are added
m := map[string]int{}

// With preallocation — allocates enough buckets upfront
m := make(map[string]int, expectedSize)
```

Preallocation avoids repeated growths. The hint is approximate: Go sizes the tables so the hint fits under the 7/8 load factor.

## Pointers vs Values

For large value types, storing pointers reduces copy overhead:

```go
// Large struct — copied on every read/write
m := map[string]BigStruct{}  // copies large struct

// Pointer — only pointer is copied
m := map[string]*BigStruct{} // copies 8-byte pointer
```

Trade-off: pointer maps add GC pressure. For small structs (< 128 bytes), value maps are typically faster.

## `maps` Package (Go 1.21+)

| Function | Description |
| --- | --- |
| `Clone`, `Equal`, `EqualFunc` | Shallow copy and equality comparison |
| `Keys`, `Values`, `All` (1.23+) | Iterators over keys, values, or pairs |
| `Collect`, `Insert` (1.23+) | Build maps from iterators or insert entries |

See `lucas-varjao/golang-skills@golang-safety` skill for `Clone`, `Equal`, and sorted iteration patterns.

## Map Key Requirements

Map keys must be comparable (`==` must work). This includes:

- All numeric types, `string`, `bool`
- Pointers, channels, interfaces (compared by identity)
- Arrays of comparable types
- Structs where all fields are comparable

Slices, maps, and functions **cannot** be map keys.
