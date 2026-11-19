# QSwift QLz — Benchmarks

QLz is QSwift's fastest mode: a lightweight streaming compressor built for very fast restore with a tiny memory footprint (restores in under 1 MB of working memory).

**Compared against:** Google Snappy 1.2.1

## Headline results (30-file reference set)
| | Reduction | Restore |
|---|---|---|
| QSwift QLz | **56.53%** | ~1,977 MB/s |
| Snappy 1.2.1 | 52.91% | — |

QSwift QLz has a **7.68% compression advantage** over Snappy.

## CSV uploads
Drop each run into this folder as `YYYY-MM-DD-<description>.csv` (e.g. exported from the QSwift Sandbox "Download CSV" button).

| Column | Meaning |
|---|---|
| file | Original file name |
| mode | QSwift mode (QLz) |
| input_bytes | Original size |
| qytte_bytes | QSwift output size |
| qytte_reduction_pct | Space saved by QSwift |
| V's | Competitor (Snappy) |
| V's Bytes | Competitor output size |
| encode_ms / decode_ms | QSwift timings on the test machine |
| original_sha256 / decoded_sha256 / verified | Byte-for-byte round-trip proof |

Timings depend on the machine that produced them. Every result is decoded and SHA-256 checked.

© Qytte · All Rights Reserved · https://qytte.com
