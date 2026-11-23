# QSwift Turbo — Benchmarks

Turbo is QSwift's default mode: fast compression and restore with more space saved than the standard fast setting, using the same light memory.

**Compared against:** Zstandard level 3

## Headline results (30-file reference set)
| | Reduction |
|---|---|
| QSwift Turbo | **74.1%** |
| Zstd 3 | 70.2% |

QSwift Turbo has a **13.19% compression advantage** over Zstd 3, with compression up to ~320 MB/s and restore ~1,215 MB/s (offline program).

### How to Run & Benchmark

Evaluate files online at [qytte.com/qswift/](https://qytte.com/qswift/) or test locally using the pre-compiled `qswift` binary:

```bash
# Compress with Turbo mode (default)
./qswift -e input.json -o input.qsw --mode turbo

# Decompress with unified auto-dispatching decoder
./qswift -d input.qsw -o restored.json
