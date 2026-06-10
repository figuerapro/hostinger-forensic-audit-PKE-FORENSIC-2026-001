# 02 — Timeline of Events

All times in UTC unless otherwise noted.

---

## June 3, 2026 — Service Provisioned and Suspended

| Time | Event | Source |
|------|-------|--------|
| ~14:00 | KVM 8 VPS provisioned. Assigned: 45.132.241.248, hostname lam3roma.com. AlmaLinux 9.7. | Purchase receipt |
| 16:44 | Initial performance benchmark completed. BLAKE3 18.2 GB/s (8 threads). TCP loopback 3.5 Gbps. 70,104 connections. | `VPS_PHOENIX_CERTIFICADO.txt` |
| 17:20 | Disk write audit begins: 35 parallel `dd` processes, 1 MB blocks, 10 GB per writer. | `forensic-disk-benchmark-report.txt` |
| 17:37 | 35 GB written. Speed: 35 MB/s. | `forensic-disk-benchmark-report.txt` �Section 4 |
| 17:50 | 281 GB written. Speed: 159 MB/s (peak). | `forensic-disk-benchmark-report.txt` �Section 4 |
| 18:19 | Audit completes. 376 GB written in 59 min. Average: 108.7 MB/s. Disk: 399/400 GB (100%). Simultaneously: 1,025 BLAKE3 threads, 200 HTTP workers, 28 GB RAM allocated. System LOAD: 1,077. CPU idle: 0%. | `forensic-disk-benchmark-report.txt` �Section 4-5 |
| 18:54 | Extended compute workload begins (BLAKE3 hashing). | Session records |
| 18:56 | LOAD 1,399 (175× nominal). TCP sockets: 20,662. Steal time: 21.4%. OOM kill #14. 51 TB BLAKE3 processed in 1.5 hours. | Telemetry log |
| 19:17 | Steal time: 21.4%. OOM kills: 16. LOAD: 1,393. | Telemetry log |
| 19:25 | **Last recorded heartbeat.** LOAD: 2,949 (369× nominal). OOM kills: 18. Steal time: 21.1% (constant cap). Disk: 399/400 GB. | Telemetry log |
| 19:28 | **VPS becomes unreachable.** All ports timeout (TCP 22, 80, 443, 50112). Ping: 100% packet loss. | Connectivity tests |
| 19:30 | hPanel displays: "Current action not allowed. VPS is in suspended state." | Hostinger management panel |
| ~19:45 | Initial forensic report compiled (15 sections). SHA-256 sealed. | `FORENSIC_DENUNCIA_HOSTINGER.md` |
| ~22:00 | Hostinger support claims suspension was "due to an abuse report." | Support chat (Acknowledgment 2) |
| ~22:56 | **First forced VPS restart.** All customer processes terminated. | `/proc/stat`, `last reboot` |

---

## June 4, 2026 — Acknowledgments and Retractions

| Time | Event | Source |
|------|-------|--------|
| ~12:00 | Support agent Muthi'a (Customer Success Team) issues corrections: (a) "No external abuse report exists. My earlier mention was an error." (b) "The suspension was triggered by sustained high resource usage affecting the shared physical host." (c) "CPU sustained at 100% for the entire 24-hour period." (d) "RAM stable at ~15 GB until ~4-5 AM, then dropped to ~2 GB consistent with OOM killer terminating processes." (e) "Port exhaustion: 65,535. SYN_SENT awaiting response that never arrives." | Support chat (Acknowledgments 2-4, 8) |
| ~12:00 | CPU limitation applied to the VPS. hPanel shows: "CPU limitation activated. This may affect your VPS performance." | hPanel + Acknowledgment 5 |
| ~12:00 | Support lifts CPU restriction temporarily, warns it "may be reapplied in approximately 1 hour if CPU usage remains high." | Support chat (Acknowledgment 5) |
| ~23:24 | **Third forced VPS restart.** | `/proc/stat`, `last reboot` (Acknowledgment 6) |

---

## June 5, 2026 — Credential Revocation and Legal Escalation

