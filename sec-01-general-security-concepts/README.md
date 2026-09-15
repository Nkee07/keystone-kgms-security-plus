# SEC-01: Domain 1, General Security Concepts
### Keystone Grants Management System (KGMS) | 12 percent of the exam

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Security+ SY0-701 1.0 General Security Concepts, applied to KGMS  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** CompTIA Security+ SY0-701 Domain 1.0, 12 percent of the exam. Mapped to NIST SP 800-53 Rev 5  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 5 September 2026  
**Status:** Complete

**Navigation:** [Repository home](../README.md) &nbsp;|&nbsp; [SEC-02: Domain 2, Threats, Vulnerabilities and Mitigations ->](../sec-02-threats-vulnerabilities-mitigations/README.md)

---

## Purpose

Domain 1 is the vocabulary the other four domains are written in. It is the smallest domain
by weighting at 12 percent and the one everything else depends on.

Plain language version: before you can argue about how to protect a building, everyone has
to agree what a lock is, what a guard is, what a camera is, and which of those stops a
break in versus merely records one. Domain 1 is that shared vocabulary.

---

## Section 1: The CIA triad, applied rather than recited

Confidentiality, integrity and availability. The reason this matters here is that
KGMS rates them **separately**, and they genuinely diverge.

| Objective | What it means | Rating on this system | Why |
|---|---|---|---|
| Confidentiality | Keeping information from people who should not see it | Moderate | Applicant and reviewer personal information, and unpublished merit scores |
| Integrity | Keeping information correct and unaltered | Moderate, adjusted down from High | Award and disbursement records are integrity critical, but a daily reconciliation and an independent payment service sit between an alteration and the harm |
| Availability | Keeping information reachable when needed | Moderate | Award cycles have statutory deadlines, but a short outage does not destroy the mission |

**The lesson worth carrying:** award amounts are integrity critical and only moderately
confidential, because they become public once announced. Rating the three together as one
sensitivity level, which is the instinct, would have produced the wrong control set.

---

## Section 2: Control categories and control types

Security+ separates **category** (who or what implements the control) from **type** (what
the control does about an event). Mixing them is the single most common confusion in this
domain.

| Category | Meaning | Example on this system |
|---|---|---|
| Technical | Implemented by technology | Multifactor authentication enforced by the identity provider |
| Managerial | Implemented through planning and governance | The security impact analysis required before any significant change |
| Operational | Implemented by people carrying out a process | The weekly correlated review of privileged account activity |
| Physical | Implemented in the physical world | Data centre entry control, inherited from the cloud provider |

| Type | What it does | Example on this system | Timing relative to the event |
|---|---|---|---|
| Preventive | Stops it happening | Allow list input validation on all 31 form fields | Before |
| Deterrent | Discourages the attempt | The login banner stating that activity is monitored | Before |
| Detective | Notices it happened | SIEM detection rules across seven log groups | During or after |
| Corrective | Fixes it after the fact | Restoring a database from backup | After |
| Compensating | Does a different job when the intended control is not possible | Restricting the management security group while multifactor authentication for reviewers was still being procured | While the real control is absent |
| Directive | Tells people what to do | The acceptable use policy and the separation checklist | Before |

### The compensating control that actually mattered

When the assessment found single factor authentication for 65 external reviewers, that
control could not be fixed immediately: it needed procurement of identity provider licences.
So the exposure was limited by compensating controls, reviewer accounts expiring
automatically at panel end plus 14 days and the population capped at 65, while the real fix
was procured.

**A compensating control is not a substitute for the control.** It buys time, and it has to
be written down and time bounded or it quietly becomes the permanent answer. The Authorizing
Official attached a hard date of 31 July 2026 for exactly that reason.

---

## Section 3: AAA, and why the third A is the one people forget

| Element | Question it answers | On this system |
|---|---|---|
| Authentication | Are you who you say you are? | PIV card for 420 agency users; authenticator application multifactor for 65 external reviewers since 24 July 2026 |
| Authorisation | What are you allowed to do? | Nine application roles, enforced server side, least privilege, reviewed quarterly |
| Accounting | What did you actually do? | Seven log groups, one year online and two years archived, with privileged actions logged with a justification string |

Accounting is the one that gets under resourced, and it is the one that matters most after
an incident. On this system the accounting leg had two separate failures: one log group
retained records for only 90 days against a one year requirement, and the weekly review that
would have used those records produced evidence for only half the weeks sampled. Both were
findings.

---

## Section 4: Zero trust, described honestly

Zero trust means no implicit trust from network position. Being inside the network is not
authorisation.

