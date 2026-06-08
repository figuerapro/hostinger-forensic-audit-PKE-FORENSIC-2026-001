# 05 — Support Communications

All communications documented below occurred between June 3 and June 5, 2026, through Hostinger's official support channels. Each confession has been preserved with its date, source, and literal transcription where available.

---

## Confession 1 — Suspension Without Cause

**Date:** June 3, 2026, ~19:30 UTC  
**Source:** Hostinger hPanel Management Interface  
**Status:** Terminal communication — no human response

> "Current action not allowed. VPS is in suspended state."

The VPS was suspended without prior notification or documented justification. No Acceptable Use Policy violation was cited. No email or ticket was generated to inform the customer of the action.

---

## Confession 2 — Fabricated Abuse Report (and Retraction)

**Date:** June 3, 2026, ~22:00 UTC  
**Source:** Hostinger Support (chat)

> "The service was suspended following an abuse report related to the server's activity."

**Retraction:** June 4, 2026, ~12:00 UTC  
**Source:** Muthi'a, Customer Success Team

> "I want to make an immediate correction regarding the reason for the suspension. No external abuse report exists, nor any legal infraction. The earlier mention of an 'abuse report' was an error on my part during the initial review."

**Significance:** Hostinger support initially justified the suspension with a nonexistent abuse report — and then explicitly retracted that justification in writing. No external report was ever cited, produced, or referenced by any third party.

---

## Confession 3 — Physical Environment is Shared

**Date:** June 4, 2026, ~12:00 UTC  
**Source:** Muthi'a, Customer Success Team

> "The suspension was triggered by sustained high resource usage that was affecting the stability of the host node."
>
> "Usage at 99–100% can affect the **shared physical environment.** "

**Significance:** This constitutes an explicit written admission that Hostinger's infrastructure is shared — directly contradicting the product page claim that "Your VPS resources are exclusively yours. No sharing with other users."

---

## Confession 4 — CPU at 100% for 24 Hours, RAM Killed by OOM

**Date:** June 4, 2026, ~12:00 UTC  
**Source:** Muthi'a, Customer Success Team

> "CPU: Sustained at 100% for the entire 24-hour period."
>
> "RAM: Stable at approximately 15 GB until around 4–5 a.m., then dropped drastically to ~2 GB. This is consistent with the OOM (Out of Memory) killer terminating processes."
>
> "Traffic: Nearly zero during most of the day, then a pronounced spike at the same time as the RAM drop."

**Significance:** This confirms that (a) Hostinger monitors customer CPU and RAM usage in real time at the process level, (b) the host OOM killer terminates customer processes when memory approaches the contracted limit, and (c) Hostinger's infrastructure cannot sustain 100% utilization of the resources it sells without intervention.

---

## Confession 5 — CPU Limitation on "Dedicated" Resources

**Date:** June 4, 2026, ~12:00 UTC  
**Source:** Hostinger hPanel + Muthi'a, Customer Success Team

> "CPU limitation activated. This may affect your VPS performance."
> — hPanel notification

> "I have proceeded to lift the CPU restriction for now. Please note that it may be reapplied in approximately 1 hour if CPU usage remains high."
> — Muthi'a

**Significance:** A CPU limitation was applied to resources marketed as "dedicated" and "exclusively yours." The restriction was temporarily lifted while the support agent warned it would be reapplied. This is functionally equivalent to selling a product and penalizing the customer for using it.

---

## Confession 6 — Forced System Restarts (4 in 48 Hours)

**Date:** June 3–4, 2026  
**Source:** `/proc/stat`, `last reboot`, system availability logs

| # | Approximate Time (UTC) | Cause | Customer Notification |
|---|----------------------|-------|:---------------------:|
| 1 | June 3, ~19:28 | Administrative suspension | None |
| 2 | June 3, ~22:56 | Full system restart | None |
| 3 | June 4, ~23:24 | Full system restart | None |
| 4 | June 5 | Credential revocation | None |

**Significance:** Four service interruptions in under 48 hours on a plan marketed with "99.9% uptime guarantee." None were preceded by customer notification.

---

## Confession 7 — Root Credential Revocation

**Date:** June 5, 2026  
**Source:** Direct access attempt

Following the final support communication, SSH authentication to 45.132.241.248 began returning `Permission denied (publickey,password)` for credentials that had been used successfully for 48+ hours. No notification of credential change was received by the customer. The root account was rendered inaccessible.

