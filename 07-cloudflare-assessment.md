# 07 — Cloudflare Infrastructure Assessment

This document documents observations regarding Cloudflare's role in Hostinger's infrastructure architecture. Its purpose is to provide context about the separation between Hostinger's protected corporate frontend and their exposed customer-facing infrastructure.

---

## 7.1 Hostinger's Cloudflare Dependency

Hostinger's public-facing web services are served behind Cloudflare's content delivery network. The following domains were observed to resolve through Cloudflare's Anycast network during the audit period:

| Domain | Function | Behind Cloudflare |
|--------|----------|:-----------------:|
| www.hostinger.com | Marketing website | Yes |
| hpanel.hostinger.com | Customer management panel | Yes |
| support.hostinger.com | Support portal | Yes |
| api.hostinger.com | API endpoint | Yes |

Cloudflare provides DDoS mitigation, caching, and IP obfuscation for these services.

---

## 7.2 What Cloudflare Does Not Protect

Cloudflare's protection extends only to Hostinger's corporate web properties — not to the customer-facing VPS and shared hosting infrastructure that Hostinger sells:

| Infrastructure | IP Address | Behind Cloudflare |
|----------------|-----------|:-----------------:|
| VPS KVM 8 (audited customer instance) | 45.132.241.248 | No |
| Shared hosting server (server1165) | 194.59.166.148 | No |
| Shared hosting server (server1168) | 195.35.39.114 | No |

These IPs are directly accessible on standard service ports (TCP 22, 80, 443, 3306, 8080) without any CDN-layer protection. Hostinger's actual hosting infrastructure — the service its customers pay for — operates on bare IP addresses with no Cloudflare proxy, no DDoS mitigation, and no IP obfuscation.

---

## 7.3 Access to Hostinger's Legal Pages

During the audit period, attempts to access Hostinger's own legal policy pages from the auditor's source IPs returned HTTP 403 via Cloudflare's Web Application Firewall:

| Page | URL | Status |
|------|-----|:------:|
| Universal Terms of Service | www.hostinger.com/legal/universal-terms-of-service-agreement | 403 |
| Privacy Policy | www.hostinger.com/privacy-policy | 403 |
| Acceptable Use Policy | www.hostinger.com/acceptable-use-policy | 403 |
| Refund Policy | www.hostinger.com/refund-policy | 403 |

This creates an asymmetric enforcement architecture: Hostinger deploys Cloudflare to protect its corporate presence and gatekeep access to its legal documentation, while customer-facing infrastructure operates on exposed IP addresses without equivalent protection. A customer whose service is restricted based on Hostinger's Terms of Service may be unable to access those same Terms to verify the basis of the restriction.

---

## 7.4 Architectural Asymmetry Summary

| Layer | Hostinger Corporate | Hostinger Customer VPS |
|-------|:-------------------:|:----------------------:|
| CDN / DDoS mitigation | Cloudflare (active) | None |
| IP obfuscation | Cloudflare proxy | Direct IP exposure |
| WAF | Cloudflare (active) | None |
| Legal documentation access | Gated behind WAF | N/A |
| Service ports exposed | 443 only (Cloudflare) | 22, 80, 443, 3306, 8080, and others |

