# Support Chat Screenshots — Forensic Sequence

Each image documents a critical statement or contradiction by Hostinger Customer Success personnel during the 72-hour audit of KVM 8 VPS (45.132.241.248).

---

## Image 01 — hPanel CPU Limitation

**File:** `01-hpanel-cpu-limitation.jpg`  
**Date:** June 5, 2026

Hostinger hPanel dashboard showing **"CPU limitation activated"** warning on a VPS sold with "8 dedicated vCPUs." The restriction is applied automatically by Hostinger's systems regardless of actual resource exhaustion. This demonstrates that the provider can silently throttle resources marketed as "dedicated" and "exclusively yours."

---

## Image 02 — Agent Aymene: "Abuse Report" Fabrication

**File:** `02-aymene-abuse-report.jpg`

> **Aymene, Hostinger Support — June 3, 2026, 18:49 UTC**

Aymene informs the customer that the VPS was suspended following an "abuse report" related to server activity — not due to resource limits. The agent confirms reactivation and recommends reviewing processes to avoid permanent suspension from repeated abuse reports.

**Forensic significance:** This "abuse report" did not exist. The same agent retracted it 7 minutes later (documented in chat transcripts �Section 02). Fabricating a legal instrument as justification for service restriction and threatening "permanent suspension" constitutes deceptive support conduct.

---

## Image 03 — Agent Muthi'a: CPU >180 Minutes + Server Usage Graph

**File:** `03-muthia-cpu-180-minutes.jpg`

> **Muthi'a, Quality & Development Department — June 4, 2026, 00:13 UTC**

Muthi'a explains the VPS was administratively suspended due to sustained CPU usage exceeding the threshold for over 180 continuous minutes, affecting the stability of the physical node. The Server Usage graph shows CPU at 100% and that "all available CPU restarts have been used."

**Forensic significance:** This is the 3rd different explanation for the same suspension. Muthi'a confirms the existence of other customers "on the same physical node" — contradicting the product page claim of dedicated, exclusive resources. The graph proves CPU was at 100% — which should be permissible on "dedicated" vCPUs.

---

## Image 04 — Agent Muthi'a: Section 6.4 Terms of Service

**File:** `04-muthia-tos-section-6-4.jpg`

> **Muthi'a, Quality & Development Department — June 4, 2026, 00:13 UTC**

Muthi'a cites Section 6.4 of the Universal Terms of Service to justify the suspension for "using many resources." The agent provides steps for the customer to identify the process via SSH, optimize CPU usage with cpulimit/cgroups, scan for malware, and wait for automatic reactivation.

**Forensic significance:** Section 6.4 prohibits software "designed to, or capable of, using many resources" without defining what "many" means. On a plan sold as "8 dedicated vCPUs," using 8 vCPUs at 100% is using the purchased allocation — not "many" resources. The clause is a structural compliance trap: undefined thresholds enforced at the provider's sole discretion.

---

## Image 05 — Agent Carola: Network Traffic Cause + Standard KVM

**File:** `05-carola-network-traffic-standard-kvm.jpg`

> **Carola, Customer Success Team — June 5, 2026, 12:37 UTC**

Carola explains the suspension was due to an abnormally high and sustained outbound network traffic rate — a 5th different cause. She clarifies that the components identified by the forensic audit (crashkernel, balloon driver, serial console, firmware, virtualized disk) are all standard KVM virtualization components present on every VPS, not monitoring tools or access mechanisms of any kind.

**Forensic significance:** This is the 5th contradictory explanation for the same suspension. It is directly contradicted by Muthi'a's own Server Usage data from 12 hours earlier showing "traffic: nearly zero." Carola's confirmation that all forensic findings are "standard KVM components" confirms their existence while dismissing their significance — without explaining why these components are never disclosed to customers.

---

## Image 06 — Agent Carola: Self-Managed Claim

**File:** `06-carola-self-managed-claim.jpg`

> **Carola, Customer Success Team — June 5, 2026, 12:37 UTC**

Carola reiterates that the VPS is a self-managed service: the customer has full control over the operating system, applications, files, and configurations. Hostinger only manages the virtualization layer, hardware, and network infrastructure. She states: "We do not access or modify anything within your VPS."

**Forensic significance:** This claim of "full control" and "no access" is contradicted by: (a) the Server Usage panel showing 24-hour process-level monitoring, (b) the crashkernel reservation capturing customer RAM data, (c) the active ttyS0 serial console enabling unlogged hypervisor access, (d) SonicPanel PHP executing as root on boot, and (e) CPU limitation being applied and lifted by the provider at will.

---

## 5 Contradictory Explanations Summary

| # | Date (UTC) | Agent | Cause Given |
|---|-----------|-------|-------------|
| 1 | Jun 3, 18:49 | **Aymene** | "Abuse report" |
| 2 | Jun 3, 18:56 | **Aymene** | RETRACTION. "No abuse report — error. High CPU affecting SHARED physical environment." |
| 3 | Jun 4, 00:13 | **Muthi'a** | "CPU >180 continuous minutes. Customers on the same physical node." |
| 4 | Jun 4, 23:55 | **Muthi'a** | "No resource contention at infrastructure level" (contradicts #2, #3) |
| 5 | Jun 5, 12:37 | **Carola** | "Abnormally high outbound network traffic rate" (contradicted by Muthi'a's own data showing "traffic nearly zero") |

Full verbatim transcripts available in [`../transcripts/`](../transcripts/).

---

## Agents Identified

| Name | Role | Conversation Date |
|------|------|-------------------|
| **Aymene** | Support Agent | June 3, 2026 |
| **Muthi'a** | Quality & Development / Customer Success | June 3–4, 2026 |
| **Carola** | Customer Success | June 4–5, 2026 |
| **Raminta** | Team Lead | June 5, 2026 |


