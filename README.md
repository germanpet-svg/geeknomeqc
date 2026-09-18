<div align="center">

# 🧬 GeekNome QC

**De novo FASTQ quality control — compiled binaries for Linux & Windows**

[![Latest Release](https://img.shields.io/github/v/release/geeknome/geeknome-qc?label=latest&color=00b4d8&style=flat-square)](https://github.com/geeknome/geeknome-qc/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/geeknome/geeknome-qc/total?color=2a9d8f&style=flat-square)](https://github.com/geeknome/geeknome-qc/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)
[![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Windows-blue?style=flat-square)](#-download)
[![WebAssembly](https://img.shields.io/badge/WebAssembly-ready-654ff0?style=flat-square&logo=webassembly&logoColor=white)](https://geeknome.com/geeknomeqc)

**Numerical parity with fastp (99.98%) · 3.19× faster · Zero dependencies · Bit-for-bit deterministic**

[Download](#-download) · [Quick Start](#-quick-start) · [Benchmarks](#-benchmarks) · [Citation](#-citation)

</div>

---

## 📥 Download

Pre-compiled binaries for **Linux (x86_64)** and **Windows (x86_64)**. No dependencies beyond the OS standard library.

| Platform | Architecture | Download | Size | SHA256 |
|---|---|---|---|---|
| 🐧 **Linux** | x86_64 | [`geeknomeqc-v2.3.0-linux-x86_64.tar.gz`](https://github.com/geeknome/geeknome-qc/releases/latest) | ~150 KB | `a1b2c3...` |
| 🪟 **Windows** | x86_64 | [`geeknomeqc-v2.3.0-windows-x86_64.zip`](https://github.com/geeknome/geeknome-qc/releases/latest) | ~180 KB | `d4e5f6...` |
| 🌐 **Web (WASM)** | any | [geeknome.com/geeknomeqc](https://geeknome.com/geeknomeqc) | — | — |

👉 **[All releases →](https://github.com/geeknome/geeknome-qc/releases)**

> **Always verify checksums before running.** See [Verification](#-verification).

---

## ⚡ Highlights

- 🧬 **10 canonical filters** — adapter auto-detection, 5'/3' quality trimming, poly-G/poly-X removal, N-base filter, mean quality, low-Q percentage, minimum length, and sequence complexity
- ⚡ **~250k reads/s** sustained throughput (16 pthreads)
- 📊 **99.98% parity** with fastp v0.23.4 on 1M-read UHR reference
- 🚀 **3.19× faster** than fastp on UHR 1M reads (4.07 s vs 13.00 s)
- ✅ **Bit-for-bit deterministic** — MD5-identical output across native, WASM, and batch modes
- 📦 **No external dependencies** — only `libc` + `pthreads`
- 🔒 **Zero-Upload** — runs entirely on your machine; no network calls
- 🖥️ **Cross-platform** — Linux (glibc ≥ 2.17), Windows 10/11, WebAssembly

---

## 🚀 Quick Start

### Linux (x86_64)

```bash
# 1. Download the latest release
VERSION="2.3.0"
wget "https://github.com/geeknome/geeknome-qc/releases/download/v${VERSION}/geeknomeqc-v${VERSION}-linux-x86_64.tar.gz"
wget "https://github.com/geeknome/geeknome-qc/releases/download/v${VERSION}/SHA256SUMS.txt"

# 2. Verify integrity (always!)
sha256sum -c SHA256SUMS.txt

# 3. Extract
tar -xzf "geeknomeqc-v${VERSION}-linux-x86_64.tar.gz"

# 4. Install (user-local, no sudo)
mkdir -p ~/.local/bin
mv geeknomeqc ~/.local/bin/
export PATH="$HOME/.local/bin:$PATH"

# 5. Verify installation
geeknomeqc --version
# → geeknomeqc v2.3.0 (built 2026-09-17)

# 6. Run QC
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16 \
  --min-len=100 --max-n=5 --sw-window=4 --sw-qual=20
Windows (x86_64, PowerShell)
powershell
# 1. Download
$VERSION = "2.3.0"
$url = "https://github.com/geeknome/geeknome-qc/releases/download/v${VERSION}/geeknomeqc-v${VERSION}-windows-x86_64.zip"
Invoke-WebRequest -Uri $url -OutFile "geeknomeqc.zip"

# 2. Verify integrity (always!)
Get-FileHash .\geeknomeqc.zip -Algorithm SHA256
# Compare against SHA256SUMS.txt from the release page

# 3. Extract
Expand-Archive geeknomeqc.zip -DestinationPath .

# 4. Move to a folder in your PATH (e.g., C:\Tools)
Move-Item .\geeknomeqc.exe C:\Tools\ -Force

# 5. Verify installation
geeknomeqc.exe --version
# → geeknomeqc v2.3.0 (built 2026-09-17)

# 6. Run QC
.\geeknomeqc.exe sample_R1.fastq clean_R1.fastq report.json 16 --min-len=100 --max-n=5
WebAssembly (browser)
No installation. Just open geeknome.com/geeknomeqc.

All processing happens locally in your browser — no file is ever uploaded.

📖 Usage
text
geeknomeqc <input.fastq> <output.fastq> <report.json> <threads> [options]
Required arguments
Argument	Description
input.fastq	Input FASTQ file (optionally .gz compressed)
output.fastq	Cleaned FASTQ output path
report.json	QC metrics in JSON format
threads	Number of worker threads (1–16)
Common options
Option	Default	Description
--min-len=N	100	Minimum read length after trimming (bp)
--max-n=N	5	Maximum ambiguous (N) bases allowed
--sw-window=N	4	Sliding-window size for 3' trimming
--sw-qual=N	20	Sliding-window quality threshold (Phred)
--head-qual=N	20	5' head quality threshold (Phred)
--complexity-min=N	30	Minimum sequence complexity (%)
--qual-phred=N	15	Low-quality base threshold
--qual-pct-limit=N	40	Max % of bases below --qual-phred
--polyg-min-run=N	10	Min poly-G run length to trigger trim
--polyx-min-run=N	10	Min poly-X run length to trigger trim
--enable-adapter=0|1	1	Enable adapter auto-detection
--enable-polyg=0|1	1	Enable poly-G trimming
--enable-polyx=0|1	1	Enable poly-X trimming
--help	—	Show full help
--version	—	Show version
Examples
Minimal (all defaults):

bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16
Strict quality (Q30):

bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16 \
  --sw-qual=30 --head-qual=25 --min-len=150
Disable adapter detection (already-trimmed input):

bash
geeknomeqc sample_R1.fastq clean_R1.fastq report.json 16 --enable-adapter=0
Paired-end (run sequentially):

bash
geeknomeqc sample_R1.fastq clean_R1.fastq report_R1.json 16
geeknomeqc sample_R2.fastq clean_R2.fastq report_R2.json 16
Full documentation: docs/USAGE.md

Threads
Threads	Use case
1	Debugging, reproducibility tests
4	Laptop, low-power scenarios
8	Desktop (recommended — saturation point)
16	Server (Amdahl-saturated, no gain beyond 8)
Note: On 1M-read inputs, throughput saturates at 8 threads. Additional threads provide no measurable benefit — serial fraction ≈ 15% (Amdahl).

📊 Benchmarks
Test dataset: Illumina UHR reference, 1,000,000 reads (140,793,077 bases), GC 44.96%.
Hardware: Intel Xeon E5-2690 v4 (14c/28t, 32 GB RAM).

Numerical parity vs fastp v0.23.4
Metric	fastp v0.23.4	GeekNome QC v2.3	Δ
Input reads	1,000,000	1,000,000	0.000%
Input bases	140,793,077	140,793,077	0.000%
GC content	44.96%	44.87%	−0.09 pp
Output reads	924,061	923,879	−0.020%
Output bases	133,388,689	133,322,360	−0.050%
Retention rate	92.406%	92.388%	−0.018 pp
Execution time (8t)	13.00 s	4.00 s	3.25×
Execution time (16t)	—	4.07 s	3.19×
Parallel scalability
Threads	Wall time	Speedup	Efficiency
1	~28.0 s	1.00×	100%
4	~8.0 s	3.50×	88%
8	4.00 s	7.00×	88%
16	4.07 s	6.88×	43%
Amdahl analysis: Serial fraction S ≈ 15%, theoretical max speedup 1/S ≈ 6.7×.

Throughput scaling with file size
Data volume	Reads	Time	Throughput
329 MB	1,000,000	5.34 s	187,265 reads/s
1.5 GB	4,556,177	23.30 s	195,544 reads/s
2.0 GB	6,075,090	34.80 s	174,571 reads/s
Production-scale batch validation
Files	Total reads	Total time	Throughput	Failures
18	15,000,000	107 s	140k reads/s	0
Full data: docs/BENCHMARKS.md

🔐 Verification
Every release ships with SHA256SUMS.txt. Always verify before running.

Linux
bash
sha256sum -c SHA256SUMS.txt
# Expected: geeknomeqc-v2.3.0-linux-x86_64.tar.gz: OK
Windows (PowerShell)
powershell
Get-FileHash .\geeknomeqc.exe -Algorithm SHA256
# Compare the output hash manually against SHA256SUMS.txt
GPG Signature (coming soon)
Each release will be signed with the GeekNome release key (0xDEADBEEF...). Verify with:

bash
gpg --verify geeknomeqc-v2.3.0-linux-x86_64.tar.gz.sig
VirusTotal
All binaries are scanned before release. VirusTotal links are included in every release note.

Windows SmartScreen
The first time you run the binary, Windows may show a SmartScreen warning because the binary is not code-signed. Click More info → Run anyway. This is expected behavior for unsigned open-source binaries. Verify the SHA256 hash to be sure.

📄 Citation
If you use GeekNome QC in your research, please cite:

bibtex
@article{pena2026geeknome,
  title   = {De Novo FASTQ Quality Control Engine in C11 and WebAssembly:
             Numerical Parity, Parallel Saturation, and Production-Scale Batch Validation},
  author  = {Pe{\~n}a, German},
  journal = {Bioinformatics},
  year    = {2026},
  doi     = {10.1093/bioinformatics/XXXXXXX},
  note    = {ORCID: 0009-0004-0164-5414},
  url     = {https://github.com/geeknome/geeknome-qc}
}
Machine-readable metadata: CITATION.cff

DOI
https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg

Every release is archived on Zenodo and gets its own DOI. Cite the specific version you used.

🐛 Support
🐞 Bug reports: Open an issue

💬 Questions & discussions: Community forum

🌐 Web version: geeknome.com/geeknomeqc

🔒 Security issues: See SECURITY.md

✉️ Direct contact: contact@geeknome.com

📜 License
MIT — see LICENSE.

You are free to use, modify, and redistribute this software, including for commercial purposes. Attribution is appreciated but not required.

👤 Author
FES. German Peña
https://img.shields.io/badge/ORCID-0009--0004--0164--5414-A6CE39?style=flat-square&logo=orcid&logoColor=white

GeekNome · Bioinformatics Division
🌐 geeknome.com · ✉️ contact@geeknome.com

<div align="center">
Built with ❤️ for the bioinformatics community

Zero-Upload · Bit-for-bit deterministic · No dependencies

</div> ```
