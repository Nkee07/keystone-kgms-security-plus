# SEC-04: Domain 4, Security Operations
### Keystone Grants Management System (KGMS) | 28 percent of the exam, the largest domain

> ### PORTFOLIO EXERCISE, SIMULATED SYSTEM
> **Keystone Grants Management System (KGMS) does not exist.** The Federal Workforce Development Agency (FWDA) is a fictional federal
> agency invented for this portfolio. Every organisation, person, system
> identifier, network address, finding, date and signature on this page is
> fabricated for training purposes, with one exception: the author, Nkeiru Sarah Adesida, is a real person. Where her name appears in a scenario role, that role is fictional, and she has never worked for this agency, which does not exist. Nothing here is drawn from any real
> employer, client or government system, and no real document, artifact or
> data has been reproduced. This file is coursework, not a government record.

---

**Document:** Security+ SY0-701 4.0 Security Operations, applied to KGMS  
**Simulated system:** Keystone Grants Management System (KGMS) | `FWDA-KGMS-2026-MOD`  
**Framework and standards:** CompTIA Security+ SY0-701 Domain 4.0, 28 percent of the exam. Mapped to NIST SP 800-53 Rev 5  
**Author:** Nkeiru Sarah Adesida  
**Document date:** 5 September 2026  
**Status:** Complete