| Zero trust principle | Implemented on this system? | Detail |
|---|---|---|
| Verify explicitly, every request | Partially | Every request is authenticated and authorised server side. Continuous risk based reauthentication is not implemented |
| Least privilege access | Yes | Nine roles, quarterly attestation, privileged actions from a role assumed per task |
| Assume breach, segment accordingly | Yes | Three tier subnet model. The data subnet has no route to the internet, so a compromised application instance cannot exfiltrate directly |
| No standing administrative access | Not yet | Standing membership in three privileged groups. Just in time elevation is a planned treatment for risk R-04 |
| Encrypt everything in transit | Yes | TLS 1.2 minimum externally, mutual TLS on the payment interconnection |
| Log and inspect all traffic | Partially | Seven log groups. Full internal traffic inspection is not implemented |

**The honest summary:** this system applies zero trust principles, it is not a zero trust
architecture. Three of six are partial or absent. Claiming the label because some principles
are present is exactly how the term became meaningless, and an interviewer who works in this
area will ask which principles specifically.

---

## Section 5: Cryptography and PKI, objective 1.4

This is filed in Domain 1 rather than Domain 3 because that is where the exam puts it.

| Use | Algorithm or protocol | Where on this system | Why this choice |
|---|---|---|---|
| Data at rest | AES-256 in GCM mode | Database storage, object storage, snapshots | Authenticated encryption: it provides confidentiality and integrity together, so ciphertext tampering is detected rather than silently decrypted |
| Data in transit, external | TLS 1.2 minimum | All external paths | TLS 1.0 and 1.1 are deprecated. A finding for legacy TLS on an internal console is open |
| Data in transit, payment path | Mutual TLS | Payment service interconnection | Both ends present certificates, so the receiving service verifies the sender rather than accepting any client with the right URL |
| Key exchange | ECDHE | TLS handshakes | Ephemeral, so it provides forward secrecy: a later compromise of the server private key does not decrypt captured past sessions |
| Integrity of records | SHA-256 | Audit record hashing, artifact checksums | SHA-1 is deprecated for security use after demonstrated collisions |
| Message authentication | HMAC-SHA256 | API request signing | A plain hash proves data was not changed. HMAC also proves who produced it, because it requires the key |
| Password storage | Not applicable | No passwords are stored | Authentication is federated to the identity provider. The best way to protect a password store is not to have one |

### Public key infrastructure on this system

| Element | Detail |
|---|---|
| Public certificates | Issued by a public certificate authority for external facing endpoints, renewed automatically 30 days before expiry |
| Internal certificates | Issued by the agency private certificate authority for internal service to service authentication |
| Client certificates | Held by the payment interconnection at both ends for mutual TLS |
| Key management | Customer managed keys in the platform key management service. The agency controls key policy, rotation and who may decrypt, and every key use is logged |
| Revocation | Certificate revocation checking through OCSP stapling, so the client does not have to reach the certificate authority itself |

### The key management finding, because it is a good illustration

Snapshots replicated to the secondary region were encrypted with the platform default key
rather than the agency customer managed key. **The data was still encrypted.** What was lost
was key governance: the agency could not rotate that key, could not restrict who may decrypt
with it, and received no key usage logs for that copy.

That distinction, encryption present but key control absent, is exactly what a certificate
and key management question is testing. It was rated Low rather than Moderate precisely
because no plaintext was exposed.

---

## Section 6: Change management, objective 1.3

Change management is in Domain 1 because uncontrolled change undoes every other control.

| Element | How this system does it |
|---|---|
| Approval process | Every change requires review. Changes meeting the significant change threshold also require a security impact analysis before deployment |
| Impact analysis | Assesses whether the change affects the boundary, the interconnections, the authentication model, the information types or the compensating controls |
| Backout plan | Instances are replaced rather than edited, so the backout is redeploying the previous image |
| Maintenance window | Standard changes in a defined window. Emergency changes permitted with retrospective review |
| Version control | Infrastructure defined as code, peer reviewed before merge |
| Documentation | The System Security Plan is updated when the change alters what it describes. It is at version 1.3 |

### The change nobody would think to control

The system's integrity impact rating was reduced from High to Moderate because a daily
reconciliation exists in the finance function. **That control is operated by people outside
security who could reasonably change its cadence for efficiency reasons.**

So the change control gate explicitly covers it: any change to that reconciliation, including
its frequency, goes to the Authorizing Official before implementation, not after. Without
that gate, a sensible efficiency decision in a different department would silently invalidate
the system's security categorisation.

---

**Navigation:** [Repository home](../README.md) &nbsp;|&nbsp; [SEC-02: Domain 2, Threats, Vulnerabilities and Mitigations ->](../sec-02-threats-vulnerabilities-mitigations/README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
