# Storage Footprint Analysis: Hexadecimal vs. Base-12 Encoding

This document evaluates the storage efficiency and overhead vectors between `output.bin`, `output.hex`, and `output.twelve`.

## Storage Footprint Metrics Comparison

| File Asset | Encoding Format Type | Storage Efficiency Factor | Relative Size Ratio | Storage Evaluation |
| :--- | :--- | :---: | :---: | :--- |
| **`output.bin`** | Raw Binary (ELF Executable) | 100% (Dense) | 1.00x | **Best choice for raw compute cold storage.** |
| **`output.hex`** | Base-16 ASCII Text String | 50.0% | 2.00x | **Optimal text-encoded trade-off.** |
| **`output.twelve`**| Base-12 ASCII Text String | 44.8% | 2.23x | **Poor storage choice; high structural inflation.** |

## Comprehensive Key Findings

### 1. Which Text Format is Better for Storage?
**`output.hex` is significantly better for storage** than `output.twelve`. 
* Because 16 is a perfect power of 2 ($2^4$), Hexadecimal maps exactly 4 bits of binary data to 1 character. This guarantees a clean, un-fragmented **2:1 size inflation ratio** (2 bytes of text for every 1 byte of raw binary data).
* Base-12 is not a power of 2. It forces an arbitrary mathematical shift across byte boundaries, causing the string data to swell by approximately **2.23x** the original binary size.

### 2. Compression & Git Delta Characteristics
* **Hexadecimal Layouts** compress extremely well under standard pipeline algorithms (Gzip, Zstd) due to predictable character alignment boundaries.
* **Base-12 Layouts** break natural byte alignments, leading to lower data compression ratios and bloated storage commits inside your Git history object database over time.

### Recommendation
For text-safe pipeline operations, network transfers, and database storage where raw binary blobs are restricted, **standardize entirely on `output.hex`**. Abandon Base-12 transformations unless strict downstream hardware architecture requires duodecimal logic math.