**Navigation:** [<- SEC-03: Domain 3, Security Architecture](../sec-03-security-architecture/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [SEC-05: Domain 5, Security Program Management and Oversight ->](../sec-05-program-management-oversight/README.md)

---

## Purpose

Domain 4 is the largest domain at 28 percent, and that weighting is correct: security
operations is where most security work actually happens, every day, after the architecture
is built and the policies are signed.

Plain language version: the building is designed and the rules are written. Domain 4 is the
shift that keeps it running: checking the doors, watching the cameras, handing out and taking
back keys, and knowing what to do when the alarm goes off.

---

## Section 1: Hardening and secure baselines

| Measure | Implementation on this system |
|---|---|
| Baseline images | Hardened image per tier, derived from agency benchmarks |
| Disable unnecessary services | No unused listening services in the baseline |
| Disable password SSH | Key based only, and in practice no inbound SSH at all |
| Host firewall | Default deny inbound |
| Audit rules | Privileged command execution logged at the host level |
| Immutable infrastructure | Instances are replaced, not edited. A change means a new image and a rebuild |
| Drift detection | Continuous compliance evaluation against the baseline, active since July 2026 |

### The drift finding, and what it really cost

Four of eleven instances had drifted from the baseline. Two permitted password SSH, one
exposed an unused administration interface, and **one was missing the audit rules for
privileged command execution.**

The last one is the interesting failure. That host was not recording privileged actions at
all, so a configuration weakness in the CM family had quietly degraded assurance in the AU
family. **Control families are not independent**, and a finding in one can silently remove
evidence you would need for another.

Root cause: two instances were manually patched during an incident in November 2025 and never
rebuilt, and two were built from an image version predating a baseline update. **Nothing
detected any of it**, because the only thing that ever compared a running host to its baseline
was an assessment, once a year.

Fix: rebuild all four, and add continuous compliance evaluation so drift raises a finding
within hours rather than within a year.

---

## Section 2: Asset and inventory management

| Stage | Control | Where it failed |
|---|---|---|
| Acquisition | Components provisioned through infrastructure as code with peer review |  |
| Inventory | Authoritative component inventory | **Twice.** Three container images omitted in March 2026, two database instances omitted in July 2026 |
| Assignment | Every component has a named owner |  |
| Monitoring | Scan coverage derived from inventory | Coverage fell to 96 percent because the scan target group was maintained by hand |
| Disposal | Decommissioning with media sanitisation inherited from the provider | A retired lookup service is still running, scheduled for removal 15 October 2026 |

### Why inventory is the load bearing control

**Scan coverage can never exceed inventory accuracy.** If a component is not in the
inventory, nothing scans it, and a clean vulnerability report simply means the scanner did
not know the thing existed.

Two separate findings on this system, container images missing and database instances
missing, are the same failure wearing different clothes: **a human step between provisioning
something and registering it.** The correction is not a better checklist. It is binding the
scan target group to the inventory and having the build pipeline register components
automatically, so the human step is removed rather than reinforced.

---

## Section 3: Vulnerability management

| Scan type | Cadence | Credentialed | Scope |
|---|---|---|---|
| Infrastructure | Weekly | Yes | 11 compute plus 4 managed database instances |
| Web application | Monthly | Yes, as a test user in each of 9 roles | 31 input fields, upload path, all authenticated routes |
| Container image | Every build | Static analysis | All 3 images, build blocks on Critical |
| Cloud configuration | Continuous | Read only assessment role | Every resource against the baseline |

| Severity | CVSS base | Remediate within |
|---|---|---|
| Critical | 9.0 to 10.0 | 30 days |
| High | 7.0 to 8.9 | 30 days |
| Moderate | 4.0 to 6.9 | 90 days |
| Low | 0.1 to 3.9 | 180 days |

### Credentialed versus uncredentialed, the finding that proves it

An unauthenticated scan sees a host from outside: listening services and version banners. An
authenticated scan logs in and enumerates installed package versions and configuration.

The database tier had been scanned unauthenticated since it was built, and every report was
clean. **The reports were clean because the scanner could not see anything.** The first
authenticated scan returned 14 findings including 2 High. Nothing had changed on the hosts;
the only change was that the scanner could now look.

**An uncredentialed scan reporting zero findings is not a clean result.** It is an absence of
information presented as a clean result, which is worse than no scan, because somebody will
believe it.

### Base score versus environmental score

CVSS produces several scores and they are not interchangeable. The **base score** is a
property of the vulnerability, set by whoever published it, the same number for everyone, and
**it cannot be adjusted by your environment.** The **environmental score** adjusts for your
situation.

Writing "CVSS 10.0, downgraded to High in our environment" in a score field is wrong, and it
appears constantly in real reports. Nothing was downgraded. A separate environmental score was
computed and it is lower.

On this system, CVE-2021-44228 carries a base score of 10.0 and an environmental score of 6.1,
because the reporting service is not internet reachable and does not log untrusted input.
**Both numbers are reported, and the remediation deadline follows the base score**, because
the two facts that lower the environmental score are properties of the current architecture,
and architecture changes.

---

## Section 4: Monitoring, alerting and the evidence problem

| Layer | Source | Cadence | Output |
|---|---|---|---|
| Log collection | 7 log groups | Continuous | 1 year online, 2 years archived |
| Detection | SIEM rules | Continuous | Alerts |
| Triage | Security operations centre | Daily | Ticket record |
| Correlated review | Privileged activity across application, database and control plane | Weekly | Review record |
| Configuration compliance | Platform compliance rules | Continuous | Drift exceptions |
| Reporting | Monthly report to the Authorizing Official | Monthly | Report |

### The most instructive finding in the whole portfolio

The daily alert triage was well evidenced. The weekly correlated review of privileged activity
produced evidence for **6 of 12 sampled weeks.**

Interviews indicated the review was probably performed in most of the missing weeks. The
analyst worked from a live dashboard and escalated anything interesting. **When nothing was
interesting, nothing was written down**, so a week with no findings looked identical to a week
with no review.

**For an assessor, an unevidenced control is an unperformed control.** There is no credit for
work that left no record. This is the most common shape of finding in federal assessment work
and it is almost never somebody failing to do their job.

The correct fix is not a reminder. It is **automating the artifact** so it exists whether or
not anything interesting is in it: queries run, period covered, reviewer, volume examined, and
an explicit "no anomalies identified" where that is the outcome.

The same root cause explains a second finding, missing separation checklists: a human
performed step with no system forcing an artifact. **Two findings, one cause, one fix.**
Nobody sees that by writing up findings in isolation.

---

## Section 5: Identity and access management

| Population | Count | Authentication | Lifecycle |
|---|---|---|---|
| Agency users | 420 | PIV card through the agency identity provider | Approved request, automated provisioning, disabled within one business day of separation |
| Contracted peer reviewers | 65 | Authenticator application multifactor since 24 July 2026 | Created per panel, expiry set at creation to panel end plus 14 days |
| Service accounts | 9 | Credentials in the platform secrets service, rotated every 90 days | Non interactive, named human owner |
| Privileged accounts | 16 across 3 groups | As above, plus a role assumed per task with a logged justification | Documented approval, quarterly full population review |

| Practice | Implementation |
|---|---|
| Least privilege | 9 application roles, enforced server side |
| Separation of duties | Award approval and disbursement are different roles |
| Access review | Quarterly, line by line supervisor attestation, retained in RSA Archer |
| Inactivity | Accounts with no login for 35 days automatically disabled |
| Deprovisioning | Disable, not delete. The account object is kept 180 days so audit records stay attributable |
| Privileged access | Assumed per task, not standing in daily use. Just in time elevation is planned |

### Two IAM design decisions worth defending in an interview

**Reviewer accounts expire at creation.** The expiry is set when the account is made, to panel
end plus 14 days, rather than tracked separately for later cleanup. A reviewer account that
outlives its panel is the most likely stale account on this system, and the cheapest moment to
handle it is the moment of creation.

**Deprovisioning disables rather than deletes.** Deleting the account object breaks
attribution: audit records then point at an identifier that no longer resolves to a person.
180 days of retention keeps the audit trail readable.

**And the failure worth admitting:** group membership inside the application was managed
separately from account status in the identity provider, so disabling a departed contractor's
account did not remove them from the application administrator group. They would have regained
privilege if the account were ever re-enabled. Two systems holding the same fact will always
drift. The fix was to make the identity provider authoritative for both.

---

## Section 6: Incident response

| Phase | On this system |
|---|---|
| Preparation | Documented plan, defined roles, annual exercise, reporting path to the security operations centre |
| Detection and analysis | Detection rules on 7 log groups, daily triage, one hour response for High severity alerts |
| Containment | Instances are replaceable, so containment is isolate and rebuild rather than clean in place |
| Eradication | Rebuild from a known good image. Root cause identified before closure |
| Recovery | Restore from backup where data is affected. Monitored return to service |
| Lessons learned | Root cause recorded, and the control change that prevents recurrence |

### Forensics and the control that would have failed

Evidence handling depends on audit records being complete and attributable. On this system
that had two weaknesses: one log group retained records for only 90 days against a one year
requirement, and one drifted host was not recording privileged commands at all.

**Both were corrected before they were needed**, which is the only acceptable time to find
out. The forensic capability of a system is not tested until an incident, and an incident is
the worst possible moment to discover the logs stop 90 days ago.

---

## Section 7: Automation

| Automated | Replaces | Why it is better than the manual version |
|---|---|---|
| Account provisioning from an approved request | Manual account creation | Removes the gap between approval and creation, and produces its own record |
| Deprovisioning on separation notification | Manual account disabling | One business day, consistently, with an audit log entry |
| Configuration compliance evaluation | Periodic manual inspection | Detects drift in hours rather than at the annual assessment |
| Scan target group bound to inventory | Manually maintained scan list | Removes the step that failed twice |
| Weekly review report generation | An analyst remembering to write it up | Produces the artifact whether or not anything is found |
| Dependency and image scanning in the pipeline | Post deployment scanning | Blocks a vulnerable image before it reaches a registry |

**The pattern across every row:** automation here is not about speed. Each one removes a human
step that had already failed at least once, and each one produces an artifact as a side effect
rather than as an extra task somebody has to remember.

---

**Navigation:** [<- SEC-03: Domain 3, Security Architecture](../sec-03-security-architecture/README.md) &nbsp;|&nbsp; [Repository home](../README.md) &nbsp;|&nbsp; [SEC-05: Domain 5, Security Program Management and Oversight ->](../sec-05-program-management-oversight/README.md)

---

*Author: Nkeiru Sarah Adesida, Information System Security Officer (ISSO), portfolio scenario*  
*Simulated system: Keystone Grants Management System (KGMS) | FWDA-KGMS-2026-MOD*  
*This document is a portfolio exercise built on a fictional organisation. It is not a government record.*  
*[GitHub portfolio](https://github.com/Nkee07) | [LinkedIn](https://www.linkedin.com/in/nkeiru-adesida-grc)*
