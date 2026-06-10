# 08 "” Evidence Ledger

This document catalogs every piece of evidence referenced in the audit, with its cryptographic seals and verification instructions.

---

## 8.1 Primary Evidence Files

| # | File | Size | Content |
|---|------|------|---------|
| 1 | `FORENSIC_DENUNCIA_HOSTINGER.md` | 44,290 B | Full forensic report (15 sections, Spanish) |
| 2 | `CORREO_HOSTINGER_FINAL.md` | 18,372 B | Legal correspondence to Hostinger (Spanish) |
| 3 | `forensic-disk-benchmark-report.txt` | 14,930 B | Disk write benchmark with timestamps (English) |
| 4 | `VPS_PHOENIX_CERTIFICADO.txt` | 4,464 B | Initial VPS benchmark certification |
| 5 | `hostinger-pre-audit-notification.txt` | 3,486 B | Pre-operation message to Hostinger |
| 6 | `CERTIFICADO_RENDIMIENTO.txt` | 2,287 B | Local workstation benchmark (June 5) |
| 7 | `CERTIFICADO_RENDIMIENTO.json` | 1,866 B | Local benchmark data (JSON) |
| 8 | `RSQN_BRAIN_VIGIA.log` | ~1,000 B | SSH availability monitor log |
| 9 | `FORENSIC_SEAL.txt` | 140 B | SHA-256 seal for FORENSIC_LEDGER.json |
| 10 | `pke_storm_report.json` | 283 B | Local storm test data |

---

## 8.2 Audit Package Files (This Repository)

| # | File | Description |
|---|------|-------------|
| 16 | `README.md` | Entry point with TL;DR and navigation |
| 17 | `01-executive-summary.md` | Extended summary for journalists/CTOs |
| 18 | `02-timeline.md` | Chronological event log |
| 19 | `03-infrastructure-analysis.md` | Sold vs. delivered analysis |
| 20 | `04-benchmark-data.md` | All benchmark metrics |
| 21 | `05-support-communications.md` | Support Acknowledgments verbatim |
| 22 | `06-regulatory-violations.md` | Legal analysis |
| 23 | `07-cloudflare-assessment.md` | Cloudflare dependency analysis |
| 24 | `08-evidence-ledger.md` | This document |
| 25 | `seals/MANIFEST.txt` | Complete dual-hash manifest |

---

## 8.4 Verification Instructions

### Prerequisites

- **Linux/macOS:** `sha256sum` is built in. Install `b3sum` via `cargo install b3sum` or your package manager.
- **Windows:** `Get-FileHash -Algorithm SHA256` in PowerShell. Install `b3sum` via `cargo install b3sum`.
- **Python (cross-platform):** `pip install blake3`

### Verify SHA-256

```bash
# Linux/macOS
sha256sum -c seals/MANIFEST.txt

# Windows PowerShell
Get-Content seals\MANIFEST.txt | ForEach-Object {
    $parts = $_ -split '\s+'
    $expected = $parts[0]
    $file = $parts[2]
    $actual = (Get-FileHash -Algorithm SHA256 $file).Hash
    if ($actual -eq $expected) { "$file OK" } else { "$file MISMATCH" }
}
```

### Verify BLAKE3

```bash
# Using b3sum
b3sum -c seals/MANIFEST.txt

# Using Python
python -c "
import blake3, os
with open('seals/MANIFEST.txt') as f:
    for line in f:
        expected, _, filename = line.strip().partition('  ')
        with open(filename, 'rb') as f2:
            actual = blake3.blake3(f2.read()).hexdigest().upper()
        status = 'OK' if actual == expected else 'MISMATCH'
        print(f'{status}: {filename}')
"
```

---

## 8.5 Chain of Custody

| Date | Action | Operator |
|------|--------|----------|
| 2026-06-03 16:44 UTC | Initial benchmark captured | Automated (PKE Transporter) |
| 2026-06-03 17:20"“18:19 | Disk write benchmark captured | Automated (dd + telemetry) |
| 2026-06-03 19:28 | VPS suspended (custody interrupted) | Hostinger (administrative action) |
| 2026-06-03 ~19:45 | Forensic report compiled | Auditor |
| 2026-06-04 | Support Acknowledgments documented | Auditor (from chat transcripts) |
| 2026-06-05 11:03 | Legal correspondence sent | Auditor |
| 2026-06-05 ~20:38 | Cloudflare report compiled | Auditor |
| 2026-06-07 | Audit package compiled and sealed | Auditor |

The primary evidence (benchmarks, support communications) was collected at the time of the events and has been preserved without modification. All seals were generated at the time of original file creation and verified during package compilation.

---

## 8.6 Forensic Standards Reference

This audit was conducted under the following forensic investigation standards:

- **ISO/IEC 27037:2012** "” Information technology "” Security techniques "” Guidelines for identification, collection, acquisition and preservation of digital evidence
- **NIST SP 800-86** "” Guide to Integrating Forensic Techniques into Incident Response

These standards govern the handling, preservation, and documentation of digital evidence to ensure its admissibility in legal and regulatory proceedings.


