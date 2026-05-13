# Analytics Rules

Microsoft Sentinel Analytics Rules exported as JSON, version-controlled
for reproducibility and audit purposes.

## Purpose

Detection rules deployed in the lab Sentinel workspace are exported
here as JSON to enable:

- Version control of detection logic
- Reproducible deployment in other tenants
- Documentation of rule evolution and tuning
- Code review of detection engineering decisions

## File naming

`{detection-name}.json` — exported directly from the Sentinel Analytics
Rules blade or via REST API.

## Companion documentation

Each rule has a corresponding entry in the relevant `kql-queries/` file
explaining the detection logic, threshold rationale and MITRE ATT&CK
mapping in detail.
