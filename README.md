<div align="center">

# 🧬 GeekNome QC

**De novo FASTQ Quality Control Engine — Native C & WebAssembly**

[![Latest Release](https://img.shields.io/github/v/release/germanpet-svg/geeknomeqc?label=latest&color=00b4d8&style=flat-square)](https://github.com/germanpet-svg/geeknomeqc/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/germanpet-svg/geeknomeqc/total?color=2a9d8f&style=flat-square)](https://github.com/germanpet-svg/geeknomeqc/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Windows-blue?style=flat-square)](#-download)
[![WebAssembly](https://img.shields.io/badge/WebAssembly-ready-654ff0?style=flat-square&logo=webassembly&logoColor=white)](https://geeknome.com/geeknomeqc)

**99.98% numerical parity with fastp · Up to 3.25× faster · Zero dependencies · Deterministic**

[Download](#-download) · [Quick Start](#-quick-start) · [Usage](#-usage) · [Benchmarks](#-benchmarks) · [Citation](#-citation)

</div>

---

## 🧬 Overview

**GeekNome QC** is a native FASTQ quality-control engine written in C, with a WebAssembly build for browser-based analysis.

It is designed for fast, deterministic preprocessing of large FASTQ datasets while keeping the analysis local to the user's machine.

### Design principles

* 🔒 **Zero-Upload** — FASTQ files are processed locally.
* ⚡ **High throughput** — optimized for multicore CPUs.
* 🧵 **Multithreaded** — supports up to 16 worker threads.
* 📦 **Zero external dependencies** — native builds rely on the standard C/POSIX runtime and pthreads.
* 🌐 **WebAssembly** — the same engine can run directly in a browser.
* 🔁 **Deterministic** — identical input and parameters produce reproducible output.
* 📈 **Large-file capable** — tested with datasets up to multi-gigabyte scale.

GeekNome QC is intended as a **FASTQ preprocessing and quality-control engine**, not as a replacement for complete downstream genomic analysis pipelines.

---

## 📥 Download

Pre-compiled binaries are provided for:

* 🐧 Linux x86_64
* 🪟 Windows x86_64
* 🌐 WebAssembly / browser

| Platform   | Architecture | Download                                                                              |
| ---------- | ------------ | ------------------------------------------------------------------------------------- |
| 🐧 Linux   | x86_64       | [Latest Linux release](https://github.com/germanpet-svg/geeknomeqc/releases/latest)   |
| 🪟 Windows | x86_64       | [Latest Windows release](https://github.com/germanpet-svg/geeknomeqc/releases/latest) |
| 🌐 Web     | Browser      | [geeknome.com/geeknomeqc](https://geeknome.com/geeknomeqc)                            |

👉 **[View all releases](https://github.com/germanpet-svg/geeknomeqc/releases)**

> Always verify the SHA256 checksum of downloaded binaries before execution.

---

## ⚡ Highlights

* 🧬 **FASTQ quality control**
* 🔍 **Adapter auto-detection**
* ✂️ **5' and 3' quality trimming**
* 🧪 **Poly-G and Poly-X trimming**
* 🧬 **N-base filtering**
* 📊 **Mean-quality filtering**
* 📉 **Low-quality base percentage filtering**
* 📏 **Minimum read-length filtering**
* 🧠 **Sequence-complexity filtering**
* ⚡ **~250k reads/s** sustained throughput under the benchmark conditions
* 📊 **99.98% numerical parity** with fastp v0.23.4 in the benchmark dataset
* 🚀 **3.25× faster at 8 threads** and **3.19× faster at 16 threads** in the reported benchmark
* 🧵 **1–16 worker threads**
* 🔒 **Zero-Upload / local processing**
* 🌐 **WebAssembly support**
* 📦 **No third-party runtime dependencies**
* 🔁 **Bit-for-bit deterministic output**

---

## 🚀 Quick Start

### Linux x86_64

Download the latest release from:

https://github.com/germanpet-svg/geeknomeqc/releases/latest

Example:

```bash
VERSION="2.3.0"

wget "https://github.com/germanpet-svg/geeknomeqc/releases/download/v${VERSION}/geeknomeqc-v${VERSION}-linux-x86_64.tar.gz"

wget "https://github.com/germanpet-svg/geeknomeqc/releases/download/v${VERSION}/SHA256SUMS.txt"
```

### Verify the download

```bash
sha256sum -c SHA256SUMS.txt
```

### Extract

```bash
tar -xzf "geeknomeqc-v${VERSION}-linux-x86_64.tar.gz"
```

### Install locally

```bash
mkdir -p ~/.local/bin
mv geeknomeqc ~/.local/bin/
export PATH="$HOME/.local/bin:$PATH"
```

### Verify

```bash
geeknomeqc --version
```

Expected:

```text
geeknomeqc v2.3.0
```

### Run QC

```bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16 \
  --min-len=100 \
  --max-n=5 \
  --sw-window=4 \
  --sw-qual=20
```

---

## 🪟 Windows x86_64

Download the Windows ZIP from:

https://github.com/germanpet-svg/geeknomeqc/releases/latest

PowerShell example:

```powershell
$VERSION = "2.3.0"

$url = "https://github.com/germanpet-svg/geeknomeqc/releases/download/v${VERSION}/geeknomeqc-v${VERSION}-windows-x86_64.zip"

Invoke-WebRequest -Uri $url -OutFile "geeknomeqc.zip"
```

### Verify the checksum

```powershell
Get-FileHash .\geeknomeqc.zip -Algorithm SHA256
```

Compare the resulting hash with `SHA256SUMS.txt` from the release.

### Extract

```powershell
Expand-Archive geeknomeqc.zip -DestinationPath .
```

### Run

```powershell
.\geeknomeqc.exe --version
```

Example QC:

```powershell
.\geeknomeqc.exe sample_R1.fastq clean_R1.fastq report.json 16 `
  --min-len=100 `
  --max-n=5
```

---

## 🌐 WebAssembly

GeekNome QC is also available as a browser application.

**No installation is required.**

Open:

https://geeknome.com/geeknomeqc

All FASTQ processing is performed locally in the browser.

> **Your FASTQ data is not uploaded to a remote analysis server.**

The WebAssembly build is designed to provide the same core QC functionality while taking advantage of the user's local CPU resources.

---

## 📖 Usage

```text
geeknomeqc <input.fastq> <output.fastq> <report.json> <threads> [options]
```

### Required arguments

| Argument       | Description                     |
| -------------- | ------------------------------- |
| `input.fastq`  | Input FASTQ file                |
| `output.fastq` | Cleaned FASTQ output            |
| `report.json`  | QC metrics in JSON format       |
| `threads`      | Number of worker threads (1–16) |

### Common options

| Option                  | Default | Description                                      |
| ----------------------- | ------: | ------------------------------------------------ |
| `--min-len=N`           |   `100` | Minimum read length after trimming (bp)          |
| `--max-n=N`             |     `5` | Maximum ambiguous `N` bases allowed              |
| `--sw-window=N`         |     `4` | Sliding-window size for 3' trimming              |
| `--sw-qual=N`           |    `20` | Sliding-window quality threshold (Phred)         |
| `--head-qual=N`         |    `20` | 5' head quality threshold (Phred)                |
| `--complexity-min=N`    |    `30` | Minimum sequence complexity (%)                  |
| `--qual-phred=N`        |    `15` | Low-quality base threshold                       |
| `--qual-pct-limit=N`    |    `40` | Maximum percentage of bases below `--qual-phred` |
| `--polyg-min-run=N`     |    `10` | Minimum poly-G run length                        |
| `--polyx-min-run=N`     |    `10` | Minimum poly-X run length                        |
| `--enable-adapter=0\|1` |     `1` | Enable adapter auto-detection                    |
| `--enable-polyg=0\|1`   |     `1` | Enable poly-G trimming                           |
| `--enable-polyx=0\|1`   |     `1` | Enable poly-X trimming                           |
| `--help`                |       — | Display help                                     |
| `--version`             |       — | Display version                                  |

---

## 🧪 Examples

### Minimal

```bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16
```

### Strict quality / Q30

```bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16 \
  --sw-qual=30 \
  --head-qual=25 \
  --min-len=150
```

### Disable adapter detection

Useful when the input has already been adapter-trimmed:

```bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16 \
  --enable-adapter=0
```

### Paired-end FASTQ

R1 and R2 can be processed independently:

```bash
geeknomeqc sample_R1.fastq clean_R1.fastq report_R1.json 16

geeknomeqc sample_R2.fastq clean_R2.fastq report_R2.json 16
```

Additional documentation:

`docs/USAGE.md`

---

## 🧵 Threading

GeekNome QC supports **1–16 worker threads**.

| Threads | Typical use                         |
| ------: | ----------------------------------- |
|       1 | Debugging and reproducibility tests |
|       4 | Laptop / low-power systems          |
|       8 | Desktop systems                     |
|      16 | High-core-count systems             |

### Observed scaling

In the reported 1M-read benchmark, throughput reached its practical saturation point around **8 threads**.

Additional threads can provide little or no benefit for workloads dominated by the serial portion of the pipeline.

This behavior is workload- and hardware-dependent.

---

## 📊 Benchmarks

### Benchmark dataset

* Dataset: Illumina UHR reference
* Reads: **1,000,000**
* Bases: **140,793,077**
* GC: **44.96%**
* Hardware: Intel Xeon E5-2690 v4
* CPU: 14 cores / 28 threads
* RAM: 32 GB

---

### Numerical parity vs fastp

Comparison against **fastp v0.23.4**:

| Metric                      | fastp v0.23.4 | GeekNome QC v2.3 | Difference |
| --------------------------- | ------------: | ---------------: | ---------: |
| Input reads                 |     1,000,000 |        1,000,000 |     0.000% |
| Input bases                 |   140,793,077 |      140,793,077 |     0.000% |
| GC content                  |        44.96% |           44.87% |   −0.09 pp |
| Output reads                |       924,061 |          923,879 |    −0.020% |
| Output bases                |   133,388,689 |      133,322,360 |    −0.050% |
| Retention rate              |       92.406% |          92.388% |  −0.018 pp |
| Execution time — 8 threads  |       13.00 s |           4.00 s |  **3.25×** |
| Execution time — 16 threads |             — |           4.07 s |  **3.19×** |

> Numerical parity is reported for the specific benchmark dataset and configuration above. It should not be interpreted as universal equivalence across all FASTQ datasets or parameter combinations.

---

### Parallel scalability

| Threads | Wall time | Speedup | Efficiency |
| ------: | --------: | ------: | ---------: |
|       1 |   ~28.0 s |   1.00× |       100% |
|       4 |    ~8.0 s |   3.50× |        88% |
|       8 |    4.00 s |   7.00× |        88% |
|      16 |    4.07 s |   6.88× |        43% |

The observed scaling indicates that the workload becomes increasingly limited by the serial portion of the pipeline beyond approximately 8 threads.

---

### Throughput scaling with file size

| Data volume |     Reads |    Time |      Throughput |
| ----------: | --------: | ------: | --------------: |
|      329 MB | 1,000,000 |  5.34 s | 187,265 reads/s |
|      1.5 GB | 4,556,177 | 23.30 s | 195,544 reads/s |
|      2.0 GB | 6,075,090 | 34.80 s | 174,571 reads/s |

These measurements demonstrate multi-gigabyte processing under the reported benchmark conditions.

---

### Production-scale batch validation

| Files | Total reads | Total time |    Throughput | Failures |
| ----: | ----------: | ---------: | ------------: | -------: |
|    18 |  15,000,000 |      107 s | ~140k reads/s |        0 |

Full benchmark information:

`docs/BENCHMARKS.md`

---

## 🔐 Verification

Every release should include a `SHA256SUMS.txt` file.

### Linux

```bash
sha256sum -c SHA256SUMS.txt
```

### Windows PowerShell

```powershell
Get-FileHash .\geeknomeqc.zip -Algorithm SHA256
```

Compare the calculated hash against the official checksum published with the release.

### Windows SmartScreen

Windows may display a SmartScreen warning if the executable is not code-signed.

If this occurs, verify the SHA256 checksum against the official release checksum before deciding whether to run the executable.

---

## 🔒 Privacy / Zero-Upload

GeekNome QC is designed around local processing.

### Native

FASTQ files are processed directly on the user's computer.

### WebAssembly

The browser version executes the QC engine locally through WebAssembly.

The intended architecture does not require uploading FASTQ data to a remote genomic-processing server.

> **Zero-Upload means the genomic reads remain local to the execution environment.**

---

## 🧬 What GeekNome QC Does

GeekNome QC focuses on the **FASTQ quality-control stage**:

```text
FASTQ
  │
  ├── Adapter detection
  ├── Quality trimming
  ├── Poly-G / Poly-X filtering
  ├── N-base filtering
  ├── Complexity filtering
  ├── Length filtering
  │
  ▼
Clean FASTQ
  │
  ▼
QC Report
```

It can therefore serve as an upstream component for downstream genomic workflows such as alignment, variant calling, microbial screening, or other sequence-analysis pipelines.

---

## 🏗️ Architecture

```text
                 ┌─────────────────────┐
                 │      FASTQ input    │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   GeekNome QC Core  │
                 │        C engine     │
                 └──────────┬──────────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
          Native executable       WebAssembly
          Linux / Windows          Browser
                 │                     │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │   Clean FASTQ +     │
                 │     QC report       │
                 └─────────────────────┘
```

The native and WebAssembly builds are based on the same core processing engine.

---

## 📁 Repository structure

```text
geeknomeqc/
├── src/
│   └── ...
├── docs/
│   ├── USAGE.md
│   └── BENCHMARKS.md
├── web/
│   └── ...
├── README.md
├── LICENSE
├── SECURITY.md
└── CITATION.cff
```

---

## 📄 Citation

If you use GeekNome QC in research or software development, please cite the specific release or repository version used.

### BibTeX

```bibtex
@software{pena2026geeknomeqc,
  author  = {Peña, German},
  title   = {GeekNome QC: De novo FASTQ Quality Control Engine in C and WebAssembly},
  year    = {2026},
  url     = {https://github.com/germanpet-svg/geeknomeqc},
  note    = {ORCID: 0009-0004-0164-5414}
}
```

Machine-readable citation metadata:

`CITATION.cff`

### DOI

A Zenodo DOI should be added here once the repository has an actual DOI assigned.

> Do not publish a placeholder DOI such as `10.5281/zenodo.XXXXXXX` as a real citation identifier.

---

## 🐛 Support

### Bug reports

Please open a GitHub Issue and include:

* GeekNome QC version
* Operating system
* CPU / number of threads
* Input FASTQ characteristics
* Command used
* Relevant error message
* Minimal reproducible example when possible

### Documentation

* `docs/USAGE.md`
* `docs/BENCHMARKS.md`

### Web version

https://geeknome.com/geeknomeqc

### Security

For security-related issues, see:

`SECURITY.md`

### Contact

[contact@geeknome.com](mailto:contact@geeknome.com)

---

## 📜 License

GeekNome QC is released under the **MIT License**.

See [`LICENSE`](LICENSE).

You are free to use, modify, and redistribute the software, including for commercial purposes, subject to the terms of the MIT License.

---

## 👤 Author

**FES. German Peña**

ORCID:
https://orcid.org/0009-0004-0164-5414

**GeekNome · Bioinformatics Division**

🌐 https://geeknome.com
✉️ [contact@geeknome.com](mailto:contact@geeknome.com)

---

<div align="center">

**Built with ❤️ for the bioinformatics community**

**Zero-Upload · Deterministic · Multithreaded · WebAssembly**

</div>
