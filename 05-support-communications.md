# 05 — Support Communications

All communications documented below occurred between June 3 and June 5, 2026, through Hostinger's official support channels. Each Acknowledgment has been preserved with its date, source, and literal transcription where available.

---

## Acknowledgment 1 — Suspension Without Cause

**Date:** June 3, 2026, ~19:30 UTC  
**Source:** Hostinger hPanel Management Interface  
**Status:** Terminal communication — no human response

> "Current action not allowed. VPS is in suspended state."

The VPS was suspended without prior notification or documented justification. No Acceptable Use Policy violation was cited. No email or ticket was generated to inform the customer of the action.

---

## Acknowledgment 2 — Fabricated Abuse Report (and Retraction)

**Date:** June 3, 2026, ~22:00 UTC  
**Source:** Hostinger Support (chat)

> "The service was suspended following an abuse report related to the server's activity."

**Retraction:** June 4, 2026, ~12:00 UTC  
**Source:** Muthi'a, Customer Success Team

> "I want to make an immediate correction regarding the reason for the suspension. No external abuse report exists, nor any legal infraction. The earlier mention of an 'abuse report' was an error on my part during the initial review."

**Significance:** Hostinger support initially justified the suspension with a nonexistent abuse report — and then explicitly retracted that justification in writing. No external report was ever cited, produced, or referenced by any third party.

---

## Acknowledgment 3 — Physical Environment is Shared

**Date:** June 4, 2026, ~12:00 UTC  
**Source:** Muthi'a, Customer Success Team

> "The suspension was triggered by sustained high resource usage that was affecting the stability of the host node."
>
> "Usage at 99–100% can affect the **shared physical environment.** "

**Significance:** This constitutes an explicit written admission that Hostinger's infrastructure is shared — directly contradicting the product page claim that "Your VPS resources are exclusively yours. No sharing with other users."

---

## Acknowledgment 4 — CPU at 100% for 24 Hours, RAM Killed by OOM

**Date:** June 4, 2026, ~12:00 UTC  
**Source:** Muthi'a, Customer Success Team

> "CPU: Sustained at 100% for the entire 24-hour period."
>
> "RAM: Stable at approximately 15 GB until around 4–5 a.m., then dropped drastically to ~2 GB. This is consistent with the OOM (Out of Memory) killer terminating processes."
>
> "Traffic: Nearly zero during most of the day, then a pronounced spike at the same time as the RAM drop."

**Significance:** This confirms that (a) Hostinger monitors customer CPU and RAM usage in real time at the process level, (b) the host OOM killer terminates customer processes when memory approaches the contracted limit, and (c) Hostinger's infrastructure cannot sustain 100% utilization of the resources it sells without intervention.

---

## Acknowledgment 5 — CPU Limitation on "Dedicated" Resources

**Date:** June 4, 2026, ~12:00 UTC  
**Source:** Hostinger hPanel + Muthi'a, Customer Success Team

> "CPU limitation activated. This may affect your VPS performance."
> — hPanel notification

> "I have proceeded to lift the CPU restriction for now. Please note that it may be reapplied in approximately 1 hour if CPU usage remains high."
> — Muthi'a

**Significance:** A CPU limitation was applied to resources marketed as "dedicated" and "exclusively yours." The restriction was temporarily lifted while the support agent warned it would be reapplied. This is functionally equivalent to selling a product and penalizing the customer for using it.

---

## Acknowledgment 6 — Forced System Restarts (4 in 48 Hours)

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

## Acknowledgment 7 — Root Credential Revocation

**Date:** June 5, 2026  
**Source:** Direct access attempt

Following the final support communication, SSH authentication to 45.132.241.248 began returning `Permission denied (publickey,password)` for credentials that had been used successfully for 48+ hours. No notification of credential change was received by the customer. The root account was rendered inaccessible.

---

## Acknowledgment 8 — Port Exhaustion Confirmed

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

## Acknowledgment 9 — Agent Rani: Formal Defense / 6th Explanation

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

## Acknowledgment 10 — Agent Muthi'a: Third Retraction, New Legal Basis

**Date:** June 9, 2026  
**Source:** Muthi'a, Customer Success Team (formally escalated technical review)

> "Part of the previously provided information was not accurate and I should have verified it more thoroughly before responding. After consulting with our technical team, here is the precise and complete information."

> "The reference to CPU usage as the reason for suspension was an error on my part, and I apologize for the confusion this caused. The precise reason throughout our technical review has been the network traffic event of June 3."

> "The traffic near zero observed on June 4 is a direct result of the suspension taking effect — not a contradiction of it."

> "Regarding the QEMU disk and NVMe: what your guest OS reports as a QEMU virtual disk is the virtualization layer, not the physical hardware. In KVM environments, the guest OS always sees a virtual disk controller regardless of the physical backend; the physical backend is NVMe. These are two separate layers."

> "Intel RAPL on AMD EPYC: This is standard QEMU/KVM behavior for compatibility purposes. It has no impact on your server's actual CPU performance."

> "The suspension was based on Section 4 of the Hosting Agreement accepted at registration. Hostinger reserves the right to remove or suspend services when activities threaten the stability of our network."

**Forensic significance — five findings:**

1. **Third retraction.** Muthi'a explicitly admits that "the reference to CPU usage as the reason for suspension was an error on my part." This is now three separate occasions where a Hostinger agent has acknowledged providing incorrect information: (a) Aymene retracted the "abuse report" on June 3, (b) Muthi'a retracted "CPU >180 min" on June 9, (c) Muthi'a acknowledged "part of the previously provided information was not accurate." A support process with three documented retractions in 6 days cannot be considered reliable.

