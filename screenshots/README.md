# Screenshots

Visual evidence of each lab milestone and configuration.

## Naming convention

Files follow the pattern: `NN-short-description.png`

Where `NN` is a two-digit sequence number reflecting chronological
order of the lab build-out.

## Index

| # | File | Description |
|---|---|---|
| 01 | `01-sentinel-defender-integration.png` | Workspace `lab-sentinel` connected to Defender XDR portal |
| 02 | `02-entra-id-connector-config.png` | Entra ID Data Connector configuration (Audit Logs only) |
| 03 | `03-user-creation-maria-lopez.png` | Test user creation event |
| 04 | `04-role-assignment-reports-reader.png` | Role assignment to test user |
| 05 | `05-kql-detection-user-creation-result.png` |  First KQL detection executed against real audit data — user creation event |
| 06 | 06-analytics-rule-active.png | First Analytics Rule deployed: "Entra ID - User Account Creation" (T1136.003) |
| 07 | 07-first-incident-generated.png | First incident generated automatically by Analytics Rule |
| 08 | 08-incident-details-view.png | Incident details: entities, timeline, MITRE mapping |
| 09 | `09-analytics-rules-active.png` | Two Analytics Rules active covering Persistence (T1136) and Privilege Escalation (T1098) |
| 10 | 10-playbook-email-notification.png | SOAR Playbook: automated email notification with incident details |
| 11 | `11-hunting-query-validated.png` | Service Principal Creation hunting query (T1098.001) validated with controlled event |



Screenshots are referenced from the corresponding documentation
files in `docs/` and from inline references in `kql-queries/`.
