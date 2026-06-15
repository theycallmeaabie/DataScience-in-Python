## Saving and Loading NumPy Arrays

NumPy provides efficient binary formats for saving and loading arrays.

### Three Functions, Three Use Cases

| Function | Arrays | Compressed | Extension |
|----------|---------|------------|-----------|
| `np.save()` | One | No | `.npy` |
| `np.savez()` | Many | No | `.npz` |
| `np.savez_compressed()` | Many | Yes | `.npz` |

---

## 1. Saving a Single Array (`.npy`)

Use `np.save()` when you need to store a single NumPy array.

```python
arr = np.arange(10)

np.save("some_array", arr)
```

This creates:

```text
some_array.npy
```

Load it back:

```python
np.load("some_array.npy")
```

Output:

```python
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

### Characteristics of `.npy`

- Stores one array.
- Binary format.
- Fast to save and load.
- No precision loss.
- Not human-readable.

---

## 2. Saving Multiple Arrays (`.npz`)

Use `np.savez()` to store multiple arrays in a single file.

```python
arr = np.arange(10)

np.savez(
    "array_archive.npz",
    a=arr,
    b=arr
)
```

Load the archive:

```python
arch = np.load("array_archive.npz")
```

Access arrays by key:

```python
arch["a"]
```

Output:

```python
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

```python
arch["b"]
```

Output:

```python
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

### Characteristics of `.npz`

- Stores multiple arrays.
- Dictionary-like access.
- Arrays are loaded lazily.
- No compression.

---

## Lazy Loading

When an `.npz` file is opened:

```python
arch = np.load("array_archive.npz")
```

The arrays are not immediately read into memory.

They are loaded only when accessed:

```python
arch["a"]
```

This can reduce memory usage when working with large archives.

---

## 3. Saving Multiple Arrays with Compression

Use `np.savez_compressed()` to reduce disk space.

```python
np.savez_compressed(
    "arrays_compressed.npz",
    a=arr,
    b=arr
)
```

Load exactly the same way:

```python
arch = np.load("arrays_compressed.npz")
```

```python
arch["a"]
```

### Characteristics

- Stores multiple arrays.
- Compresses data before writing.
- Smaller file size.
- Same interface as `np.savez()`.

---

## Compressed vs Uncompressed

### What Compression Does

Compression applies an algorithm that represents the data using fewer bytes on disk.

When loading:

```text
Compressed file
      ↓
Decompress
      ↓
Original NumPy array
```

The data remains exactly the same.

### Tradeoff

```text
Uncompressed → Fast save/load, Large file

Compressed   → Slower save/load, Smaller file
```

Compression trades CPU time for reduced storage.

---

## Binary vs Text Formats

### Text Format

```python
np.savetxt("arr.csv", arr)
```

Advantages:

- Human-readable.
- Easy to inspect manually.

Disadvantages:

- Larger files.
- Slower read/write.
- Possible precision loss.

---

### Binary Format

```python
np.save("arr.npy", arr)
```

Advantages:

- Fast.
- Exact representation.
- No precision loss.

Disadvantages:

- Not human-readable.

---

## When to Use What?

| Situation | Recommended Format |
|------------|-------------------|
| Single NumPy array | `.npy` |
| Multiple arrays | `.npz` |
| Multiple arrays with limited storage | `.npz` (compressed) |
| Human-readable data | CSV/Text |
| Tabular data with column names | Pandas (`to_csv`, `to_parquet`) |

---

## Key Points

- `.npy` stores a single array efficiently.
- `.npz` stores multiple arrays in one file.
- `.np.savez_compressed()` reduces file size using compression.
- Binary formats are faster and preserve exact values.
- Compression saves disk space but increases save/load time.
- For structured tabular datasets, Pandas formats are usually more appropriate than raw NumPy binaries.