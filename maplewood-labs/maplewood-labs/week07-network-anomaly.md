# Week 7: Network Traffic Anomaly During Night Shift
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 15, 2026
**Analyst:** Tyree Anderson
**Case ID:** NET-2026-1012-001 (related: FOR-2026-1005-001)

---

## Incident Summary

Network flow analysis identified several anomalous connections during the October 3-4 night shift, including a large outbound transfer and a beacon-like communication pattern originating from Marcus Webb's laptop. These findings correlate with Week 6 disk evidence involving the modification and deletion of insurance_export_final.xlsx and warrant further investigation through the approved incident response process.

---

## Network Flow Analysis

| Flow | Source | Destination | Classification | Finding |
|------|--------|-------------|----------------|---------|
| 1 | Webb laptop | 185.220.101.47 | Data Exfiltration | A 2,457,814-byte HTTPS transfer occurred shortly after insurance_export_final.xlsx was modified. The timing and size support possible unauthorized data transfer, but the flow record does not identify the content transferred. |
| 2 | Webb laptop | 185.220.101.47 | C2 Beacon | Repeated 312-byte HTTPS connections occurred every four minutes from 11:15 PM through 6:42 AM. The regular timing and consistent size are consistent with beaconing behavior, but the flow data alone does not prove command-and-control activity. |
| 3 | EHR prod server | AWS | Authorized Traffic | The traffic matches the documented nightly EHR backup process between the production EHR server and the AWS-hosted vendor environment. This activity is consistent with expected operations. |
| 4 | Radiology WS | 8.8.8.8 | DNS Anomaly | The workstation generated unusual DNS query activity during the review period. Additional investigation would be required to determine whether the activity was benign or malicious. |
| 5 | Billing WS | 203.0.113.99 | C2 Beacon | Nine HTTP connections of similar size occurred every four minutes to an unapproved destination. The pattern is consistent with beaconing behavior, but the evidence does not independently prove command-and-control communications. |

---

## Disk and Network Evidence Correlation

**Disk (Week 6):** insurance_export_final.xlsx was approximately 2.4 MB, last modified at 11:47 PM, and deleted at 11:52 PM.

**Network (Week 7):** Flow 1 shows a 2,457,814-byte transfer from Webb's laptop to 185.220.101.47 beginning at 11:48 PM. Flow 2 shows repeated 312-byte connections to the same destination beginning at 11:15 PM.

**Correlation:** The evidence supports a sequence in which the spreadsheet was modified, a large outbound transfer occurred shortly afterward, and the file was then deleted. The beacon-like connections began before the larger transfer and continued for several hours, establishing a timeline that may be relevant to the investigation.

**Limitation:** Network flow records identify endpoints, timing, ports, protocols, and approximate data volume, but they do not reveal encrypted payload contents. Without packet captures, transfer logs, or another corroborating artifact, it cannot be confirmed that the spreadsheet itself was transmitted.

---

## Network Artifact Inspection Commands Used

| Command | Purpose |
|---------|---------|
| `curl -I https://example.com` | Retrieved HTTP response headers to examine the server response, status information, and available metadata without intentionally downloading the page content. |
| `curl -v --max-time 5` | Displayed connection details, TLS negotiation