| Time | Event | Source |
|------|-------|--------|
| — | **Root credentials changed without notification.** Access attempts with original credentials rejected. | SSH authentication failure (Acknowledgment 7) |
| 11:03 | Formal legal correspondence sent to legal@hostinger.com, compliance@hostinger.com, dpo@hostinger.com, with CC to support@hostinger.com and quality@hostinger.com. 24-hour deadline. | `CORREO_HOSTINGER_FINAL.md` |
| ~17:19 | Local workstation benchmark (SFDK, 16 threads). BLAKE3: 11.59 GB/s. | `CERTIFICADO_RENDIMIENTO.txt` |
| ~20:38 | Cloudflare forensic report ("Operation Cielo en la Tierra") finalized. | `INFORME_FINAL_CIELO_EN_LA_TIERRA.md` |

---

## June 7, 2026 — Audit Package Publication

| Time | Event |
|------|-------|
| — | Complete forensic audit package compiled, dual-sealed with SHA-256 + BLAKE3, and published. |

---

## June 8, 2026 — Agent Rani: Formal Defense / 6th Explanation (Acknowledgment 9)

| Event | Detail |
|-------|--------|
| **Agent** | Rani, Customer Success Team |
| **Claimed cause** | "An abnormally high and sustained outbound network traffic rate" — maintaining Carola's 6th version |
| **Key admission #1** | "Hostinger does not access or modify anything within your VPS. However, we monitor network traffic at the infrastructure level." — This contradicts Muthi'a's Server Usage data showing CPU%, RAM GB, and OOM killer events tracked at process level within the guest OS. These metrics cannot be obtained from "outside the VPS." |
| **Key admission #2** | "These components (kdump/crashkernel, virtio_balloon, ttyS0, SeaBIOS, and the virtio QEMU disk) are standard parts of KVM virtualization technology... not something specific to your account or added by us." — This confirms every forensic finding while dismissing their significance. The agent provides no explanation for why these components are never disclosed to customers. |
| **Contradiction** | Claims "no access to VPS" while simultaneously acknowledging possession of internal VPS metrics that require guest-level access. The Server Usage panel provided by Muthi'a on June 4 tracked CPU at 100% for 24h, RAM dropping from 15GB to 2GB, and OOM killer events — all of which originate from within the guest operating system, not from "network traffic at the infrastructure level." |
| **Contradiction count** | Now **6 agents** have provided **6 different explanations** for the same suspension in 5 days. |

---

| June 9, 2026 | Muthi'a — 3rd retraction. Admits "reference to CPU usage was an error." Clarifies: observed zero traffic was post-suspension. Cites new legal basis: Section 4 of Hosting Agreement (3rd different provision). Does not address steal time, dashboard falsification, or systemic convergence. | Support chat (Acknowledgment 10) |

| June 9, 2026 | SAE Team (Muthi'a) — **Acknowledgment 11.** Confirms CPU limitation active for "more than a week" — BEFORE the first suspension. Admits steal time caused by Hostinger's own CPU rate limit, NOT shared infrastructure (contradicting Aymene's June 3 statement). Explains hPanel/top discrepancy as "delivered vs demanded" — without customer disclosure. Provides "Remove Limitation" button instructions for resources sold as "dedicated." Does not address why the limitation exists on dedicated resources. | Support chat (Acknowledgment 11) |

## Summary of Documented Interventions by Hostinger

| # | Intervention | Date | Notification |
|---|-------------|------|-------------|
| 1 | Administrative suspension of VPS | Jun 3, 19:28 | None |
| 2 | Fabrication of "abuse report" as justification | Jun 3, ~22:00 | False statement to customer |
| 3 | Retraction of fabricated abuse report | Jun 4, ~12:00 | Written admission of error |
| 4 | CPU limitation applied to "dedicated" resources | Jun 4, ~12:00 | hPanel notification |
| 5 | Forced system restart (1st) | Jun 3, ~22:56 | None |
| 6 | Forced system restart (2nd) | Jun 4, ~23:24 | None |
| 7 | Root credential change | Jun 5 | None |
| 8 | SSH access blocked (all servers) | Jun 5–7 | None |

---

## Escalating Contradictions — 6 Agents, 6 Explanations

| # | Date | Agent | Cause Given |
|---|------|-------|-------------|
| 1 | Jun 3 | Aymene | "Abuse report" — RETRACTED |
| 2 | Jun 3 | Aymene | High CPU affecting "shared physical environment" |
| 3 | Jun 4 | Muthi'a | "CPU >180 continuous minutes" |
| 4 | Jun 4 | Muthi'a | "No resource contention at infrastructure level" |
| 5 | Jun 5 | Carola | "Abnormal outbound network traffic" |
| 6 | Jun 8 | Rani | "Outbound network traffic" + formal defense: all components are "standard KVM" |


