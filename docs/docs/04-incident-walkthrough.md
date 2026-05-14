# Incident Walkthrough — Entra ID User Account Creation

## Context

This document walks through the first end-to-end detection cycle
executed in the lab. It documents the full lifecycle of an incident:
from event generation to detection and analyst review.

The goal is to demonstrate how a Detection Engineer validates a
custom Analytics Rule against controlled real-world activity.

## Scenario

A new user account was created in the lab Entra ID tenant. The 
detection rule `Entra ID - User Account Creation` (T1136.003) was 
expected to surface the event automatically as a Sentinel incident.

## Step-by-step walkthrough

### 1. Event generation

A test user was created in the tenant via the Entra ID admin portal:

- **User principal name:** `carlos.fernandez@<tenant>.onmicrosoft.com`
- **Display name:** Carlos Fernández
- **Action performed by:** the lab administrator account
- **Timestamp:** 14 May 2026, 10:08:26 UTC

This action generates an `Add user` operation in the `AuditLogs` table,
categorised as `UserManagement`.

### 2. Ingestion to Microsoft Sentinel

Audit events are forwarded from Entra ID to the `lab-sentinel`
workspace via the Microsoft Entra ID data connector. Typical
ingestion latency observed in the lab: **15 to 30 minutes** from
event timestamp to log availability.

Validation query:

```kql
