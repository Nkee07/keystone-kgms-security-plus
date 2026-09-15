# SEC-05: Domain 5, Security Program Management and Oversight
### Keystone Grants Management System (KGMS) | 20 percent of the exam

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Security+ SY0-701 5.0 Security Program Management and Oversight, applied to KGMS  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** CompTIA Security+ SY0-701 Domain 5.0, 20 percent of the exam. Mapped to NIST SP 800-53 Rev 5  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 5 September 2026  
**Status:** Complete

**Navigation:** [<- SEC-04: Domain 4, Security Operations](../sec-04-security-operations/README.md) &nbsp;|&nbsp; [Repository home](../README.md)

---

## Purpose

Domain 5 is 20 percent of the exam and it is the bridge between Security+ and the Governance,
Risk and Compliance work in the rest of this portfolio. It asks who decides, who is
accountable, and how the organisation proves any of it.

Plain language version: the previous four domains are about doing security. This one is about
being able to show somebody that you did it, and about who carries the consequences when it
goes wrong.

---

## Section 1: Governance

| Element | On this system |
|---|---|
| Policy | Information security policy committing the agency to protect grant information, comply with statute, manage risk through a documented process, and improve continually. Reviewed annually and on significant change |
| Standards | Agency cryptographic standard, hardening benchmarks, authenticator standard |
| Procedures | Provisioning, deprovisioning, access review, incident response, change management, supply chain |
| Guidelines | Advisory, not mandatory. Secure development guidance |

| Governance role | Name in the scenario | Accountability |
|---|---|---|
| Authorizing Official | Patricia L. Ambrose | Accepts residual risk and signs the authorisation. A mission executive |
| Chief Information Security Officer | Daniel K. Osei | Advises the AO. **Does not sign the authorisation** |
| System Owner | Marcus T. Delacroix | Owns the mission function and the budget |
| Information System Security Officer | Nkeiru Sarah Adesida | Day to day posture, evidence, POA&M upkeep |
| Senior Agency Official for Privacy | Helen M. Barragan | Privacy threshold analysis and impact assessment |

### The separation that the whole framework rests on

**The Authorizing Official and the CISO are two different people.** The CISO recommends; the
AO decides and personally accepts the risk.

Collapsing them, which is a common shortcut in practice scenarios, means the person who
assessed the risk is also the person who accepted it, and nobody independent ever challenged
the decision. It is a segregation of duties failure, and it removes the single control the
framework is built around: **a named individual, in writing, with a date, accountable for
the residual risk.**

---

## Section 2: Risk management

| Step | Method here |
|---|---|
| Identification | Asset and threat based. Assets from inventory; threats from the agency threat profile, assessment findings and scan history |
| Analysis | Likelihood 1 to 5 multiplied by impact 1 to 5. Scored twice: inherent, before current controls, and residual, after them |
| Evaluation | Residual in the Low band is within tolerance. Moderate requires treatment or a documented retention decision |
| Treatment | Modify, Retain, Avoid or Share, per ISO/IEC 27005 |
| Monitoring | Quarterly review and on significant change |

### Risk response terms, used correctly

| Response | What it means | Example on this system |
|---|---|---|
| Mitigate, or Modify | Add or strengthen controls | Enforcing multifactor authentication for 65 reviewers |
| Accept, or Retain | Live with it, documented and decided | SAR-015, the paper visitor log at a regional office holding no system components. Accepted permanently by the AO |
| Avoid | Stop doing the activity | Decommissioning the retired grantee lookup service rather than maintaining it |
| Transfer, or Share | Move part of it to another party | Physical and platform risk shared with the cloud provider under its own certification |

**Acceptance is a decision, not neglect.** SAR-015 was accepted because that location houses
no system component, so an electronic badge system there would spend money without reducing
risk. It was signed, dated, and it is reviewed at each annual assessment. An unaccepted,
unrecorded, unowned open finding is neglect. The paperwork is the entire difference.

### Why every risk is scored twice

An inherent score alone describes a world with no controls, which is not a decision. A
residual score alone tells you where you are but not which control is carrying the weight.
Both together show where control investment is earning: cloud misconfiguration drops from 15
to 6, the strongest control effect in the register, while supply chain risk drops from 12 to
12, that is, not at all. **That is why supply chain is first in the treatment plan**, and
you cannot see it with one score.

---

## Section 3: Third party and supply chain risk

| Relationship | Risk | Control |
|---|---|---|
| Cloud service provider | Platform compromise, or a control assumed inherited that is actually shared | Provider authorisation package reviewed annually; written responsibility split for every shared control |
| Payment service interconnection | Compromise of a trusted partner | Mutual TLS, executed agreement, validation of everything received |
| Container base images | Upstream compromise | Signed images, checksum verification in the pipeline |
| Application dependencies | Malicious or vulnerable upstream code | Dependency scanning. **This failed**, see below |
| Third party merit scoring library | A component processing in-scope data | Formal vetting, or removal if vetting is not satisfactory |
| Contracted peer reviewers | External people with access to confidential data | Screening, non disclosure agreements, multifactor authentication, accounts expiring at panel end |

### The supply chain finding, and the honest account of it