2. **Third legal basis.** The suspension justification has shifted from "abuse report" → "CPU >180 min (Section 6.4 ToS)" → "network traffic (Section 4 Hosting Agreement)." This is now three different legal provisions cited for the same suspension — none of which were communicated to the customer at the time of the action.

3. **Traffic timing correction — valid.** The customer's observation of "traffic near zero" on June 4 is correctly identified as occurring after the suspension. This is a valid correction to the audit's earlier analysis. The contradiction between Carola's "abnormal traffic" claim and Muthi'a's "traffic nearly zero" observation is resolved: the zero traffic was a consequence of the suspension, not evidence contradicting the cause.

4. **NVMe claim — unverifiable.** The assertion that "the physical backend is NVMe" cannot be reconciled with the sustained write benchmark of 108.7 MB/s over 59 minutes and 376 GB. If the backend is NVMe, the virtualization layer is reducing 96.4% of the storage performance — which contradicts the product page's representation of NVMe-grade storage. A customer purchasing "400 GB NVMe" has no way to distinguish between "NVMe backend with catastrophic virtualization overhead" and "no NVMe at all." The delivered performance is functionally identical to rotational storage.

5. **Unaddressed findings.** Muthi'a's response does not address: steal time of 0-53% on "dedicated" vCPUs; the hPanel dashboard reporting 9% CPU when the kernel measures 98%; the convergence of 3 independent services on the same AMD EPYC 7543P processor with identical configurations; or the systemic pattern across all audited services.

---

## Acknowledgment 11 — SAE Team: CPU Limitation Active for Over a Week

**Date:** June 9, 2026
**Source:** Hostinger SAE Team (escalated technical review via Muthi'a)

> "The reason you see CPU steal is because the virtual machine has been limited in CPU for more than a week. When the CPU limit is reached, the hypervisor withholds CPU cycles from your VM. From inside the VM, this appears as CPU steal (%st in top); your vCPUs are waiting for cycles that the hypervisor is not delivering due to the active limit."

> "hPanel reports delivered CPU usage (what the hypervisor is actually providing), while tools like top inside the VM report demanded CPU usage (what your processes are attempting to use). Both readings are accurate: they measure different things. This is not falsified data."

> "The CPU limitation active at the time of the test affected your results. The same CPU limitation is the reason you could not perform the disk benchmark correctly; the overall resource restrictions active at the time of the test affected your results."

> "If CPU usage remains at a high level (e.g., ~90% for 24 hours), the CPU rate limit will be applied and your effective CPU allocation will be reduced. hPanel will display a notification banner when this is active."

> "You can remove this limitation by navigating to hPanel and clicking the Remove Limitation button. After that, the steal issue and the I/O speed issue will be resolved."

**Forensic significance — four key admissions:**

1. **CPU limitation was active BEFORE the first suspension.** The SAE team confirms the limitation "has been active for more than a week." The disk benchmark occurred on June 3 between 17:20-18:19 UTC. The first suspension occurred at 19:28 UTC. If the CPU limitation was already active during the benchmark, then Hostinger was silently throttling resources sold as "dedicated" BEFORE any notification, BEFORE any suspension, and BEFORE any communication to the customer. This contradicts the earlier narrative that the suspension was triggered by the customer's resource usage — the resources were already being limited before the usage that supposedly caused the suspension.

2. **Steal time caused by Hostinger's own limitation, not by shared infrastructure.** The SAE team explicitly states that the 0-53% steal time was caused by their CPU rate limit, not by other tenants on a shared host. This directly contradicts agent Aymene's statement on June 3, 18:56 UTC: *"un uso elevado y sostenido al límite (99-100%) puede afectar al entorno físico COMPARTIDO... otros usuarios en la misma infraestructura."* ("sustained high usage at the limit can affect the SHARED physical environment... other users on the same infrastructure.") The SAE team's explanation and Aymene's explanation are mutually exclusive. One of them is false.

3. **"Remove Limitation" button on "dedicated" resources.** Hostinger sells "8 dedicated vCPUs — exclusively yours" and simultaneously provides a "Remove Limitation" button in hPanel to remove CPU throttling from those same "dedicated" resources. The existence of the button proves the limitation exists. The limitation existing proves the resources are not dedicated. A customer should not need to click a button to receive the resources they were sold.

4. **Dashboard discrepancy confirmed and reframed.** The SAE team acknowledges that hPanel and `top` report different CPU percentages. Their explanation — "delivered vs. demanded" — is technically plausible. However, at no point in the purchase process, product documentation, or Terms of Service is this distinction disclosed to the customer. A customer buying "8 dedicated vCPUs" has no way to know that the hPanel dashboard displays only the throttled allocation, not the actual CPU demand. The distinction may be technically accurate. The non-disclosure of the distinction is not.

---

## Agent Identification

| Name | Role | Dates Active |
|------|------|-------------|
| Aymene | Support Agent | June 3, 2026 |
| Muthi'a | Quality & Development / Customer Success Team | June 3–4, June 9, 2026 |
| Carola | Support (secondary contact) | June 4–5, 2026 |
| Raminta | Team Lead | June 5, 2026 |
| Rani | Customer Success Team (escalated response) | June 8, 2026 |
| SAE Team | Escalated Technical Review | June 9, 2026 |

All communications occurred through Hostinger's official ticketing and live chat systems. Transcripts have been preserved with SHA-256 seals as part of the evidence package.


