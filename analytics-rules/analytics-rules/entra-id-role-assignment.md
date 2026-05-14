# Entra ID — Role Assignment

## Overview

| Field | Value |
|---|---|
| **Rule type** | Scheduled query rule |
| **Severity** | Medium |
| **Status** | Enabled |
| **Data source** | `AuditLogs` (Microsoft Entra ID) |
| **Run frequency** | Every 1 hour |
| **Lookup window** | Last 1 hour |
| **Event grouping** | Trigger an alert for each event |

## MITRE ATT&CK mapping

| Tactic | Technique | Sub-technique |
|---|---|---|
| Privilege Escalation (TA0004) | T1098 — Account Manipulation | T1098.003 — Additional Cloud Roles |

## Detection logic

The rule triggers when a user is successfully added to a directory
role in Microsoft Entra ID. Four fields are extracted from the
nested log structure:

- `Actor` — the principal that performed the assignment
- `TargetUser` — the UPN of the user receiving the role
- `TargetDisplayName` — the friendly name of the user
- `RoleName` — the name of the role assigned (extracted from
  `TargetResources.modifiedProperties`)

See the full KQL query in `kql-queries/entra-id/role-assignment-detection.kql`.

## Entity mapping

| Entity type | Identifier | Source field |
|---|---|---|
| Account | Name | `TargetUser` |
| Account | Name | `Actor` |

This mapping enables cross-incident correlation with the
`Entra ID - User Account Creation` rule. When both rules fire
against the same `TargetUser` within a short timeframe, this
represents a high-confidence indicator of attacker activity:
account creation followed by privilege escalation.

## Validation

The rule logic was validated by querying historical data containing
a controlled role assignment event:

- **Target user:** maria.lopez
- **Role assigned:** Reports Reader
- **Timestamp:** 13 May 2026, 11:11 UTC

The query returned the expected event with all enriched fields
correctly populated.

## Tuning recommendations

In production environments, this rule benefits from progressive
tuning:

- **Filter by privileged roles only.** Add a `where RoleName in ("Global Administrator", "Privileged Role Administrator", "User Administrator", ...)` clause to suppress noise from low-impact role assignments.
- **Correlate with account age.** Escalate severity if the target user account is less than 1 hour old at the time of role assignment.
- **Exclude approved workflows.** Suppress alerts from known automation principals (PIM, HR provisioning).
- **Time-of-day filtering.** After-hours role assignments warrant additional scrutiny.

## Deployment

The rule is provided as an ARM template / JSON export in this folder
(`entra-id-role-assignment.json`) for reproducible deployment in other
Sentinel workspaces.
