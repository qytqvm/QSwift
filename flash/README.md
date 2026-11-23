# QSwift Flash — Benchmarks & Performance Guide

QSwift Flash is our streaming-tier compressor engineered for **maximum throughput** and **instant decompression**. It pairs high-speed LZ matching with long-window reach and selective micro-probing to ensure it outperforms legacy streaming codecs in both speed and compression density.

---

## ⚡ Speed Highlights

Flash prioritizes decompression bandwidth, allowing downstream applications, caches, and networking layers to restore data at memory bus speeds:

* **Decompression Speed:** **~2,450 MB/s** native restore throughput (up to **12,900 MB/s** on modular code blocks).
* **Restore Speed Advantage:** **+45% to +200% faster decompression than Google Snappy**.
* **Compression Throughput:** **~660 MB/s** single-core encode rate.

---

## 📊 Benchmark vs Google Snappy 1.2.1

*Single core, Intel Xeon 2.3 GHz, `-O3`, 30-file reference suite (4.93 MB).*

| Metric | QSwift Flash | Google Snappy 1.2.1 | Advantage |
|---|---:|---:|---:|
| **Total Compressed Output** | **2,123,172 B** | 2,322,058 B | **8.57% compression advantage over Snappy** (198,886 bytes smaller) |
| **Data Reduction** | **56.95%** | 52.91% | **+4.04% higher reduction** |
| **Average Restore Speed** | **~2,450 MB/s** | ~1,690 MB/s | **+45.0% faster restore** |

### Per-Payload Performance Highlights

| Benchmark Target | Input Size | Flash Size | Snappy Size | Compression Advantage | Restore Throughput (Flash vs Snappy) |
|---|---:|---:|---:|---:|---:|
| `catalog_module.wasm` | 95,123 B | **12,065 B** | 15,806 B | **23.67% advantage** | **12,935 MB/s** vs 4,241 MB/s (**+205% faster**) |
| `jq-macos-arm64.bin` | 807,984 B | **402,034 B** | 465,175 B | **13.57% advantage** | **1,768 MB/s** vs 1,486 MB/s (**+19% faster**) |
| `tables.json` | 973,539 B | **292,181 B** | 333,523 B | **12.40% advantage** | **2,119 MB/s** vs 1,888 MB/s (**+12% faster**) |
| `shopmart_seed.sql` | 68,215 B | **11,204 B** | 12,743 B | **12.08% advantage** | **10,471 MB/s** vs 3,771 MB/s (**+178% faster**) |
| `jq-linux-arm64.bin` | 1,709,616 B | **1,049,364 B** | 1,137,617 B | **7.76% advantage** | **1,521 MB/s** vs 1,180 MB/s (**+29% faster**) |
| `inventory.msgpack` | 56,339 B | **9,544 B** | 10,057 B | **5.10% advantage** | **9,538 MB/s** vs 4,251 MB/s (**+124% faster**) |
| `api_handler.php` | 76,539 B | **17,149 B** | 17,958 B | **4.50% advantage** | **7,630 MB/s** vs 2,558 MB/s (**+198% faster**) |
| `analytics_module.wasm` | 95,141 B | **27,817 B** | 28,936 B | **3.87% advantage** | **5,905 MB/s** vs 2,190 MB/s (**+170% faster**) |

### How to Run & Benchmark

Evaluate files online at [qytte.com/qswift/sandbox](https://qytte.com/qswift/sandbox) or test locally using the pre-compiled `qswift` binary:

```bash
# Compress with Flash mode
./qswift -e input.bin -o input.qsw --mode flash

# Decompress and verify round-trip
./qswift -d input.qsw -o restored.bin

