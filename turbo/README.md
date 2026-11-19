# QSwift Turbo — Benchmarks

Turbo is QSwift's default mode: fast compression and restore with more space saved than the standard fast setting, using the same light memory.

**Compared against:** Zstandard level 3

## Headline results (30-file reference set)
| | Reduction |
|---|---|
| QSwift Turbo | **74.1%** |
| Zstd 3 | 70.2% |

QSwift Turbo has a **13.19% compression advantage** over Zstd 3, with compression up to ~320 MB/s and restore ~1,215 MB/s (offline program).

## CSV uploads
Drop each run into this folder as `YYYY-MM-DD-<description>.csv`.

Columns: `file, mode, input_bytes, qytte_bytes, qytte_reduction_pct, V's (Zstd 3), V's Bytes, encode_ms, decode_ms, original_sha256, decoded_sha256, verified`.

Timings depend on the machine that produced them. Every result is decoded and SHA-256 checked.

© Qytte · All Rights Reserved · https://qytte.com
