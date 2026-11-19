# QSwift Maximus — Benchmarks

Maximus is QSwift's densest mode: a deep search that picks the smallest exact result for each file, while restoring in about the same memory as Zstd 19 (~8 MB).

**Compared against:** Zstandard level 19 (Silesia results also include GZIP 9, zlib 9 and Deflate 9)

## Headline results
| Corpus | QSwift Maximus | Zstd 19 | Advantage |
|---|---|---|---|
| 30-file reference set | **79.0%** reduction | 76.2% | **11.66% smaller** |
| Silesia | **75.4%** reduction | 75.0% | 1.47% smaller |

## CSV uploads
Drop each run into this folder as `YYYY-MM-DD-<description>.csv`.

Columns: `file, mode, input_bytes, qytte_bytes, qytte_reduction_pct, V's (Zstd 19), V's Bytes, encode_ms, decode_ms, original_sha256, decoded_sha256, verified`.

Timings depend on the machine that produced them. Every result is decoded and SHA-256 checked.

© Qytte · All Rights Reserved · https://qytte.com
