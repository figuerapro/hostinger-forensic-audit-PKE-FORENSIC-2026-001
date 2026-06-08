# 04 — Benchmark Data

All benchmarks executed on Hostinger KVM 8 VPS, IP 45.132.241.248, AlmaLinux 9.7 (kernel 5.14.0), AMD EPYC 7543P, 8 vCPUs, 31 GB usable RAM. All timestamps UTC.

---

## 4.1 CPU Baseline Benchmark

**Date:** June 3, 2026, 16:44 UTC  
**Tool:** PKE Transporter v3.0 (BLAKE3 hash engine)  
**Duration:** 10 seconds per test

| Metric | Single-Thread | Multi-Thread (8) |
|--------|:------------:|:----------------:|
| BLAKE3 throughput | 2.83 GB/s | 18.20 GB/s |
| Raw compute (integer ops) | — | 1.70 trillion |

**Comparative baseline** (local Ryzen 16-core workstation):
- BLAKE3 single-thread: 1.09 GB/s (EPYC per-core: 2.1× faster)
- BLAKE3 multi-thread (8): 17.4 GB/s (EPYC: comparable at same thread count)

**BLAKE3 Seal:** `b3314c0e453404def78a8f35ad202f6611360f4b6b239520ea275b401e0f2b8e`

---

## 4.2 CPU Steal Time Measurement

**Date:** June 3, 2026, 18:54–19:25 UTC  
**Source:** `/proc/stat`, `top`

| Timestamp | User % | System % | Steal % | Idle % | LOAD |
|-----------|:------:|:--------:|:-------:|:------:|:----:|
| 18:56:22 | 43.8 | 42.4 | 21.4 | 0.0 | 1,399 |
| 19:17:45 | — | — | 21.4 | — | 1,393 |
| 19:25:10 | — | — | 21.1 | — | 2,949 |

**Mean steal time:** 21.2% (σ < 0.2 pp)  
**Effective vCPU count:** ~6.3 of 8 purchased

---

## 4.3 Disk Write Benchmark

**Date:** June 3, 2026, 17:20–18:19 UTC  
**Method:** 35 parallel `dd` processes, 1 MB blocks, 10,240 count per writer  
**Source:** `/dev/zero` → `/root/fill/diskfill_N.dat`  
**Simultaneous load:** 1,025 BLAKE3 threads, 200 HTTP workers, 28 GB RAM allocation

### Timeline

| Time | Elapsed | Disk Used | Data Written | Instantaneous Speed |
|------|:-------:|:---------:|:------------:|:-------------------:|
| 17:20 | 0 min | 23 GB | 0 GB | — |
| 17:37 | 17 min | 58 GB | 35 GB | 35 MB/s |
| 17:40 | 20 min | 93 GB | 70 GB | 59 MB/s |
| 17:44 | 24 min | 150 GB | 127 GB | 90 MB/s |
| 17:50 | 30 min | 304 GB | 281 GB | 159 MB/s |
| 17:55 | 35 min | 351 GB | 328 GB | 159 MB/s |
| 18:14 | 54 min | 374 GB | 351 GB | 110 MB/s |
| 18:19 | 59 min | 399 GB | 376 GB | 108.7 MB/s |

### Summary

| Metric | Value |
|--------|-------|
| Total data written | 376 GB |
| Duration | 3,540 seconds (59 minutes) |
| Average throughput | 108.7 MB/s |
| Peak instantaneous | 159 MB/s (at 30 min, 35 concurrent writers) |
| Disk start | 23 GB (6%) |
| Disk end | 399 GB (100%) |
| Δ disk utilization | +94 percentage points |

### Concurrent System Load During Benchmark

| Component | Threads | CPU % | Throughput |
|-----------|:-------:|:-----:|------------|
| BLAKE3 engine | 1,025 | 462% | 147 GB/s (hash compute) |
| dd disk writers | 35 | 35% | 108 MB/s (disk write) |
| HTTP bandwidth workers | 200 | 27% | 4 Gbps (network) |
| RAM allocation (Python) | 1 | 2% | 28 GB allocated |
| Nginx + MariaDB + FTP + SSH | ~20 | 5% | All ports active |
| **Total** | **1,281** | **531%** | **0% CPU idle** |

### Verification Commands

| Command | Output |
|---------|--------|
| `df -h /` | 399 GB / 400 GB (100%) |
| `du -sh /root/fill/` | 351 GB |
| `ls /root/fill/diskfill_*.dat` | 35 files × 10 GB each |
| `free -h` | 17 GB / 31 GB used |
| `swapon -s` | 4 GB swap active |
| `uptime` | LOAD: 1,077 · 18 zombie processes |
| `ps aux \| wc -l` | 258 processes |

---

## 4.4 Memory Benchmark

**Date:** June 3, 2026, 16:44 UTC  
**Tool:** PKE Transporter v3.0

| Metric | Value |
|--------|-------|
| Memory read bandwidth | 11.99 GB/s |
| Memory write bandwidth | 13.63 GB/s |
| Memory copy bandwidth | 29.08 GB/s |

---

## 4.5 Network Benchmark

**Date:** June 3, 2026, 16:44 UTC  
**Tool:** PKE Transporter v3.0 (loopback TCP saturation)

| Metric | Value |
|--------|-------|
| TCP loopback throughput | 3.50 Gbps |
| TCP connections established | 70,104 |

---

## 4.6 Combined Load Snapshot (Peak)

**Date:** June 3, 2026, 19:25:10 UTC  
**Source:** `top`, `uptime`, `dmesg`, `df`, `ss`, `free`

| Metric | Value | Relative to Capacity |
|--------|-------|:--------------------:|
| System LOAD (1 min avg) | 2,949 | 369× (on 8 vCPU) |
| Total TCP sockets | 21,318 | — |
| OOM kills (cumulative) | 18 | — |
| CPU user | ~44% | — |
| CPU system | ~42% | — |
| CPU steal | 21.1% | — |
| CPU idle | 0.0% | — |
| RAM used | 30 GB / 31 GB | 97% |
| Disk used | 399 GB / 400 GB | 100% |
| Bandwidth consumed | ~27 GB / 32 TB | 0.08% |
| BLAKE3 data processed | ~51 TB | — |
| Process count | 258 | — |
| Zombie processes | 18 | — |

---

## 4.7 Chunking Benchmark (FastCDC + BLAKE3)

**Date:** June 3, 2026, 16:44 UTC

| Metric | Value |
|--------|-------|
| Chunking throughput | 265.08 MB/s |
| Data processed | 2.63 GB |
| Chunks generated | 172,032 |
| Chunk sizes | 16K / 64K / 256K (FastCDC) |

---

## 4.8 Cryptographic Seals for This Document

| Document | SHA-256 | BLAKE3 |
|----------|---------|--------|
| forensic-disk-benchmark-report.txt | `f4e94a6a...` (HITO 40) | `458babcc...` (HITO 40) |
| VPS_PHOENIX_CERTIFICADO.txt | — | `b3314c0e...` |

Full 64-character hashes are available in `seals/MANIFEST.txt`.

