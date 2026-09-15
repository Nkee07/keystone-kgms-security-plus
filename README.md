# Keystone Grants Management System (KGMS): CompTIA Security+ SY0-701 Domain Portfolio
### All five exam domains, applied to one simulated federal system

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

![Cert](https://img.shields.io/badge/CompTIA-Security%2B%20SY0--701-CC0000?style=flat-square&logo=comptia&logoColor=white)
![Domains](https://img.shields.io/badge/Domains-5%20of%205-CC0000?style=flat-square)
![Standard](https://img.shields.io/badge/Mapped%20to-NIST%20SP%20800--53%20Rev%205-003087?style=flat-square)
![Simulated](https://img.shields.io/badge/Simulated-Fictional%20System-6B7280?style=flat-square)
![Author](https://img.shields.io/badge/Author-Nkeiru%20Sarah%20Adesida-00C853?style=flat-square)

---

## What this repository is

CompTIA Security+ certifies broad security knowledge across five domains. Holding the
certification proves the exam was passed. This repository is the different and harder
claim: **that the knowledge behind each domain can be applied to a specific system and
produce something an employer can use.**

Plain language version: a driving licence proves you passed a test. It does not show
anyone how you actually drive. This repository is the drive. Every one of the five exam
domains is worked through against the same fictional federal system used in the rest of
this portfolio, so each concept arrives attached to a real decision rather than a
definition.

**Why this repository exists alongside the GRC work.** Governance, Risk and Compliance is
where I work, and the other three repositories are that work. But a GRC analyst who cannot
follow a technical conversation cannot assess a technical control: you end up reading an
implementation statement and taking the engineer's word for it. This repository is the
technical breadth underneath the compliance work, and it is deliberately anchored to the
same system so the connection is visible rather than claimed.

---

## Domain coverage, with the official exam weightings

| Domain | Name | Exam weighting | Module | Anchor in this system |
|---|---|---|---|---|
| 1.0 | General Security Concepts | 12 percent | [SEC-01](sec-01-general-security-concepts/README.md) | Control types, the CIA triad, zero trust, cryptography and PKI, change management |
| 2.0 | Threats, Vulnerabilities, and Mitigations | 22 percent | [SEC-02](sec-02-threats-vulnerabilities-mitigations/README.md) | Threat actors, attack surface, the real vulnerabilities found on this system and how they were mitigated |
| 3.0 | Security Architecture | 18 percent | [SEC-03](sec-03-security-architecture/README.md) | Cloud architecture, network segmentation, data protection, resilience and recovery |
| 4.0 | Security Operations | 28 percent | [SEC-04](sec-04-security-operations/README.md) | Hardening, monitoring, identity and access management, incident response, automation |
| 5.0 | Security Program Management and Oversight | 20 percent | [SEC-05](sec-05-program-management-oversight/README.md) | Governance, risk management, third party risk, compliance, audits, awareness |

### A note on the domain mapping, because it is easy to get wrong

Two of these are commonly misfiled in portfolios and study material:

- **Cryptography and PKI belong to Domain 1**, General Security Concepts, under objective
  1.4. They are frequently filed under Security Architecture because that feels
  architectural. It is not where the exam puts them.
- **Architecture, segmentation and infrastructure design belong to Domain 3**, Security
  Architecture, not Domain 2. Domain 2 is about threats, vulnerabilities and the
  mitigations applied to them, which is a different question from how the system is built.

The mapping in this repository follows the published SY0-701 objectives rather than
intuition, and the weightings above are the official ones: 12, 22, 18, 28 and 20, which
sum to 100.

---

## The simulated system

| Field | Detail |
|---|---|
| System | Keystone Grants Management System (KGMS) |
| Identifier | `FWDA-KGMS-2026-MOD` |
| Operator | Federal Workforce Development Agency (FWDA), fictional |
| Hosting | AWS GovCloud (US-East), provided by Keystone Digital Services LLC, fictional |
| Users | ~8,400 external grant applicants and grantee staff, 420 agency users, 65 contracted peer reviewers, 9 regional offices |
| Data | Grant application content, grantee organisation records including Employer Identification Numbers, individual reviewer PII, award and disbursement records, audit logs |
| Authorisation | FedRAMP Moderate agency ATO, granted 15 May 2026 |

Everything in this repository refers to the same system, the same people and the same
findings as the other three repositories. Where a module discusses a vulnerability or a
control failure, it is one of the 16 findings from the independent assessment,
not a textbook example invented for the occasion.

---

## The portfolio this belongs to

| Repository | What it covers |
|---|---|
| [keystone-kgms-fedramp-rmf](https://github.com/Nkee07/keystone-kgms-fedramp-rmf) | FedRAMP Moderate authorisation: NIST RMF Steps 0 to 6, 16 assessment findings, a 10 item POA&M and the ATO memorandum |
| [keystone-kgms-iso27001](https://github.com/Nkee07/keystone-kgms-iso27001) | ISO/IEC 27001:2022 and ISO/IEC 27005, all 93 Annex A controls, risk register, internal audit and a framework crosswalk |
| [keystone-kgms-conmon-vulnmgmt](https://github.com/Nkee07/keystone-kgms-conmon-vulnmgmt) | Continuous monitoring and vulnerability management in Tenable Nessus, ServiceNow and RSA Archer |
| **This repository** | The technical breadth underneath all three, organised by Security+ domain |

---

## About the author

**Nkeiru Sarah Adesida**
Cybersecurity Governance, Risk and Compliance analyst. Certified Information Systems
Auditor (CISA), **CompTIA Security+ (SY0-701)**, and a Master of Science in Cybersecurity
Management and Policy from University of Maryland Global Campus.

[GitHub](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc) | [nkiru_sarah@yahoo.com](mailto:nkiru_sarah@yahoo.com)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
