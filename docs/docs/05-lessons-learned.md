# Lessons Learned

Honest retrospective on what worked, what did not work, and what 
I would do differently if rebuilding this lab from scratch. 
Written as a working document for future iterations, not as 
marketing material.

## What worked

### Identity-focused scope from day one

Limiting the lab to Microsoft Entra ID Audit Logs forced depth 
over breadth. With one well-understood data source, every 
detection rule could be validated end-to-end against real events 
rather than synthetic data. Three identity-based detections 
(account creation, role assignment, service principal creation) 
cover the most common cloud attacker techniques. Adding more data 
sources before mastering this one would have diluted the quality 
of every rule.

### Controlled event generation before writing rules

Generating real events first (Maria Lopez, Carlos Fernandez) and 
inspecting the raw logs in Log Analytics before writing any KQL 
prevented several rounds of trial-and-error. Once the JSON schema 
of `AuditLogs` was understood for each operation, the rules wrote 
themselves. This is the opposite of how most beginner labs work, 
where rules are copied from blogs and then "validated" against 
nothing.

### Entity mapping as a first-class concern

Mapping `Actor` and `TargetUser` as separate `Account` entities 
on every rule means Sentinel automatically correlates incidents 
across rules. The day a real attacker creates an account and 
escalates privileges, both detections will fire and Sentinel will 
group them. This is the kind of design decision that pays off 
later in incident investigation.

### SOAR automation, even if minimal

The notification Playbook is the simplest possible SOAR action 
(email), but having it deployed end-to-end demonstrates the full 
detection-to-response pipeline. Without it, the lab would be a 
detection-only exercise. With it, the lab is a complete (if 
small) SOC.

## What did not work

### The Windows VM detour

Significant time was lost trying to deploy a Windows VM for 
Security Event ingestion. Quota limitations in West Europe, 
incompatible VM sizes, and unexpected pricing for the smallest 
options ultimately led to abandoning that scope. In retrospect, 
the decision to skip Windows entirely and document it as 
"Identity-focused by design" was the correct call. The 
distraction cost was real.

### Gmail connector for the Playbook

Initial Playbook design used Gmail as the notification channel. 
Google's security policy blocks consumer Gmail accounts from 
combining with enterprise connectors like Microsoft Sentinel 
(GmailConnectorPolicyViolation). Resolution: a free Outlook.com 
account dedicated to lab notifications. In a real corporate 
environment this would be a corporate mailbox, Teams channel, or 
ticketing system integration.

### Lookup window blindness

The first Analytics Rule was configured with a one-hour lookup 
window matching its one-hour run frequency. This is fragile: 
ingestion latency of 15-30 minutes means events near the edge of 
the window are lost. Production rules typically use a five-minute 
frequency with a fifteen-minute lookup, balancing freshness 
against duplicate alerts. This was discovered only after a test 
user was created and the expected incident never appeared.

### Test runs of the Playbook return empty fields

Running the Playbook in test mode populated the email with the 
correct structure but empty values, because no real incident was 
attached to the test trigger. Only by running the Playbook 
manually against an existing real incident did the dynamic fields 
populate. This is a common point of confusion when learning 
Logic Apps and worth documenting.

## What I would do differently

### Start with the architecture diagram

The Mermaid diagram in the main README took fifteen minutes once 
the lab was nearly complete. Drawing it on day one would have 
forced clarity about which components are in scope and which are 
explicitly out of scope. Future labs will start with the diagram, 
not end with it.

### Version every artifact from the first commit

Some Analytics Rules and the Playbook were exported to JSON 
after the fact. If the workspace had been corrupted or deleted, 
recovery would have meant rebuilding from screenshots. From 
day one, every rule, every Playbook and every Workbook should be 
exported and committed alongside its documentation.

### Treat hunting queries as first-class

The service principal hunting query was added late in the 
process. In production detection engineering, hunting queries 
often outnumber deployed rules ten to one, because they 
represent the backlog of "interesting patterns, not yet tuned 
enough to alert on." Future labs will create a hunting query 
folder from day one and grow it as patterns are discovered.

## What I learned about myself

Building this lab in a compressed timeframe (four days, with 
real cost pressure and real fatigue) taught me that the limit 
in detection engineering is rarely technical knowledge. It is 
discipline: documenting decisions, resisting scope creep, 
deleting failed experiments instead of patching them, and 
writing things down before they become muscle memory.

This document is part of that discipline. If a future version 
of me reads this and remembers nothing else, the one thing to 
remember is: **scope discipline beats technical ambition every 
time**.

## What comes next

- Splunk SOC Home Lab (planned: August-October 2026)
- Detection-as-Code with Sigma rules (planned: November 2026)
- Active Directory Attack and Defense lab (planned: 2027)

These will reuse the patterns established here: scope first, 
controlled events, entity mapping, version control, and an 
honest lessons-learned document at the end.