---

## Confession 8 — Port Exhaustion Confirmed

**Date:** June 3–5, 2026  
**Source:** Hostinger internal diagnosis shared with customer

> "Sequential ports at the limit (65,535). SYN_SENT awaiting response that never arrives. The machine has run out of available ports."

**Significance:** Hostinger's own infrastructure diagnosed port exhaustion as a consequence of the customer's legitimate workload. If resources were truly "dedicated," port exhaustion for a single tenant would not affect the host node. The fact that Hostinger identified this as a host-level concern confirms shared infrastructure.

---

## Summary: Written Admissions vs. Marketing Claims

| Support Admission | Contradicted Marketing Claim |
|------------------|------------------------------|
| "Shared physical environment" | "Dedicated resources — exclusively yours" |
| "CPU limitation activated" | "8 vCPU dedicated" |
| "OOM killer terminating processes" | "32 GB RAM guaranteed" |
| "Abuse report" → retracted | "24/7 professional support" |
| 4 interruptions in 48 hours | "99.9% uptime guarantee" |
| Root credentials changed without notice | "Full root access" |
| CPU monitored at process level | "Secure environment" |
| Port exhaustion affecting host node | "No sharing with other users" |

---

## Confession 9 — Agent Rani: Formal Defense / 6th Explanation

**Date:** June 8, 2026  
**Source:** Rani, Customer Success Team (formally escalated response)

> "The suspension was caused by sustained and abnormally high outbound network traffic, which triggered a network policy violation in our systems."

> **On self-management vs. monitoring:** "Self-managed means you have full control over everything inside your VPS: the operating system, software, files, and configurations. Hostinger does not access or modify anything within your VPS environment. However, like any hosting provider, we monitor network traffic at the infrastructure level (outside your VPS) to maintain network stability and security for all customers. Monitoring traffic at the network layer is not the same as accessing the internal elements of your VPS."

> **On the forensic components:** "These components (kdump/crashkernel, virtio_balloon, ttyS0, SeaBIOS, and the virtio QEMU disk) are standard parts of KVM virtualization technology, which is the industry-standard hypervisor used by practically all VPS providers. They are part of the virtualization layer that makes your VPS work as an isolated server — not something specific to your account or added by us."

**Forensic significance — four contradictions within a single message:**

1. **"No access to VPS" contradicted by their own Server Usage data.** Muthi'a provided process-level metrics on June 4 (CPU at 100% for 24h, RAM dropping from 15GB to 2GB, OOM killer events correlated with traffic spikes). CPU percentage, RAM gigabytes, and OOM killer events originate from within the guest operating system. They cannot be obtained from "network traffic at the infrastructure level (outside your VPS)." Either Hostinger has access to internal VPS metrics, or their Server Usage panel displays fabricated data. Both cannot be true.

2. **All forensic findings confirmed by name.** The agent explicitly confirms the existence of kdump/crashkernel, virtio_balloon, ttyS0, SeaBIOS firmware, and the virtio QEMU (IDE) storage device on the provisioned VPS. Every finding in the forensic audit is acknowledged as present — the dispute is only over their characterization.

3. **No disclosure of "standard" components.** While describing these components as "standard parts of KVM," the agent provides no evidence that they are disclosed to customers prior to purchase. The KVM 8 product page, Terms of Service, and pre-purchase documentation contain no mention of ttyS0 serial console access, crashkernel memory reservation, balloon driver RAM reclamation, or IDE-emulated storage. A component cannot be "standard" and simultaneously undisclosed.

4. **6th agent, maintaining the 6th version.** Rani is the 6th Hostinger employee to address this case. The claimed cause — "network traffic" — is the same as Carola's 5th version, which was contradicted by Muthi'a's own Server Usage data showing "traffic: nearly zero." The escalation to "the technical team" that Rani references produced no new evidence — only a restatement of a previously contradicted claim.

---

## Agent Identification

| Name | Role | Dates Active |
|------|------|-------------|
| Aymene | Support Agent | June 3, 2026 |
| Muthi'a | Quality & Development / Customer Success Team | June 3–4, 2026 |
| Carola | Support (secondary contact) | June 4–5, 2026 |
| Raminta | Team Lead | June 5, 2026 |
| Rani | Customer Success Team (escalated response) | June 8, 2026 |

All communications occurred through Hostinger's official ticketing and live chat systems. Transcripts have been preserved with SHA-256 seals as part of the evidence package.

