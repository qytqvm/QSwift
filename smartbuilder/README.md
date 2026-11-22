# QSwift Smart Builder — Benchmarks

This folder holds benchmark data comparing ordinary QSwift compression with compression using a trained Smart Builder profile. Train once on similar files, then measure separate, previously unseen test files.

Smart Builder profile encoding currently supports **Turbo and Maximus**. Flash files can be restored through the shared decoder, but Flash is not a trained-profile encoding mode.

## Add a benchmark run

1. In the private owner area, open **QSwift · Smart bench**.
2. Select similar training files and a separate set of test files, then run the benchmark.
3. Download the CSV and save it here as `YYYY-MM-DD-<batch>-<profile>.csv`.
4. Add a matching `.md` note recording the profile name, profile size, training/test counts, dataset description, program version, machine and timing method.

Publish only non-confidential filenames and results. Do not upload training/test data, `.qsm` profiles, licence keys, owner keys, private SDKs or compressor source. A trained profile can contain pieces of the training data.

## Owner Smart bench CSV columns

The current export uses this exact header:

```csv
file,mode,input_bytes,standalone_bytes,standalone_reduction_pct,trained_bytes,trained_reduction_pct,trained_gain_pct,used_profile,compress_mb_s,decompress_mb_s,verified
```

| Column | Meaning |
|---|---|
| `file` | Test filename |
| `mode` | `turbo` or `maximus` |
| `input_bytes` | Original file size |
| `standalone_bytes` | Normal mode output without a profile |
| `standalone_reduction_pct` | Reduction relative to the original input |
| `trained_bytes` | Selected output from profile-assisted encoding |
| `trained_reduction_pct` | Selected output reduction relative to input |
| `trained_gain_pct` | `100 × (1 − trained_bytes / standalone_bytes)` |
| `used_profile` | Whether the profile stream was selected |
| `compress_mb_s` | Profile-assisted operation throughput, including the standalone comparison; excludes profile training |
| `decompress_mb_s` | Selected output restoration throughput |
| `verified` | Whether restored bytes exactly match the original |

The speed columns use MiB/s (1,048,576 bytes per unit). They do not include a separately timed standalone compression speed. The CSV does not contain profile names, profile sizes or hashes; record those separately if needed.

The profile output is kept only if smaller than the ordinary mode output; otherwise the normal output is returned. `used_profile=false` therefore means fallback, not a failed restore. These figures do not guarantee faster encoding.

## Existing reference batches

These synthetic in-house batches used 200 training files and 50 separate test files per category, with offline exact-restoration checks. Values below are percentage reduction, not speed measurements.

| Batch | Standalone Turbo | Turbo + profile | Maximus + profile |
|---|---:|---:|---:|
| Webhook events (JSON) | 38.4% | 79.4% | 82.1% |
| Server log chunks | 76.6% | 77.9% | 81.3% |
| IoT sensor readings (CSV) | 70.8% | 73.7% | 79.0% |
| Chat messages (JSON) | 28.1% | 71.0% | 71.8% |
| Invoices (XML) | 54.1% | 84.1% | 87.7% |
| Order records (CSV row batches) | 67.0% | 68.4% | 72.9% |

These output figures exclude the reusable profile's storage cost. They are dataset-specific, not expected savings for every enterprise batch.

## Report fair totals

Calculate totals separately for each mode; the same test file appears once for Turbo and once for Maximus.

- Standalone reduction: `100 × (1 − sum(standalone_bytes) / sum(input_bytes))`.
- Trained reduction: `100 × (1 − sum(trained_bytes) / sum(input_bytes))`.
- Trained size advantage: `100 × (1 − sum(trained_bytes) / sum(standalone_bytes))`.
- End-to-end stored size: `sum(trained_bytes) + profile_bytes`, counting each required profile once.

Keep training and test sets separate, count the profile cost when discussing total storage, and publish only exactly restored runs. Retain dated reports rather than overwriting earlier baselines.

© 2026 Qytte · All Rights Reserved · https://qytte.com/qswift
