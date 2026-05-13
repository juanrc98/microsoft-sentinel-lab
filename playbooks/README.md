# Playbooks

Microsoft Sentinel Playbooks (Azure Logic Apps) exported as JSON
templates for incident response automation (SOAR).

## Purpose

Automated response workflows triggered by Sentinel incidents:

- Notification to security team channels (Teams, email)
- Enrichment with threat intelligence sources
- Containment actions (account disable, IP block)
- Ticketing system integration

## File naming

`{playbook-name}.json` — Logic App ARM template exports.

## Trigger types

- **Incident trigger** — fires when a Sentinel incident is created
- **Alert trigger** — fires on individual alerts
- **Entity trigger** — manual execution against entities (user, IP, host)
