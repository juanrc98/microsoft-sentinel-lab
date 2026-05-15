# Workbooks

Sentinel Workbooks deployed in the lab for SOC visibility and 
metrics tracking.

## Available Workbooks

### Identity Threat Overview

File: `identity-threat-overview.json`

Custom Workbook providing visual context over the identity-focused 
detections deployed in this lab. Designed to give a SOC analyst 
a one-screen overview of identity activity and detection coverage.

**Tiles included:**

1. Audit Logs volume over time
2. Operations breakdown by type
3. Top actors performing administrative operations
4. MITRE ATT&CK coverage matrix
5. Incidents by severity

## Deployment

To import this Workbook into another Microsoft Sentinel workspace:

1. Open Microsoft Sentinel in the Azure Portal or Defender Portal
2. Navigate to **Workbooks**
3. Click **+ Add workbook**
4. Click **Edit** to enter edit mode
5. Click the **Advanced Editor** icon (`</>`)
6. Replace the default content with the JSON from this folder
7. Click **Apply**
8. Save the Workbook with a name of your choice
