# KQL Queries

This folder contains all KQL queries authored in the lab, 
organised by data source and purpose.

## Folder structure
kql-queries/
└── entra-id/
├── user-creation-detection.kql       (deployed as Analytics Rule)
├── role-assignment-detection.kql     (deployed as Analytics Rule)
└── service-principal-creation.kql    (hunting query)
## Detection vs Hunting

Two categories of queries live here:

**Detection queries** are deployed as Sentinel Analytics Rules. 
They run on a schedule, generate alerts and create incidents 
automatically. They are tuned for high-confidence, low-false-
positive output.

**Hunting queries** are analyst-driven. They surface patterns 
that warrant human review but are not yet mature enough (or 
high-signal enough) to fire automated alerts. Hunting queries 
typically graduate to detection rules once validated against 
real data and tuned for noise.

## Current state

| Query | Type | MITRE | Status |
|---|---|---|---|
| user-creation-detection | Detection | T1136.003 | Deployed (Low) |
| role-assignment-detection | Detection | T1098.003 | Deployed (Medium) |
| service-principal-creation | Hunting | T1098.001 | Manual hunt |

## Methodology

Every query follows the same authoring flow:

1. Generate a controlled event matching the target activity
2. Inspect the raw log in Log Analytics to understand the schema
3. Write a minimal query that surfaces the event without noise
4. Document false-positive scenarios and tuning recommendations
5. If applicable, promote to Analytics Rule with entity mapping
6. Validate end-to-end with another controlled event

This is the same flow used in production detection engineering 
teams, scaled down to lab volume.
