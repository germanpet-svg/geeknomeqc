cd /mnt/c/nueva_geeknome_web/web/modulos_usados

cat > RELEASE_NOTES.md <<'EOF'
## GeekNome QC v2.3.0

**Release date:** 2026-09-17
**Engine version:** fastqc.c v2.3
**License:** Proprietary — free for academic use, commercial license required
**Platforms:** Linux x86_64 (static) · Windows x86_64 (portable)

---

### 🚀 Downloads

| Platform | File | Size |
|---|---|---|
| 🐧 Linux x86_64 (universal static) | `geeknomeqc-v2.3.0-linux-x86_64.tar.gz` | ~387 KB |
| 🪟 Windows x86_64 (portable) | `geeknomeqc-v2.3.0-windows-x86_64.zip` | ~50 KB |

**Compatibility:**
- Linux: kernel ≥ 3.2 (Ubuntu 14.04+, Debian 8+, CentOS 7+, RHEL 8+, WSL2)
- Windows: 10 / 11 (x64), no external DLLs required

No installation. Extract and run.

---

### 📜 License

**Free for academic and non-commercial research use.**

Commercial use requires a paid license. Contact: **contact@geeknome.com**

See [LICENSE](https://github.com/geeknome/geeknome-qc/blob/main/LICENSE) for full terms.

---

### ✨ What's new

- **Fixed (critical):** pthread ceiling bug in the engine (`F2B_THREADS_MAX 8` hardcoded) — now supports up to 16 worker threads
- **Added:** Production-scale batch validation — 18 files, 15,000,000 reads, 0 failures
- **Added:** Amdahl saturation analysis — serial fraction ≈ 15%, theoretical max 6.7×
- **Added:** Complete documentation suite (install, usage, benchmarks, changelog)

---

### 📊 Performance

**Test:** Illumina UHR reference, 1M reads (140,793,077 bases), GC 44.96%
**Hardware:** Intel Xeon E5-2690 v4 (14c/28t, 32 GB RAM)

| Metric | fastp v0.23.4 | GeekNome QC v2.3 |
|---|---:|---:|
| Execution time (8t) | 13.00 s | **4.00 s** (3.25×) |
| Throughput | 77k reads/s | **250k reads/s** (3.25×) |
| Output deviation | — | 182 reads (−0.020%) |
| Retention | 92.406% | 92.388% (−0.018 pp) |
| MD5 determinism | — | ✅ bit-for-bit |

---

### 📥 Install

**Linux:**
```bash
VERSION="2.3.0"
wget "https://github.com/geeknome/geeknome-qc/releases/download/v${VERSION}/geeknomeqc-v${VERSION}-linux-x86_64.tar.gz"
wget "https://github.com/geeknome/geeknome-qc/releases/download/v${VERSION}/SHA256SUMS.txt"
sha256sum -c SHA256SUMS.txt
tar -xzf "geeknomeqc-v${VERSION}-linux-x86_64.tar.gz"
mkdir -p ~/.local/bin && mv geeknomeqc-linux ~/.local/bin/geeknomeqc
export PATH="$HOME/.local/bin:$PATH"
geeknomeqc --help
