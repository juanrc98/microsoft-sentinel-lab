# Data Connectors

## Overview

This document describes the data connectors configured in the lab
Sentinel workspace, the rationale for each one, and the licensing
or cost considerations that drove the configuration decisions.

Each connector is evaluated on three axes:

- **Detection value** — what security signal does it provide?
- **Licensing cost** — does it require paid licences (P1, P2, E5)?
- **Ingestion volume** — does it fit within the free-tier quota?

## Active connectors

### Microsoft Entra ID

**Status:** Connected
**Solution version:** Installed from Content Hub
**Data types enabled:**

| Data type | Enabled | Notes |
|---|---|---|
| Audit Logs | ✅ Yes | Free, included with any Entra ID tier |
| Sign-In Logs | ❌ No | Requires Entra ID P1 or P2 (paid) |
| Non-Interactive Sign-Ins | ❌ No | Requires P1/P2 |
| Service Principal Sign-Ins | ❌ No | Requires P1/P2 |
| Provisioning Logs | ❌ No | Not required for current scope |

**Architectural decision:** Sign-In Logs require Entra ID P1 or P2
licence, which is out of scope for a zero-cost lab. Audit Logs are
sufficient for detection scenarios around:

- User lifecycle (creation, deletion, modification)
- Role and permission changes
- Application and service principal management
- Conditional Access policy changes
- Group membership modifications

These cover a significant portion of identity-related MITRE ATT&CK
techniques relevant to the SC-200 exam scope.

**Ingestion expectation:** Low volume (kilobytes/day) for a single
tenant with limited administrative activity.

## Connectors evaluated but not enabled

### Microsoft Entra ID Protection

**Reason not enabled:** Requires Entra ID P2 licence. Provides
risk-based sign-in detection (risky users, risky sign-ins, leaked
credentials). Out of scope for a zero-cost lab.

### Microsoft Defender for Endpoint

**Reason not enabled:** Requires Defender for Endpoint Plan 2 licence
and onboarded devices. Will be evaluated in a future iteration when
a Windows VM is added to the lab.

### Microsoft Defender for Cloud

**Reason not enabled:** Requires enabling Defender plans per resource
type. Generates billable events. Will be considered selectively for
specific resource types if added in the future.

## Future connectors

Planned additions based on lab evolution:

- Windows Security Events (when a Windows VM is added)
- Linux Syslog (when a Linux VM is added)
- Office 365 (if a free Microsoft 365 Developer tenant is provisioned)
- Threat intelligence feeds (free OSINT sources via TAXII)

## References

- [Microsoft Entra ID connector documentation](https://learn.microsoft.com/azure/sentinel/connect-azure-active-directory)
- [Sentinel pricing and data retention](https://azure.microsoft.com/pricing/details/microsoft-sentinel/)
