# Lab Architecture

## Overview

The lab is deployed on Microsoft Azure using a pay-as-you-go subscription
with the Microsoft Sentinel free trial active (10 GB/day ingestion quota
for 31 days).

The design prioritises three constraints:

1. **Zero unnecessary cost** — only free or trial-included services
2. **Realistic data** — actual Entra ID tenant logs, not only simulated data
3. **Reproducibility** — every component documented for redeployment

## High-level architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Microsoft Azure                          │
│                                                             │
│  ┌──────────────────────┐                                   │
│  │ Resource Group:      │                                   │
│  │ rg-lab-sentinel      │                                   │
│  │                      │                                   │
│  │  ┌───────────────┐   │                                   │
│  │  │ Log Analytics │   │   ┌──────────────────────────┐    │
│  │  │ Workspace:    │◄──┼───┤ Microsoft Entra ID       │    │
│  │  │ lab-sentinel  │   │   │ (Audit Logs)             │    │
│  │  └───────┬───────┘   │   └──────────────────────────┘    │
│  │          │           │                                   │
│  │  ┌───────▼───────┐   │   ┌──────────────────────────┐    │
│  │  │ Microsoft     │◄──┼───┤ Training Lab solution    │    │
│  │  │ Sentinel      │   │   │ (sample data)            │    │
│  │  └───────┬───────┘   │   └──────────────────────────┘    │
│  │          │           │                                   │
│  │  ┌───────▼────────┐  │                                   │
│  │  │ Analytics      │  │                                   │
│  │  │ Rules (KQL)    │  │                                   │
│  │  └───────┬────────┘  │                                   │
│  │          │           │                                   │
│  │  ┌───────▼────────┐  │                                   │
│  │  │ Incidents +    │  │                                   │
│  │  │ Playbooks      │  │                                   │
│  │  └────────────────┘  │                                   │
│  └──────────────────────┘                                   │
│                                                             │
│   Unified portal: security.microsoft.com (Defender XDR)     │
└─────────────────────────────────────────────────────────────┘
```

## Components

### Resource Group: rg-lab-sentinel

Logical container for all lab resources. Located in West Europe region
for latency proximity. Deletion of this resource group cleanly tears
down the entire lab.

### Log Analytics Workspace: lab-sentinel

The data store for all logs ingested into Sentinel. Configured with:

- Pay-as-you-go pricing tier (free trial active)
- 30-day retention by default
- Region: West Europe

### Microsoft Sentinel

Cloud-native SIEM and SOAR platform deployed on top of the Log Analytics
Workspace. Free trial active providing 10 GB/day ingestion quota.

### Microsoft Entra ID Connector

Currently ingesting **Audit Logs** only.

**Architectural decision:** Sign-in Logs require Entra ID P1 or P2
license, which is a paid SKU and outside the scope of a zero-cost lab.
Audit Logs provide sufficient signal to practise detections around
privileged role changes, user lifecycle events and configuration drift,
which are common SC-200 exam scenarios.

### Training Lab solution

Microsoft-provided package that ingests sample data and pre-built
incidents into the workspace for hands-on training without requiring
a production-scale data source.

### Portal: Microsoft Defender (security.microsoft.com)

As of 2025-2026, Microsoft has unified the SIEM (Sentinel) and XDR
(Defender) experiences in a single portal. The lab workspace is
connected to the Defender portal as the primary workspace, enabling
the converged experience for incidents and hunting.

## Cost controls

A monthly budget of **€10** is configured on the subscription with an
alert at 80% to prevent unexpected charges. With only Entra ID Audit
Logs and Training Lab data flowing, actual monthly cost during the
free trial period is expected to remain near zero.

## Future additions

- Windows VM (B1s/B2s with auto-shutdown) for Windows Security Events
- Logic Apps playbooks for SOAR automation
- Custom Workbooks for executive dashboards
## Scope Decision: Identity-Focused Detection

This lab focuses exclusively on identity-based threat detection 
in Microsoft Entra ID. The scope is intentional and grounded in 
two principles:

1. **Threat landscape priority.** According to recent Microsoft 
   Digital Defense Reports, identity compromise is the most 
   common initial access vector in cloud environments. Detecting 
   account creation, role assignment and identity manipulation 
   delivers the highest ROI per detection rule.

2. **Depth over breadth.** Rather than ingest many low-signal 
   data sources superficially, the lab prioritises mature, 
   well-tuned detection rules on a single high-value source, 
   with full validation, entity mapping and SOAR automation 
   wired end-to-end.

This is consistent with how mature SOCs prioritise detection 
engineering effort: identity-first, with additional data sources 
added incrementally as the detection backlog matures.

### Data sources intentionally out of scope (and why)

| Source | Reason for exclusion |
|---|---|
| Windows Security Events | Requires running VM; cost beyond the lab budget. Future scope. |
| Microsoft 365 Defender (full XDR) | Requires E5 licensing. Future scope when running in an enterprise tenant. |
| Sign-in Logs | Requires Entra ID P1/P2 licence (paid). Documented as architectural limitation. |
| Azure Activity Logs | Available but skipped to keep the scope tightly focused on identity. |
