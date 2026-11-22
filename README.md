# QSwift — Advanced Lossless Compression Suite

> **Pushing compression density further. Engineering faster recovery.**

**QSwift** is an asymmetric lossless compression architecture engineered for modern cloud infrastructure, storage systems, and high-throughput pipelines. While existing tools trade massive memory footprints or sluggish decoders for marginal size gains, QSwift extracts superior data density while preserving fast, lightweight decompression.

All four commercial QSwift modes are unified under a single zero-overhead decoder:
- **QSwift Flash** — Ultra-fast streaming throughput (caching, queues, RPC)
- **QSwift Turbo (Default)** — High-density, real-time cloud data (CDNs, JSON/CSV, logs)
- **QSwift Maximus** — Peak density for write-once, read-many storage (archives, distribution)
- **QSwift Smart Builder** — Schema and profile training for enterprise batches of repetitive files

Looking for our lightweight, single-header C99 LZ77 engine? Check out our free, open-source project:  
👉 **[QLz on GitHub](https://github.com/qytqvm/qlz/)** (MIT licensed, zero dependencies).

---

## The Four QSwift Products at a Glance

| Product | Target Use Case | Competing Baseline | 30-File Space Saved | Primary Advantage vs Baseline | Restore Speed | Decode RAM |
|---|---|---|---|---|---|---|
| **QSwift Flash** | Caches, queues, RPC, streaming | **Snappy** / **LZ4** | **56.9%** | **+8.57% compression advantage** over Snappy | **≈ 2,070 MB/s** | **< 1 MB** (featherweight) |
| **QSwift Turbo** *(Default)* | Cloud APIs, CDNs, structured logs | **Zstandard Level 3** | **74.1%** | **+13.20% compression advantage** over ZSTD 3 | **up to 1,215 MB/s** | **Same as ZSTD 3** |
| **QSwift Maximus** | Software delivery, archives, cold data | **Zstandard Level 19** | **79.0%** | **+11.66% compression advantage** over ZSTD 19 | **≈ 290 MB/s** | **≈ 8 MB** |
| **QSwift Smart Builder** | Repetitive sub-KB events & JSON | **Standalone Compressors** | **71.8% – 87.7%** | **Up to 3× higher density** on small batches | **Instant** | **Minimal** |

---

## 1. QSwift Flash — Streaming Throughput

Built for low-latency ingest, distributed caches, and message queues where compression must never stall the hot path.

- **Selective Adaptive Routing:** Combines a rapid 2 KB micro-probe with direct and long-window matching. Files with dense text patterns benefit from compact tokens, while large binaries and structured payloads automatically leverage extended windows.
- **Benchmark Highlights (30-sample corpus):**
  - **56.95% aggregate size reduction** (2.13 MB vs Snappy's 2.32 MB)
  - **8.57% compression advantage over Snappy**
  - **Decompression throughput of ~2,070 MB/s** (up to 25% to 200%+ faster than Snappy on structured files)
  - **Memory requirement:** Under 1 MB working memory.

---

## 2. QSwift Turbo — The Intelligent Default

Engineered as the drop-in default for cloud services, web backends, and structured data stores where Zstandard Level 3 is normally deployed.

- **Zero Full-Scan Overhead:** Inspects only the first 64 KB to determine file composition. Number-heavy payloads (JSON API responses, database dumps, metrics, CSVs) automatically route through QSwift's proprietary `numsplit` separation stream, while standard binary and text payloads take direct fast entropy paths.
- **Benchmark Highlights:**
  - **74.1% aggregate reduction** on our 30-sample benchmark set (saving 190 KB more than ZSTD 3 on 4.7 MB)
  - **13.20% compression advantage over ZSTD 3**
  - **Extreme wins on numeric structured data:** `tables.json` shrinks from 973 KB to just **2.2 KB** (vs ZSTD 3's 133.5 KB)
  - **Decompression speeds up to ~1,215 MB/s** with zero additional RAM overhead.

---

## 3. QSwift Maximus — Maximum Compression Density

Engineered for software release pipelines, operating system images, container registries, and enterprise backups where storage costs and download bandwidth are paramount.

- **Multi-Transform Deep Search:** Integrates x86 BCJ branch filters, 64-bit ARM BL-call filters, and numeric transformations evaluated within a never-worse competitive tournament.
- **Benchmark Highlights:**
  - **79.0% aggregate reduction** on the 30-sample set
  - **11.66% compression advantage over ZSTD 19**
  - **Text & Source Code:** Up to **94.10%** reduction on general text and **99.79%** on structured tabular data
  - **Silesia Corpus:** Achieves **82.30%** reduction on text payloads
  - **Asymmetric Efficiency:** Delivers dense compression while decompressing in just **~8 MB of RAM** (unlike heavy dictionary-based codecs that consume hundreds of megabytes on client machines).

---

## 4. QSwift Smart Builder — Enterprise Batch Profiling

Standard compression algorithms struggle on small, repetitive files (webhooks, IoT telemetry, chat messages, and invoices) because the dictionary overhead often exceeds the message payload.

QSwift Smart Builder trains a reusable `.qsm` profile from 200+ representative files, allowing subsequent streams to compress only the changing values:

| Batch Data Type | Avg Input Size | Standalone Turbo Reduction | Smart Builder Turbo | Smart Builder Maximus | Density Boost |
|---|---|---|---|---|---|
| **Webhook Events (JSON)** | 346 B | 38.4% | **79.4%** | **82.1%** | **+43.7% smaller** |
| **Chat Messages (JSON)** | 257 B | 28.1% | **71.0%** | **71.8%** | **+43.7% smaller** |
| **Invoices (XML)** | 809 B | 54.1% | **84.1%** | **87.7%** | **+33.6% smaller** |
| **IoT Sensor Readings (CSV)** | 2,217 B | 70.8% | **73.7%** | **79.0%** | **+8.2% smaller** |
| **Server Log Chunks** | 2,167 B | 76.6% | **77.9%** | **81.3%** | **+4.7% smaller** |
| **Order Records (CSV)** | 654 B | 67.0% | **68.4%** | **72.9%** | **+5.9% smaller** |

---

## Unified Decoder — One Command Restores Everything

Regardless of whether an archive was compressed using Flash, Turbo, Maximus, or a Smart Builder profile, clients restore data through a **single entry point**:
Zero Header Overhead: The decoder directly identifies stream magic bytes (QLZ4, QSPL, QSNP, QSWT, QVM4, QSMP) without needing external metadata wrappers.
Cross-Platform: Byte-exact identical decompression across C99, C++, Node.js, and WebAssembly.

Open-Source Legacy: QLz
For projects requiring an ultra-compact, permissive-licensed C engine, we open-sourced QLz under the MIT license:

Repository: https://github.com/qytqvm/qlz
Single-Header C99: Drop-in qlz.h with zero external dependencies
Measured Performance: 7.68% compression advantage over Google Snappy on structured files, with restore speeds exceeding 2,000 MB/s.

Enterprise Licensing & SDK Access
The commercial QSwift suite (Flash, Turbo, Maximus, Smart Builder, and the Unified Decoder) is distributed via enterprise binary packages and licensed developer SDKs.

Official Website: https://qytte.com

Interactive Compressor & Sandbox: https://qytte.com/sandbox

Full Benchmark Results: https://qytte.com/benchmark

Contact & Custom Pilots: https://qytte.com/contact

© Qytte · All Rights Reserved · https://qytte.com/qswift
