# KQL Queries

Kusto Query Language (KQL) queries developed throughout the lab,
organized by data source.

## Structure

- `entra-id/` — Queries against Microsoft Entra ID logs (SigninLogs, AuditLogs)
- `windows/` — Queries against Windows Security Events
- `hunting/` — Threat hunting queries (proactive, not tied to alerts)

## Naming convention

Each `.kql` file follows the pattern:

`{detection-name}.kql`

Every query includes inline comments with:

- Purpose of the detection
- Threshold and time window rationale
- MITRE ATT&CK technique mapping (when applicable)
- Known limitations or false positive considerations
