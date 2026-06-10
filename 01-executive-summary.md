# 01 — Executive Summary

## Discovery

On June 3, 2026, a Hostinger KVM 8 virtual private server (VPS) was provisioned at IP address 45.132.241.248. The plan's product page listed the following specifications: 8 vCPUs, 32 GB RAM, 400 GB NVMe SSD storage, 32 TB monthly bandwidth, and "dedicated resources — exclusively yours."

A standard performance benchmark was executed on the VPS. The CPU model was identified as an AMD EPYC 7543P (Zen 3 Milan, 32 physical cores). A sustained multi-threaded cryptographic hashing workload (BLAKE3) was then run as a legitimate compute benchmark. The workload consumed approximately 78% of the 8 vCPU allocation and 96% of the 32 GB RAM — both within the limits of the contracted plan.

Within 4 hours, the VPS was **administratively suspended** by Hostinger. No prior warning was issued. No violation of the Acceptable Use Policy was cited. The hPanel management interface displayed only: "VPS is in suspended state."

When the suspension was challenged through support channels, Hostinger support initially claimed the suspension was "due to an abuse report." Twenty-four hours later, the same support agent **retracted this claim in writing**, stating: "The earlier mention of an 'abuse report' was an error on my part during the initial review." No external abuse report had ever existed.

---

## Methodology

The forensic audit was conducted from within the VPS operating system (AlmaLinux 9.7, kernel 5.14.0) via direct SSH root access. The following data sources were examined:

| Source | Data collected |
|--------|---------------|
| `/proc/cpuinfo` | CPU model, flags, topology |
| `/proc/stat` | CPU time distribution (user, system, steal, idle) |
| `lsblk`, `/sys/block/sda/queue/` | Block device identification and characteristics |
| `dmesg` | Kernel ring buffer (OOM events, hardware enumeration) |
| `lsmod` | Loaded kernel modules (balloon driver detection) |
| `/proc/cmdline` | Boot parameters (crashkernel allocation, serial console) |
| `dmidecode` | BIOS/firmware version and system information |
| `top`, `uptime`, `free` | Real-time resource metrics |
| `df -h` | Disk utilization |
| `dd` benchmark | Sustained write throughput (35 writers, 1 MB blocks, 10 GB each) |
| Hostinger hPanel | Dashboard metrics vs. kernel-level measurements |
| Hostinger support chat | Written communications with support agents |

Data was collected in real time during the operation and documented with UTC timestamps. All evidence files are sealed with SHA-256 and BLAKE3 cryptographic hashes under ISO/IEC 27037:2012 forensic guidelines.

---

## Significance

Three categories of findings emerged from the audit:

**1. Systematic overselling of infrastructure.** The physical AMD EPYC 7543P processor serves multiple VPS tenants simultaneously. A constant 21% steal time was measured, indicating that the hypervisor was reallocating approximately 1.7 of the 8 purchased vCPUs to other tenants at all times. The storage device is a QEMU-emulated IDE disk, not an NVMe SSD — sustained write throughput of 108.7 MB/s is consistent with shared rotational storage or throttled virtual disk I/O, not dedicated NVMe flash (which typically delivers 3,000+ MB/s). The provisioned 1 Gbps pipe is shared among multiple tenants. Under realistic shared conditions, effective per-tenant throughput is substantially below the pipe's theoretical ceiling, placing the advertised 32 TB beyond practical reach for the typical customer.

**2. Insecure hypervisor configuration exposing customer data.** The VPS was provisioned with an active virtual serial console (ttyS0 via `systemd-serial-getty`), a kernel crash dump reservation (`crashkernel=512M`), and the VirtIO balloon driver (`virtio_balloon`) loaded — each of which enables the hosting provider to access, extract, or manipulate the customer's runtime environment without notification or audit trail.

**3. Inconsistent support practices.** When confronted with evidence of the unjustified suspension, support personnel fabricated an "abuse report" as justification, then retracted it. During the same exchange, they confirmed that the "physical environment is shared," that CPU limitation had been applied to resources sold as "dedicated," and that the host's OOM killer had terminated the customer's processes — all of which directly contradict the product's marketing claims.

This audit provides a complete, verifiable record of the discrepancy between Hostinger's advertised KVM 8 specifications and the infrastructure actually provisioned.


