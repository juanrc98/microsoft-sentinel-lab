# Playbook: Notify Sentinel Incident

## Overview

| Field | Value |
|---|---|
| **Type** | Azure Logic App (Consumption) |
| **Trigger** | Microsoft Sentinel incident creation |
| **Action** | Send email notification |
| **Region** | West Europe |
| **Resource Group** | rg-lab-sentinel |

## Purpose

Automatically notifies the SOC inbox when a new incident is created
in Microsoft Sentinel. Demonstrates the SOAR (Security Orchestration,
Automation and Response) capability of the platform end-to-end.

## Flow

```
Microsoft Sentinel Incident (trigger)
        ↓
Send email (Outlook.com)
        ↓
Notification delivered to SOC inbox
```

## Email content

The email subject includes the incident title for quick triage.
The body includes:

- **Title** — incident name as defined by the Analytics Rule
- **Severity** — Low / Medium / High / Informational
- **Status** — New / Active / Closed
- **Created at** — UTC timestamp of incident creation
- **Direct link** to the incident in the Defender portal

## Architectural decisions

**Why Outlook.com instead of Gmail:** The Gmail connector enforces
a security policy that prevents combining consumer-grade Gmail
accounts with enterprise connectors like Microsoft Sentinel
(GmailConnectorPolicyViolation). A free Outlook.com account was
used as a workaround for the lab. In production, this would be
replaced by a corporate Microsoft 365 mailbox, a Teams channel
or a ticketing integration (ServiceNow, Jira).

**Why Consumption plan:** Pay-per-execution pricing means the
Playbook only costs when it actually runs. With the expected
incident volume in this lab (a handful per day), monthly cost
is effectively zero.

## Validation

The Playbook was validated by manually running it against
Incident ID 1 (`Entra ID - User Account Creation`). Email
delivered with all dynamic fields correctly populated.

Evidence: `screenshots/10-playbook-email-notification.png`

## Future improvements

- Connect to corporate Teams channel instead of email
- Add IP reputation enrichment via VirusTotal connector
- Add conditional logic: only notify for Medium+ severity
- Auto-disable user account for High severity privilege escalation
- Integration with ticketing system for tracking

## Deployment

The Playbook is provided as an ARM template / JSON export in this
folder for reproducible deployment in other Sentinel workspaces.
