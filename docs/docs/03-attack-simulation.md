# Controlled Event Generation

## Purpose

Generate real audit events in the lab tenant to validate detection
logic against actual log entries — not synthetic or demo data.

This document describes the methodology used to produce reproducible
security-relevant events for detection engineering and tuning.

## Why real events instead of demo data

Microsoft's public demo log workspaces have limited availability and
do not always reflect the schema and behaviour of current production
tenants. Generating events directly in the lab tenant ensures:

- **Schema accuracy** — logs match what a real SOC would see
- **End-to-end validation** — from event source to detection alert
- **Documentation evidence** — each event has a known cause and timestamp

## Methodology

For each scenario:

1. Define the security-relevant event to simulate
2. Document the expected log signature (table, operation, fields)
3. Execute the action in the tenant
4. Verify the log arrives in the workspace (within 15-30 minutes)
5. Write a KQL query that surfaces the event
6. Map the detection to MITRE ATT&CK
7. Optionally convert the query into a Sentinel Analytics Rule

## Scenarios implemented

### Scenario 1 — User lifecycle: account creation

**Action:** Create a new user `maria.lopez` in Entra ID.

**Expected log:**
- Table: `AuditLogs`
- Category: `UserManagement`
- Operation: `Add user`
- Result: `success`

**Security relevance:** Bulk user creation outside business hours,
or creation of users with admin-like naming, can indicate persistence
attempts (MITRE ATT&CK T1136 — Create Account).

### Scenario 2 — Privilege change: role assignment

**Action:** Assign `Reports Reader` role to `maria.lopez`.

**Expected log:**
- Table: `AuditLogs`
- Category: `RoleManagement`
- Operation: `Add member to role`
- Result: `success`

**Security relevance:** Assignment of any role — especially privileged
ones — to a recently created user is a strong indicator of privilege
escalation activity (MITRE ATT&CK T1098 — Account Manipulation).

## Future scenarios

- Bulk user creation outside business hours
- Role assignment to multiple users in short timeframe
- Application registration with broad permissions
- Conditional Access policy modification

## Operational notes

- Test users use realistic naming conventions to mimic enterprise
  environments
- All test user credentials are stored locally in a password manager,
  never committed to this repository
- Test users are kept disabled or deleted after each detection
  validation cycle to avoid clutter
