# 09 — Compliance Deconstruction: Hostinger's Public Claims vs. Forensic Reality

This document cross-references Hostinger's published legal policies, marketing claims, and support conduct against the forensic evidence collected during the KVM 8 audit. Every Hostinger claim cited below is documented either through their publicly accessible pages or through verbatim support communications obtained during the audit.

**Note:** During the audit period, Hostinger's legal pages (Terms of Service, Privacy Policy, Refund Policy, Acceptable Use Policy) were inaccessible from the auditor's IP range — returning HTTP 403 via Cloudflare's WAF. This obstruction of access to the very policies used to justify the customer's suspension is documented as an independent finding (see �Section 07 Cloudflare Assessment).

---

## 9.1 VPS Product Page Claims — KVM 8

Hostinger's KVM 8 product page represents the following service specifications. These claims were archived prior to the suspension.

| Marketing Claim | Forensic Finding | Evidence |
|:----------------|:-----------------|:---------|
| **"8 vCPU dedicated"** | Steal time 0–53%. At 53% steal, effective vCPUs = 3.76. Support agent Aymene confirmed: *"puede afectar al entorno físico COMPARTIDO"* ("can affect the SHARED physical environment") — June 3, 2026, 18:56 UTC. | `/proc/stat` measurements; Support chat Section 2 (EN transcript) |
| **"32 GB RAM guaranteed"** | 18 OOM kills documented. Support agent Muthi'a confirmed: *"RAM dropped drastically to ~2 GB. This is consistent with the OOM killer terminating processes."* — June 4, 2026, 23:55 UTC. Balloon driver (`virtio_balloon`) active for provider-side RAM reclamation without notification. | `dmesg`, `free`, `lsmod`; Support chat Section 7 (EN transcript) |
| **"400 GB NVMe SSD"** | QEMU HARDDISK on 1996 IDE controller (`/dev/sda`). 512-byte logical sectors (NVMe native = 4,096 bytes). Sustained write: 108.7 MB/s vs. 3,000+ MB/s NVMe expected. 30-year-old PIIX3 IDE controller. | `lsblk`, `dd` benchmark (376 GB in 59 min), `/sys/block/sda/queue/` |
| **"32 TB bandwidth"** | 1 Gbps pipe maximum. Mathematical ceiling at 1 Gbps = ~324 TB/month theoretical. Shared among 5+ tenants. Realistic per-tenant: ~13 TB/month. The advertised 32 TB requires ~3+ Gbps sustained — not achievable. | Mathematical proof; `ethtool`; `virtio_net` bridge shared topology |
| **"Dedicated resources — exclusively yours"** | Support confirmed *"customers on the same physical node"* (Muthi'a, June 4, 00:13 UTC) and *"other users on the same infrastructure"* (Aymene, June 3, 18:56 UTC). Five different agents confirmed shared infrastructure across 72 hours of communication. | Support chat Sections 2, 5, 7, 9 |
| **"Full root access"** | Service was suspended for legitimate resource usage within contracted limits. Root credentials were subsequently changed without notification (Confession 7). CPU limitation was applied to "dedicated" resources with a warning of re-application. | Support chat Sections 5, 7; SSH access logs |
| **"99.9% uptime guarantee"** | Four service interruptions in 48 hours: administrative suspension (Jun 3, 19:28), two forced restarts (Jun 3 ~22:56, Jun 4 ~23:24), credential revocation (Jun 5). None preceded by customer notification. | Timeline �Section 02; Support chat Confession 6 |
| **"Modern KVM virtualization"** | i440FX + PIIX3 chipset (released: Q1 1996). BIOS firmware: QEMU Standard PC, dated 01/04/2014 — 12 years unpatched. Intel RAPL (Intel CPU feature) injected on AMD EPYC 7543P (AMD CPU). The chipset predates Google, Wi-Fi, USB 2.0, and the NVMe specification. | `dmidecode`, `lspci`, `dmesg`; Infrastructure Analysis �Section 3.3 |

---

## 9.2 Universal Terms of Service — Contradictions and Abusive Clauses

### 9.2.1 Section 6.4 — Resource Usage (Cited by Support)

Hostinger support agent **Muthi'a (June 4, 2026, 00:13 UTC)** cited Section 6.4 of the Universal Terms of Service as the basis for the customer's suspension:

> *"This is covered by Section 6.4 of our Universal Terms of Service Agreement, which prohibits use of the Services in a manner that contains or installs any code, file, or program designed to, or capable of, using many resources, interrupting, damaging, or limiting the functionality of any software or hardware."*

| Element | Analysis |
|---------|----------|
| **Vagueness** | "Many resources" is undefined. The clause does not specify what percentage of CPU, RAM, or disk constitutes prohibited usage. On a plan sold as "8 dedicated vCPUs," using 8 vCPUs at 100% is using the purchased allocation — not "many" resources. |
| **Intent requirement** | The clause targets software "designed to" use resources. The customer's workload was a BLAKE3 cryptographic hashing benchmark — a standard compute task, not software "designed to" damage infrastructure. The clause penalizes effect without intent, applied retroactively. |
| **Contradiction with product page** | Section 6.4 prohibits "using many resources." The product page sells "8 vCPU dedicated" and "32 GB RAM guaranteed." Using 100% of sold resources cannot simultaneously be a violation. |
| **DSA Article 14 compliance** | Under EU Digital Services Act Article 14, terms must be "clear, simple, intelligible, and unambiguous." Section 6.4 fails all four criteria: it does not define "many resources," does not specify thresholds, and uses subjective language ("designed to," "capable of") that grants the provider unilateral interpretive authority. |

### 9.2.2 Suspension Rights — "At Sole Discretion"

The Universal Terms grant Hostinger the right to suspend services *"at its sole discretion"* and *"without prior notice."*

| Concern | Detail |
|---------|--------|
| **EU Directive 93/13/CEE** | The Unfair Contract Terms Directive classifies as unfair any term that grants "the seller or supplier the right to determine whether the goods or services supplied are in conformity with the contract, or giving him the exclusive right to interpret any term of the contract." A suspension-at-discretion clause falls squarely within this prohibition. |
| **DSA Article 17** | Requires hosting providers to provide a *"clear and specific statement of reasons"* (SOR) to any affected user when restricting service. Hostinger provided no SOR. When challenged, support fabricated an abuse report, then retracted it. No SOR was ever issued in compliance with Article 17. |
| **Documented enforcement** | The auditor's VPS was suspended with zero notification. Five different agents provided five different causes over 48 hours. The initial cause — "abuse report" — was explicitly retracted by the agent who fabricated it. At no point was a formal, documented statement of reasons provided. |

### 9.2.3 "No Warranty / As-Is" Clause

The Terms state services are provided *"as is"* without warranty.

| Contradiction | Detail |
|---------------|--------|
| **"8 vCPU dedicated"** | A dedicated resource specification is an express warranty of capability. Marketing a specific allocation (8 vCPUs) creates a reasonable consumer expectation that the allocation will be available. Steal time of 0–53% renders this specification meaningless. |
| **"400 GB NVMe"** | Identifying a specific storage technology (NVMe) in marketing creates an express warranty of that technology. Delivering a QEMU IDE emulation with 512B sectors is a different product. |
| **"32 TB bandwidth"** | A specific numerical allocation creates an express warranty of capacity. The provisioned 1 Gbps shared pipe makes this allocation mathematically unattainable. |

The *"as-is"* clause cannot simultaneously operate while the marketing makes specific, quantified promises about CPU count, storage technology, and bandwidth allocation. Under EU consumer law, pre-contractual information forms part of the contract and overrides contradictory boilerplate.

---

## 9.3 Acceptable Use Policy — Structural Ambiguity

Hostinger's Acceptable Use Policy (AUP) prohibits, among other things, activity that *"interrupts, damages, or limits the functionality of any software or hardware."*

| The AUP Prohibits | Forensic Reality |
|-------------------|-----------------|
| Activity that "limits the functionality of hardware" | The customer ran a BLAKE3 hashing benchmark — a legitimate compute workload. No hardware was damaged. No third party was affected. The workload consumed 78% CPU and 96% RAM — within the plan's advertised limits. |
| "Excessive use" of CPU resources | Hostinger defines "excessive" nowhere. The plan sells 8 vCPUs. 100% of 8 vCPUs is 100% — not excessive. |
| Operation of "mining" software | BLAKE3 cryptographic hashing is a general-purpose cryptographic operation. It is used for file integrity verification, content-addressable storage, and forensic data sealing — not cryptocurrency mining. The auditor was performing cryptographic benchmarking, not mining. |

**The AUP's ambiguity is structurally problematic:** it prohibits vaguely defined behavior ("excessive use"), applies prohibitions retroactively to workloads that do not match the prohibited categories, and provides no quantitative thresholds against which a customer can measure compliance before using the service.

---

## 9.4 Privacy Policy — Active Surveillance Contradicting "Self-Managed" Claims

### 9.4.1 The "Self-Managed" Contradiction

Throughout the support interaction, Hostinger maintained that the VPS is a *"self-managed service"* where *"the customer has full control"* and Hostinger *"does not access or modify anything within the VPS"* (Carola, June 5, 2026, 12:37 UTC).

However, the following evidence demonstrates active monitoring and intervention:

| Hostinger Statement | Contradicted By | Evidence |
|--------------------|----------------|----------|
| "No accedemos ni modificamos nada dentro de su VPS" (Carola) | Muthi'a provided process-level Server Usage data: CPU at 100% for 24 hours, RAM killed by OOM killer, traffic spike location identified | Support chat Section 7 |
| "Self-managed service" (Muthi'a, Carola) | CPU limitation was applied automatically. Muthi'a lifted it, warning it would be reapplied if usage remained high | Support chat Sections 5, 7 |
| "Full control over everything inside the VPS" (Carola) | Crashkernel=512M reserves customer RAM for provider crash dumps. Balloon driver enables provider-side RAM reclamation. ttyS0 serial console allows provider access without SSH audit logging. SonicPanel PHP executes as root on boot without customer consent. | Infrastructure Analysis �Section 3.5, �Section 3.8 |
| "No son herramientas de monitoreo" (Carola) | Server Usage panel tracked CPU, RAM, disk, and network at process level with 24-hour granularity | Support chat Section 7 (Muthi'a's data summary) |

### 9.4.2 GDPR Implications

| GDPR Article | Requirement | Hostinger's Practice |
|-------------|-------------|---------------------|
| **Art. 5(1)(c)** — Data minimization | Processing must be adequate, relevant, and limited to what is necessary | Process-level monitoring of customer VPS exceeds what is necessary for infrastructure provision. Server Usage data captured at 24-hour granularity demonstrates surveillance, not maintenance. |
| **Art. 6** — Lawful basis for processing | Processing requires consent, contract performance, legal obligation, vital interests, public task, or legitimate interest | No consent was obtained for process-level monitoring. The monitoring exceeds the "performance of a contract" basis. The legitimate interest of "node stability" does not override the customer's right to privacy in a service sold as "dedicated" and "self-managed." |
| **Art. 12–14** — Transparency | Controller must inform data subject of processing activities | Hostinger's Privacy Policy does not disclose process-level monitoring of customer VPS instances. Customers are not informed that their CPU, RAM, disk, and network activity is tracked at 24-hour granularity. |
| **Art. 32** — Security of processing | Appropriate technical and organizational measures | ttyS0 serial console without audit logging. Crashkernel memory dumps capturing customer RAM without consent. BIOS firmware unpatched since 2014 (no Spectre/Meltdown/L1TF/MDS mitigations). These represent inadequate security measures under Article 32. |

---

## 9.5 Support Process Contradictions — "Abuse Report" as Legal Weapon

### 9.5.1 Fabrication of Legal Instrument

The "abuse report" cited by Agent Aymene (June 3, 2026, 18:49 UTC) as the cause of suspension did not exist. The agent retracted it 7 minutes later. This is documented in the support transcript (EN Section 2).

| Claim | Status | Source |
|-------|--------|--------|
| "Suspended following an abuse report" | **FABRICATED** | Aymene, June 3, 18:49 UTC |
| "No external abuse report exists. It was an error." | **RETRACTION** | Aymene, June 3, 18:56 UTC |

**Implication:** Hostinger's support process permits the fabrication of a legal instrument ("abuse report") as a justification for service restriction, then retracting it without consequence when challenged. The auditor was threatened with *"permanent suspension"* (Aymene, June 3, 18:49 UTC) based on a nonexistent report. The *"abuse report"* framing — suggesting third-party complaint, illegal activity, or law enforcement involvement — serves as an intimidation mechanism against customers who exercise their contractual rights.

### 9.5.2 Five Contradictory Explanations

| # | Agent | Date (UTC) | Cause Given | Status |
|---|-------|-----------|-------------|--------|
| 1 | Aymene | Jun 3, 18:49 | "Abuse report" | **RETRACTED** |
| 2 | Aymene | Jun 3, 18:56 | High CPU affecting shared environment | **CONTRADICTED by #4** |
| 3 | Muthi'a | Jun 4, 00:13 | CPU >180 minutes | **CONTRADICTED by #4** |
| 4 | Muthi'a | Jun 4, 23:55 | No resource contention exists | **CONTRADICTS #2, #3** |
| 5 | Carola | Jun 5, 12:37 | Abnormal outbound network traffic | **CONTRADICTED by Muthi'a's own data (#4 showing traffic nearly zero)** |

No single explanation is consistent with the others. One was explicitly retracted as fabricated. One is contradicted by the same agent's own data. The process for determining the cause of suspension is demonstrably broken — or deliberately obfuscated.

---

## 9.6 Hostinger.com Website Accessibility — Cloudflare Gatekeeping

During the audit period, attempts to access Hostinger's own legal policy pages from the auditor's source IPs returned **HTTP 403** via Cloudflare's Web Application Firewall:

| Page | URL | Status |
|------|-----|:------:|
| Universal Terms of Service | `www.hostinger.com/legal/universal-terms-of-service-agreement` | 403 |
| Privacy Policy | `www.hostinger.com/privacy-policy` | 403 |
| Acceptable Use Policy | `www.hostinger.com/acceptable-use-policy` | 403 |
| Refund Policy | `www.hostinger.com/refund-policy` | 403 |

**Implication:** Hostinger deploys Cloudflare to block access to its own legal policies from IP addresses it has flagged — while simultaneously citing those same policies to justify service restrictions. A customer who is denied access to the Terms of Service cannot verify whether the cited clause actually supports the restriction being imposed. The provider restricts service based on a policy the customer cannot access, enforced by staff who provide contradictory explanations for the restriction.

---

## 9.7 Host-Level Security Claims vs. Provisioned Configuration

| Security Area | Hostinger Claim | Actual Configuration | Risk |
|--------------|----------------|---------------------|------|
| **Isolation** | "Dedicated resources — exclusively yours" | 5+ tenants on shared EPYC 7543P. Steal time 0–53%. Balloon driver active for dynamic RAM reclamation. | Cross-tenant resource contamination |
| **Firmware** | "Modern KVM virtualization" | SeaBIOS QEMU 1.17.0 (2014-04-01). 12 years unpatched. No Spectre/Meltdown/L1TF/MDS mitigations. | Speculative execution attacks from neighboring VMs |
| **Access control** | "Full root access" (exclusive to customer) | ttyS0 serial console active. systemd-serial-getty running 24/7 providing hypervisor access without SSH authentication logging. | Undetectable provider access to customer environment |
| **Memory privacy** | "32 GB RAM dedicated" | crashkernel=512M reserves customer RAM for provider crash dumps. On kernel panic, complete RAM contents — including cryptographic keys and plaintext credentials — are captured by the provider. | Catastrophic data exposure on system crash |
| **Software control** | "Self-managed — customer controls everything inside VPS" | SonicPanel PHP code (ionCube obfuscated) executing as root on boot. Pre-configured IPTables with 20+ open ports. Unnecessary services (sendmail, rpcbind, gssproxy) running. | Unauthorized root-level code execution; expanded attack surface |
| **Monitoring disclosure** | "No son herramientas de monitoreo" (Carola: "they are not monitoring tools") | Server Usage panel provides 24-hour granular process-level monitoring of CPU, RAM, disk, and network activity. | Undisclosed customer surveillance |

---

## 9.8 Legal Exposure Summary

| Jurisdiction | Law/Regulation | Violation | Penalty/Consequence |
|-------------|---------------|-----------|-------------------|
| **EU** | Digital Services Act, Art. 17 | Failure to provide statement of reasons for service restriction | Enforcement by Digital Services Coordinator (Lithuania) |
| **EU** | Unfair Commercial Practices Directive 2005/29/EC, Art. 6 | Misleading advertising ("dedicated" vs. shared; "NVMe" vs. IDE) | Injunction, fines, corrective advertising |
| **EU** | Unfair Contract Terms Directive 93/13/CEE | Suspension at "sole discretion" without notice is an unfair contract term | Clause void ab initio — legally unenforceable |
| **EU** | GDPR Art. 5(1)(c), 6, 12–14, 32 | Undisclosed process-level monitoring; inadequate security measures | Fines up to €20M or 4% of annual global turnover |
| **EU** | Consumer Rights Directive 2011/83/EU | Pre-contractual information misleading as to main characteristics of the service | Right to remedies including price reduction or contract termination |
| **USA** | FTC Act �Section 5 | Unfair or deceptive acts or practices | Civil penalties up to $50,120 per violation; injunctive relief |
| **USA (NM)** | New Mexico Unfair Practices Act �Section 57-12-2 | False or misleading representation of goods or services | Treble damages, attorney's fees, injunctive relief |
| **Lithuania** | Civil Code Art. 6.228 | Breach of contract — failure to deliver services as described | Damages, specific performance, contract termination |
| **Lithuania** | Law on Advertising, Art. 5 | Misleading advertising | Fines, corrective publication order |
| **Global** | ICANN RAA 2013, Specification 4 | Registrar operating without integrity regarding infrastructure claims | Compliance inquiry, potential accreditation review |

---

## 9.9 Conclusion

Hostinger's public-facing claims — across its product page, Terms of Service, Acceptable Use Policy, Privacy Policy, and support communications — present a systematically misleading picture of the service delivered:

1. **The product is misrepresented.** "Dedicated" resources are shared. "NVMe" storage is IDE emulation from 1996. "32 TB bandwidth" is mathematically unattainable on the provisioned infrastructure.

2. **The Terms of Service are structurally abusive.** Vague prohibitions, undefined thresholds, unilateral suspension rights, and an "as-is" clause that conflicts with specific marketing promises create a contract designed to be enforced only against the customer, never against the provider.

3. **The support process is deceptive.** A fabricated "abuse report" was used to justify service restriction, then retracted when challenged. Five contradictory explanations were provided for the same suspension. No agent provided a documented, verifiable cause.

4. **Privacy is misrepresented.** The VPS is actively monitored at the process level while being marketed as "self-managed" — a contradiction that undermines the customer's ability to trust the environment with sensitive data.

5. **Cloudflare is deployed to gatekeep access to legal policies.** A customer who is restricted or suspended cannot verify the terms under which the restriction was applied — because the terms themselves are behind a WAF that blocks the customer's IP.

This document establishes that Hostinger's compliance posture is not merely deficient — it is structurally designed to mislead customers about the nature of the service while providing the provider with unlimited discretionary power to restrict, suspend, or terminate service without accountability.

---

**Cross-reference:** This document should be read alongside `03-infrastructure-analysis.md` (technical findings), `05-support-communications.md` (agent confessions), `06-regulatory-violations.md` (legal framework), and `07-cloudflare-assessment.md` (WAF obstruction of legal page access).

