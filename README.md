<div align="center">

# GeekNome QC

**De novo FASTQ quality control — compiled binaries for Linux & Windows**

[![Latest Release](https://img.shields.io/github/v/release/geeknome/geeknome-qc?label=latest&color=00b4d8)](https://github.com/geeknome/geeknome-qc/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/geeknome/geeknome-qc/total?color=2a9d8f)](https://github.com/geeknome/geeknome-qc/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)

</div>

---

## 📥 Download

Pre-compiled binaries for **Linux (x86_64)** and **Windows (x86_64)**.

| Platform | Architecture | Download | Size |
|---|---|---|---|
| **Linux** | x86_64 | [geeknomeqc-v2.3.0-linux-x86_64.tar.gz](https://github.com/geeknome/geeknome-qc/releases/latest) | ~150 KB |
| **Windows** | x86_64 | [geeknomeqc-v2.3.0-windows-x86_64.zip](https://github.com/geeknome/geeknome-qc/releases/latest) | ~180 KB |
| **Web (WASM)** | any | [geeknome.com/geeknomeqc](https://geeknome.com/geeknomeqc) | — |

👉 **[All releases →](https://github.com/geeknome/geeknome-qc/releases)**

---

## ⚡ Highlights

- 🧬 **10 canonical filters**: adapter, 5'/3' quality, poly-G, poly-X, N, length, complexity
- ⚡ **~250k reads/s** sustained (16 pthreads)
- 📊 **99.98% parity** with fastp v0.23.4
- 🚀 **3.19× faster** than fastp on UHR 1M reads
- ✅ **Bit-for-bit deterministic** (MD5-stable output)
- 📦 **No external dependencies** (libc + pthreads only)
- 🔒 **Zero-Upload**: runs entirely on your machine

---

## 🚀 Quick Start

### Linux

```bash
# 1. Download
wget https://github.com/geeknome/geeknome-qc/releases/latest/download/geeknomeqc-v2.3.0-linux-x86_64.tar.gz

# 2. Verify checksum
sha256sum -c geeknomeqc-v2.3.0-linux-x86_64.tar.gz.sha256

# 3. Extract
tar -xzf geeknomeqc-v2.3.0-linux-x86_64.tar.gz

# 4. Run
./geeknomeqc sample_R1.fastq out.fq report.json 16 \
  --min-len=100 --max-n=5 --sw-window=4 --sw-qual=20
