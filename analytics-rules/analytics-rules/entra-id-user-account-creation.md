# Entra ID — User Account Creation

## Overview

| Field | Value |
|---|---|
| **Rule type** | Scheduled query rule |
| **Severity** | Low |
| **Status** | Enabled |
| **Data source** | `AuditLogs` (Microsoft Entra ID) |
| **Run frequency** | Every 1 hour |
| **Lookup window** | Last 1 hour |
| **Event grouping** | Trigger an alert for each event |

## MITRE ATT&CK mapping

| Tactic | Technique | Sub-technique |
|---|---|---|
| Persistence (TA0003) | T1136 — Create Account | T1136.003 — Cloud Account |

## Detection logic

The rule triggers when a successful user creation event is observed
in the Microsoft Entra ID tenant. Three fields are extracted from the
nested log structure for context:

- `Actor` — the principal that performed the creation
- `TargetUser` — the UPN of the newly created account
- `TargetDisplayName` — the friendly name of the new account

See the full KQL query in `kql-queries/entra-id/user-creation-detection.kql`.

## Entity mapping

Two entities are mapped to enable cross-incident correlation:

| Entity type | Identifier | Source field |
|---|---|---|
| Account | Name | `TargetUser` |
| Account | Name | `Actor` |

This allows Sentinel to link incidents involving the same user across
different rules and timeframes.

## Validation

The rule was validated against a controlled event generated in the
lab tenant: creation of test user `maria.lopez`. The rule successfully
identified the event and produced an alert.

Evidence: `screenshots/06-analytics-rule-active.png`

## Tuning recommendations

In a production environment, this rule would generate noise from
legitimate user provisioning. Recommended tunings before production
deployment:

- Exclude known provisioning service principals via `Actor` filter
- Add time-of-day filter to highlight after-hours creations
- Correlate with HR ticketing system to suppress approved creations
- Escalate severity if the new account is added to a privileged role
  within 1 hour of creation

## Deployment

The rule is provided as an ARM template / JSON export in this folder
(`entra-id-user-account-creation.json`) for reproducible deployment
in other Sentinel workspaces.