The supply chain risk management procedure existed **as an unapproved draft**, and the
development team it applied to had never seen it.

Root cause: after the draft was produced, nobody was assigned to obtain approval, so it
stalled at 90 percent complete. That is not exotic; it is how most controls actually fail.

The practical consequence arrived five months later as CVE-2021-44228 in a transitive
dependency, because the governing process that would have required a software bill of
materials did not exist in an approved form.

**A document nobody follows is not a control.** The assessor found this by interviewing the
development team as well as reading the procedure, which is the only way that gap surfaces.

### A note on the control family, because it is commonly wrong

Supply chain risk management is the **SR family** in NIST SP 800-53 Revision 5, SR-1 through
SR-12. **SA-12, Supply Chain Protection, was withdrawn in Revision 5** and its content
redistributed into SR. A package that cites Rev 5 and then raises a supply chain finding
against SA-12 is citing a withdrawn control, which tells a reviewer the current catalogue was
never opened.

---

## Section 4: Compliance and audits

| Requirement | Applies because | Evidence |
|---|---|---|
| FISMA | It is a federal information system | The full authorisation package |
| FedRAMP | It is a cloud service used by a federal agency | Moderate baseline, agency ATO |
| Privacy Act of 1974 | It maintains records on individuals | System of records notice, privacy controls |
| E-Government Act section 208 | It collects information from the public | Privacy impact assessment |
| OMB Circular A-130 | Federal information management | The programme as a whole |
| OMB Circular A-123 | Internal control over award and disbursement records | Segregation of duties, daily reconciliation |

| Assessment type | Who performs it | Independence | Frequency |
|---|---|---|---|
| Independent security assessment | A contracted assessor reporting to the CISO | Reports to the CISO, not the System Owner. Had no role in building the system | Annual, one third of controls plus every previously failed control |
| Internal audit | Agency internal audit function | Reports to the Audit Committee, not to the System Owner or the ISSO | Annual |
| Continuous monitoring | The ISSO | Self assessment, which is why it is reported to the AO monthly rather than relied on alone | Monthly |

### Why the reporting line matters more than the title

An assessor who reports to the System Owner is under pressure to find less, because the
System Owner's project is what the findings delay. Recording the reporting line in the
assessment plan is the only way a reader can judge whether a clean result means anything.

**Previously failed controls are always back in scope**, every cycle, not once a year in
their turn. A closed finding is the most likely finding to recur, because the conditions that
produced it usually still exist.

---

## Section 5: Security awareness

| Element | Implementation | Result at 31 August 2026 |
|---|---|---|
| Annual training | Required for all agency users, tracked monthly rather than annually | 418 of 420 current, the remaining 2 inside grace |
| Role specific briefing | Additional content for privileged users |  |
| Phishing simulation | Periodic, with follow up training rather than punishment |  |
| Reporting culture | A reporting path to the security operations centre, and no penalty for reporting a mistake |  |
| External users | Reviewers receive a briefing and sign a non disclosure agreement |  |

### The two details that make an awareness programme work

**Tracked monthly, not annually.** Tracking annually means you discover lapses annually. The
assessment found 11 users past their due date; with monthly tracking and an automated 30 day
reminder, that population is now 2 and both are inside grace.

**No penalty for reporting.** The organisational goal is that somebody who clicks a phishing
link reports it within minutes. A programme that punishes the click produces silence, and
silence is the single worst outcome available, because the incident still happened and now
nobody knows.

---

## Section 6: How Domain 5 connects to the rest of this portfolio

Domain 5 is Security+ describing, at survey level, the work the other three repositories do
in full.

| Domain 5 topic | Where it is done properly |
|---|---|
| Governance and roles | [ISMS scope and leadership](https://github.com/Nkee07/keystone-kgms-iso27001/blob/main/iso-01-isms-scope/README.md), ISO/IEC 27001 Clauses 4 and 5 |
| Risk management | [Risk register](https://github.com/Nkee07/keystone-kgms-iso27001/blob/main/iso-03-risk-register/README.md) and [treatment plan](https://github.com/Nkee07/keystone-kgms-iso27001/blob/main/iso-04-risk-treatment/README.md) |
| Third party risk | Statement of Applicability controls 5.19 to 5.22, and risk R-06 |
| Compliance and audit | [Assessment plan and report](https://github.com/Nkee07/keystone-kgms-fedramp-rmf/blob/main/step-04-assessment/README.md) and the [internal audit](https://github.com/Nkee07/keystone-kgms-iso27001/blob/main/iso-05-audit-management-review/README.md) |
| Risk acceptance | [The authorisation decision memorandum](https://github.com/Nkee07/keystone-kgms-fedramp-rmf/blob/main/step-05-authorization/README.md) |
| Monitoring and reporting | [The continuous monitoring repository](https://github.com/Nkee07/keystone-kgms-conmon-vulnmgmt) |

**That is the argument for this repository existing.** Security+ gives the vocabulary and the
breadth. The GRC repositories are what the work looks like when it is done to a professional
standard against a real framework, with evidence, on a system somebody has to sign for.

---

**Navigation:** [<- SEC-04: Domain 4, Security Operations](../sec-04-security-operations/README.md) &nbsp;|&nbsp; [Repository home](../README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
