# Microsoft Sentinel SOC Lab

> Hands-on Microsoft Sentinel deployment in Azure for SOC operations,
> threat detection with KQL, and SC-200 certification preparation.

![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoft-azure&logoColor=white)
![Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-00BCF2?style=flat&logo=microsoft&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-0078D4?style=flat&logo=microsoft&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

## Overview

This repository documents the design, deployment and continuous improvement
of a Microsoft Sentinel SOC laboratory built on Microsoft Azure. The lab
is used for hands-on practice on detection engineering, KQL development,
incident investigation and SOAR automation with Logic Apps.

The project is part of my preparation for the **Microsoft SC-200:
Security Operations Analyst** certification, scheduled for July 2026.

## Objectives

- Deploy a functional cloud-native SIEM environment
- Build custom detection rules mapped to MITRE ATT&CK
- Develop KQL queries for hunting and investigation
- Automate incident response with Logic Apps playbooks
- Document every component end-to-end as a reusable reference

## Architecture

Resource Group → Log Analytics Workspace → Microsoft Sentinel
→ Data Connectors (Entra ID, Windows Security Events)
→ Analytics Rules (KQL) → Incidents → Playbooks (Logic Apps)

> Detailed architecture diagram in `docs/01-architecture.md`

## Tech Stack

| Component | Purpose |
|---|---|
| Microsoft Azure | Cloud infrastructure |
| Microsoft Sentinel | Cloud-native SIEM & SOAR |
| Log Analytics Workspace | Log ingestion and storage |
| Microsoft Entra ID | Identity logs (SignIn, Audit) |
| KQL | Detection and hunting queries |
| Logic Apps | SOAR automation |
| MITRE ATT&CK | Detection framework mapping |

## Repository Structure
microsoft-sentinel-lab/
├── docs/                  # Architecture, setup, lessons learned
├── kql-queries/           # Detection and hunting queries
├── analytics-rules/       # Sentinel Analytics Rules (JSON exports)
├── playbooks/             # Logic Apps playbooks (JSON exports)
└── screenshots/           # Visual evidence of each milestone
## Project Status

🚧 **In active development.** Updated weekly as I progress through
the SC-200 learning paths and add new detections to the lab.

## About me

**Juan Rodríguez Castellano** — SOC Analyst transitioning to Cloud
Security in Microsoft environments.

[LinkedIn](https://linkedin.com/in/juanrodriguez) ·
[Portfolio](https://juanrc98.github.io)
