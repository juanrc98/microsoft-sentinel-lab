# Incident Walkthrough — Entra ID User Account Creation

## Context

This document walks through the first end-to-end detection cycle
executed in the lab. It documents the full lifecycle of an incident:
from event generation to detection and analyst review.

The goal is to demonstrate how a Detection Engineer validates a
custom Analytics Rule against controlled real-world activity.

## Scenario

A new user account was created in the lab Entra ID tenant. The 
detection rule `Entra ID - User Account Creation` (T1136.003) was 
expected to surface the event automatically as a Sentinel incident.

## Step-by-step walkthrough

### 1. Event generation

A test user was created in the tenant via the Entra ID admin portal:

- **User principal name:** `carlos.fernandez@<tenant>.onmicrosoft.com`
- **Display name:** Carlos Fernández
- **Action performed by:** the lab administrator account
- **Timestamp:** 14 May 2026, 10:08:26 UTC

This action generates an `Add user` operation in the `AuditLogs` table,
categorised as `UserManagement`.

### 2. Ingestion to Microsoft Sentinel

Audit events are forwarded from Entra ID to the `lab-sentinel`
workspace via the Microsoft Entra ID data connector. Typical
ingestion latency observed in the lab: **15 to 30 minutes** from
event timestamp to log availability.

Validation query:

```kql
AuditLogs
| where OperationName == "Add user"
| where Result == "success"
| order by TimeGenerated desc
```

### 3. Analytics Rule evaluation

The Analytics Rule `Entra ID - User Account Creation` is scheduled
to run every 1 hour, looking back at the last 1 hour of data.

On its next scheduled run after ingestion, the rule:

1. Executed the KQL query against the workspace
2. Matched the `Add user` event for Carlos Fernández
3. Mapped entities: `Actor` (admin) and `TargetUser` (new account)
4. Generated an alert with severity `Low`
5. Created Incident #1 in the workspace

### 4. Incident review in the Defender portal

The incident was reviewed in the unified Defender portal under
**Microsoft Sentinel → Incidents**.

**Incident metadata:**

| Field | Value |
|---|---|
| Incident ID | 1 |
| Title | Entra ID - User Account Creation |
| Severity | Low |
| Status | Active |
| Categories | Persistence |
| Impacted assets | 2 Accounts |
| First activity | 14 May 2026, 12:08 PM |
| Creation time | 14 May 2026, 12:52 PM |

**Latency observed (event → incident):** approximately 44 minutes,
consistent with the 1-hour rule schedule plus ingestion delay.

### 5. Tabs reviewed

- **Attack story** — incident graph showing 2 user entities related
  through the `Add user` operation
- **Alerts** — single linked alert with full event context
- **Assets** — 2 user accounts mapped as impacted entities
- **Summary** — high-level overview for triage handoff

## Analyst notes

For a real SOC analyst handling this incident, the next investigation
steps would typically include:

1. Verify if the actor account is authorised to create users
2. Cross-reference the new account against HR ticketing system
3. Check whether the new account was added to privileged roles
   within a short timeframe (privilege escalation indicator)
4. Inspect sign-in activity of the new account for anomalies
5. Determine if this is part of a known onboarding workflow

In this controlled lab scenario, the event was a documented test,
so the incident would be **closed as Benign Positive (B-TP)** with
notes referencing this walkthrough.

## Lessons learned

- **Lookup window matters.** A rule configured with a 1-hour lookup
  window cannot detect events older than 1 hour. Tuning the window
  is a critical balance between coverage and ingestion volume.
- **Entity mapping is essential.** Without entity mapping, Sentinel
  cannot correlate incidents across rules. Mapping `Actor` and
  `TargetUser` enables cross-rule correlation in future detections.
- **End-to-end validation requires real data.** Test data must match
  the schema and behaviour of production logs to validate detections
  reliably.

## Evidence

- `screenshots/06-analytics-rule-active.png`
- `screenshots/07-first-incident-generated.png`
- `screenshots/08-incident-details-view.png`
