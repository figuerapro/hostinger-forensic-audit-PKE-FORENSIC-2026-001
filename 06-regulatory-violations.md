# 06 â€” Regulatory Violations

This document identifies applicable regulations in relevant jurisdictions and maps the documented findings to specific legal provisions. This is a technical analysis, not legal advice. Consult qualified legal counsel in the relevant jurisdiction before initiating any proceeding.

---

## 6.1 European Union

Hostinger International Ltd. is headquartered in Kaunas, Lithuania (Jonavos g. 60c, LT-44192), and operates within the European Union's regulatory framework.

### Digital Services Act â€” Regulation (EU) 2022/2065

| Article | Requirement | Observed Violation |
|---------|------------|-------------------|
| **Art. 17** | Hosting providers must provide a "clear and specific statement of reasons" (SOR) to any affected user when restricting service provision. The SOR must include: the consequences of the decision, its territorial scope and duration, the facts and circumstances relied on, and information on redress possibilities. | Service was suspended with zero notification. No statement of reasons was provided. When challenged, a fabricated "abuse report" was cited, then retracted. No SOR was ever issued. |
| **Art. 21** | Providers must notify users of content moderation decisions or service restrictions with specific grounds for the decision. | No notification was issued before, during, or after the suspension. The customer discovered the suspension by attempting to access the VPS. |
| **Art. 12** | Contact points must be provided for direct communication between users and the provider. | Support communications were evasive, contradictory, and ultimately resulted in a retracted justification. |

### Unfair Commercial Practices Directive â€” 2005/29/EC

| Provision | Requirement | Observed Violation |
|-----------|------------|-------------------|
| **Art. 6(1)** | A commercial practice is misleading if it contains false information or deceives the average consumer regarding the main characteristics of the product. | Product pages claim "dedicated resources" and "exclusively yours." Support confirmed in writing that the "physical environment is shared." |
| **Art. 6(1)(b)** | False claims about "the main characteristics of the product, such as its benefits, risks, execution, composition, accessories, after-sale customer assistance, complaint handling..." | "400 GB NVMe" vs. QEMU HARDDISK IDE at 108.7 MB/s. "32 TB bandwidth" vs. 1 Gbps pipe (mathematically unattainable). |

### General Data Protection Regulation â€” Regulation (EU) 2016/679

| Article | Requirement | Observed Violation |
|---------|------------|-------------------|
| **Art. 5(1)(c)** | Data minimization â€” personal data must be "adequate, relevant and limited to what is necessary." | Support agent reported real-time monitoring of CPU, RAM, and process-level metrics, indicating more extensive monitoring than necessary for service provision. |
| **Art. 6** | Processing must have a lawful basis. | Monitoring of customer VPS processes and resource consumption at the level described by support may exceed the lawful basis of "performance of a contract." |
| **Art. 32** | Security of processing â€” appropriate technical and organizational measures. | Active ttyS0 serial console, crashkernel reservation, and balloon driver represent inadequate isolation between provider and customer data. |

---

## 6.2 Republic of Lithuania (National)

As the jurisdiction of Hostinger's legal domicile, Lithuanian law provides additional consumer protections.

### State Consumer Rights Protection Authority (VVTAT)

The VVTAT mediates consumer disputes, investigates unfair commercial practices, and can impose corrective measures on companies operating in Lithuania.

| Violation | Basis |
|-----------|-------|
| Misleading advertising | Product claims of "dedicated" resources contradicted by written staff admission of "shared" environment |
| Failure to provide service as described | Storage delivered as QEMU HARDDISK IDE, not NVMe |
| Unjustified service restriction | Suspension without cause, with fabricated justification subsequently retracted |

**Contact:** https://vvtat.lrv.lt/en/ | tarnyba@vvtat.lt | +370 5 262 6751

---

## 6.3 United States

### Federal Trade Commission Act â€” 15 U.S.C. ÂSection  45 (Section 5)

| Provision | Requirement | Observed Violation |
|-----------|------------|-------------------|
| **Section 5(a)** | "Unfair or deceptive acts or practices in or affecting commerce are declared unlawful." | Representing shared, throttled infrastructure as "dedicated resources" constitutes a deceptive act under Section 5. The documented discrepancies between marketed and delivered specifications represent a pattern of misrepresentation. |

**FTC Complaint Portal:** https://reportfraud.ftc.gov/

### State-Level Consumer Protection

The customer entity (Panorama Makers Hub LLC) is registered in New Mexico, USA. New Mexico's Unfair Practices Act (NMSA ÂSection  57-12-1) prohibits "unfair or deceptive trade practices" including false representation of goods or services.

**New Mexico Attorney General Consumer Protection:** https://www.nmag.gov/contact-us/file-a-complaint/

---

## 6.4 ICANN Registrar Accreditation Agreement

Hostinger operations, UAB holds ICANN accreditation as a domain name registrar (IANA ID: 1636).

### RAA 2013 â€” Specifications

| Provision | Requirement |
|-----------|------------|
| **Specification 4, ÂSection  2.2** | Registrar must maintain accurate contact information and respond to abuse complaints within reasonable timeframes. |
| **Specification 4, ÂSection  2.5** | Registrar must investigate and respond to reports of inaccurate registration data. |

While ICANN's jurisdiction is limited to domain registration and DNS abuse (phishing, malware, botnets, pharming, spam), the documented pattern of misrepresentation regarding the infrastructure underlying registered domains may constitute a material concern under the registrar's obligation to operate with integrity.

**ICANN Contractual Compliance:** https://icann-nsp.my.site.com/compliance/s/

---

## 6.5 Summary of All Identified Violations

| # | Jurisdiction | Law/Regulation | Violation | Evidence Reference |
|---|-------------|---------------|-----------|-------------------|
| 1 | EU | DSA Art. 17 | Suspension without statement of reasons | ÂSection 5, Acknowledgments 1-2 |
| 2 | EU | DSA Art. 21 | No notification of service restriction | ÂSection 5, Acknowledgment 1 |
| 3 | EU | UCPD 2005/29 Art. 6(1) | False advertising ("dedicated" vs. shared) | ÂSection 3.4, Acknowledgment 3 |
| 4 | EU | UCPD 2005/29 Art. 6(1)(b) | Misleading product claims (NVMe, bandwidth) | ÂSection 3.6, ÂSection 3.7 |
| 5 | EU | GDPR Art. 5(1)(c) | Disproportionate monitoring | ÂSection 3.4, Acknowledgment 4 |
| 6 | EU | GDPR Art. 32 | Inadequate security measures (ttyS0, crashkernel) | ÂSection 3.8 |
| 7 | Lithuania | VVTAT consumer law | Misleading advertising, unjustified restriction | ÂSection 3.4, Acknowledgment 3 |
| 8 | USA | FTC Act ÂSection 5 | Deceptive acts or practices | ÂSection 3.10 (summary comparison) |
| 9 | USA (NM) | NM Unfair Practices Act | False representation of services | ÂSection 3.10 |
| 10 | Global | ICANN RAA Spec 4 | Registrar integrity obligations | ÂSection 3 entire |

---

## 6.6 Recommended Escalation Sequence

1. **VVTAT (Lithuania)** â€” Home-country regulator, binding authority, fastest response window
2. **FTC (USA)** â€” Federal consumer protection, investigation precedent for tech/hosting
3. **EU DSA Coordinator (Lithuania)** â€” Digital Services Act enforcement within Member State
4. **GDPR Supervisory Authority (VDAI, Lithuania)** â€” Data protection violation regarding unauthorized monitoring
5. **ICANN Contractual Compliance** â€” Registrar-specific obligations regarding integrity of service


