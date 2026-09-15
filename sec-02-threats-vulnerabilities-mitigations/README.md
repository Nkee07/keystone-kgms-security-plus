# SEC-02: Domain 2, Threats, Vulnerabilities and Mitigations
### Keystone Grants Management System (KGMS) | 22 percent of the exam

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes, with one exception: the author, Nkeiru Sarah Adesida, is a real person. Where her name appears in a scenario role, that role is fictional, and she has never worked for this agency, which does not exist. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Security+ SY0-701 2.0 Threats, Vulnerabilities, and Mitigations, applied to KGMS  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** CompTIA Security+ SY0-701 Domain 2.0, 22 percent of the exam. Mapped to NIST SP 800-53 Rev 5  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 5 September 2026  
**Status:** Complete

**Navigation:** [<- SEC-01: Domain 1, General Security Concepts](../sec-01-general-security-concepts/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [SEC-03: Domain 3, Security Architecture ->](../sec-03-security-architecture/README.md)

---

## Purpose

Domain 2 is the largest single technical domain at 22 percent. It asks three questions: who
would attack this, how would they get in, and what stops them.

Plain language version: who wants to break into the building, which doors and windows are
weak, and what you actually fit to each one.

The advantage of doing this against a real system rather than a textbook is that the
vulnerabilities below are **the actual findings from the independent assessment of
KGMS**, not invented examples.

---

## Section 1: Threat actors

| Actor | Motivation | Capability | Realistic against this system? | What they would target |
|---|---|---|---|---|
| Unskilled attacker | Opportunity, reputation | Low, uses published tooling | **Yes, constantly.** Automated scanning of internet facing endpoints is continuous background noise | Known unpatched vulnerabilities on the public facing tier |
| Organised crime | Financial gain | Moderate to high | **Yes.** This system generates payment instructions | The disbursement path, or applicant personal information for resale |
| Nation state | Espionage, disruption | Very high, including supply chain | **Plausible but not the primary threat.** Grant awards are not a high intelligence target | The supply chain, or persistence for later use |
| Insider, malicious | Grievance, financial gain | Moderate, but starts with legitimate access | **Yes, and the highest impact.** 16 privileged accounts can reach everything | Award records, or the audit trail that would evidence the act |
| Insider, unintentional | None. Error | Not applicable | **Yes, and the most likely of all.** Misconfiguration is the most common cloud incident cause | Anything. A public storage bucket, a permissive security group |
| Hacktivist | Ideological | Low to moderate | **Possible.** Grant award decisions are politically visible | Defacement, or leaking merit review deliberations |
| Competitor | Commercial advantage | Low | **Yes, in an unusual form.** Grant applicants compete with each other | Rival applications and unpublished merit scores before award |

### The one people miss

The last row. On most systems "competitor" is irrelevant. Here, applicants are competing for
finite awards, and an applicant who reads a rival's submission or their unpublished score
gains directly. That reframes the reviewer portal from a convenience feature into the most
commercially attractive target on the system, which is why single factor authentication there
was rated High rather than Moderate.

---

## Section 2: Attack surface

| Surface | Exposure | Primary threat | Control |
|---|---|---|---|
| Public application endpoints | Internet, unauthenticated for the application form | Injection, automated exploitation | Web application firewall, allow list input validation, monthly authenticated scanning |
| Reviewer portal | Internet, 65 external users | Credential theft, phishing | Multifactor authentication since 24 July 2026, session timeout, accounts expiring at panel end plus 14 days |
| Agency user access | Agency network, 420 users | Phishing, session theft | PIV card authentication, which defeats credential replay |
| Administrative access | 16 privileged accounts | Insider misuse, credential compromise | Brokered session manager with no inbound port, session recording, documented approval, quarterly review |
| Third party dependencies | Build pipeline | Supply chain compromise | Signed base images, checksum verification, dependency scanning. This is the weakest surface, see Section 4 |
| Interconnections | 3 external systems | Compromise of a trusted partner | Mutual TLS, agreements, validation of everything received |
| Cloud control plane | Administrative API | Misconfiguration, credential compromise | Infrastructure as code with peer review, continuous compliance evaluation, public access blocked at account level |

---

## Section 3: The vulnerabilities actually found

These are the 3 High and a selection of the Moderate findings from the
independent assessment, classified by vulnerability type.

| Finding | Vulnerability type | What it was | Severity |
|---|---|---|---|
| SAR-002 | Injection, stored cross site scripting | The rich text project narrative field stored a script payload that executed in a reviewer's browser | High |
| SAR-001 | Weak authentication | Single factor authentication for 65 external reviewers with access to personal information and unpublished scores | High |
| SAR-003 | Improper privilege management | Three application administrators held standing privileged access with no approval record, one belonging to a departed contractor | High |
| SAR-004 | Unpatched software | CVE-2024-6387 in OpenSSH on two bastion hosts, a signal handler race that can lead to remote code execution as root | Moderate |
| SAR-005 | Misconfiguration | Four of eleven instances drifted from the hardened baseline, including one missing audit rules for privileged commands | Moderate |
| SAR-008 | Supply chain | Supply chain risk management procedure drafted but never approved, and the development team had never seen it | Moderate |
| SAR-011 | Cryptographic weakness | Secondary region snapshots encrypted with a default key rather than an agency controlled key | Low |
| VUL-2026-08-01 | Unpatched dependency | CVE-2021-44228, Log4Shell, in a transitive dependency of the reporting service | Critical, base score 10.0 |

### Worked example: the stored cross site scripting, SAR-002

**Why it was rated High and reflected XSS would not have been.** A reflected attack needs the
victim to click a crafted link. **A stored attack sits in the application waiting**, and here
the victims were reviewers holding unpublished scores and applicant personal information.
The attacker would be an applicant, which means anyone on the internet who can start an
application.

**Root cause.** Validation used a deny list of dangerous tags. **Deny lists on HTML always
fail**, because the set of ways to express a script in a browser is larger than any list of
them.

**Mitigation, two layers.** Input is parsed against an explicit allow list of permitted
elements and attributes, rejecting everything else. Output is encoded for the context it
renders into, so a payload that somehow reached storage would render as visible text.
Either layer alone would have satisfied the finding; both were implemented because either
alone is a single point of failure.

Retested with the original payload and four variants. All rendered as inert text.

### Worked example: Log4Shell found in 2026

Finding a five year old vulnerability with a base score of 10.0 in 2026 is not a good look,
and the honest cause is specific: the reporting service uses a charting library, that library
bundles its own logging dependency, and **the dependency scanner was configured to examine
direct dependencies only.** The vulnerable library was never in a list the scanner read.

Every scan came back clean, and every scan was correct about what it examined.

The mitigation that matters is not the library upgrade. It is resolving the full dependency
tree and producing a software bill of materials on every build, so the next time a widely
exploited dependency is published the question "are we affected" is a query rather than an
investigation.

---

## Section 4: Indicators of compromise, and one worked false positive

| Indicator | What it might mean | Monitored here? |
|---|---|---|
| Authentication from an unusual geography | Credential compromise | Yes, identity provider risk signals |
| Privileged access outside working hours | Insider misuse or compromised administrator | Yes, weekly correlated review |
| Bulk record retrieval | Data exfiltration | Yes, alert rule |
| Outbound connection to an unknown host | Command and control, or exfiltration | Yes, and the data subnet has no internet route at all |
| Configuration change outside the change window | Unauthorised change or compromise | Yes, continuous compliance evaluation |
| Failed logins followed by a success | Password spraying | Yes, with lockout after 5 attempts |
| New account creation outside the provisioning workflow | Persistence | Yes, identity provider audit log |

### The July 2026 alert that was not an incident

On 17 July an alert fired for anomalous bulk record export. Investigation traced it to a
scheduled quarterly reporting job that had been rescheduled, so it ran outside its usual
window.

**Closed as a false positive, and the detection rule was tuned to reference the job schedule
rather than a fixed time window.** The reason this is worth recording rather than quietly
closing: a rule that fires on legitimate activity gets ignored, and a rule that gets ignored
is not a control. Tuning is not weakening detection, it is the difference between a rule
somebody acts on and one they learn to dismiss.

---

## Section 5: Mitigation techniques

| Technique | Applied here as | Effect |
|---|---|---|
| Segmentation | Three tier subnet model, data subnet with no internet route | A compromised application instance cannot exfiltrate directly |
| Least privilege | Nine roles, quarterly attestation, per task role assumption | Limits what a compromised account reaches |
| Patching | Monthly cycle plus emergency path, 30 day service level for Critical and High | Reduces the exposure window |
| Hardening | Baseline images, no password SSH, no unnecessary services, host firewall default deny | Reduces the attack surface before anything is deployed |
| Input validation | Allow list on all 31 fields, output encoding as a second layer | Closes the shortest path to the data |
| Monitoring | Seven log groups, detection rules, daily triage | Detects what prevention missed |
| Encryption | At rest and in transit, agency controlled keys | Limits the value of stolen data or intercepted traffic |
| Decommissioning | Retiring the legacy grantee lookup service rather than patching it | The only mitigation that removes a surface completely |

### The one nobody counts as a mitigation

**Decommissioning.** VUL-2026-08-08 sits on a retired grantee lookup service scheduled for
removal on 15 October 2026. Every other technique in the table reduces risk. Turning the
service off eliminates it, and it is almost always cheaper than the alternative of
maintaining a system nobody uses. The reason it is rarely chosen is that nobody owns the
decision to switch something off.

---

**Navigation:** [<- SEC-01: Domain 1, General Security Concepts](../sec-01-general-security-concepts/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [SEC-03: Domain 3, Security Architecture ->](../sec-03-security-architecture/README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
