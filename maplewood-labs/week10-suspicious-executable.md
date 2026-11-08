# Week 10: Suspicious Executable Running on a Server

**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** November 2, 2026
**Analyst:** Tyree Anderson
**Case ID:** EXEC-2026-1030-001

---

## Incident Summary

An unauthorized executable named mhs_monitor was discovered running on a Maplewood EHR application server for more than 72 hours. Investigation identified repeated outbound connections to 185.220.101.47, which matches the destination indicator from the Week 7 network anomaly, requiring further review and escalation.

---

## Static Analysis Findings

| # | Tool | Output Line | Finding | Severity |
|---|------|-------------|---------|----------|
| 1 | strings | 185.220.101.47 | Hardcoded external destination matching prior incident data | High |
| 2 | strings | /tmp/.mhs_session | Hidden dot-prefixed temporary file path indicating possible artifact storage | Medium |
| 3 | strings | Mozilla/5.0 (compatible; MHSMonitor/1.0) | Browser-like HTTP user-agent string suggesting web communications | Medium |
| 4 | readelf | [NEEDED] libcurl.so.4 | Network communication library supporting HTTP/HTTPS connectivity | High |

---

## Process-Level IOCs

| IOC | Observed Value | Significance |
|-----|----------------|--------------|
| Not in inventory | No package manager entry | The binary is unauthorized and cannot be linked to approved software. |
| Parent process | init (PID 1) | Indicates the process was started by the system and persisted after launch, but does not by itself prove malicious activity. |
| Repeated outbound connection | 185.220.101.47 every 4 min | The repeated pattern resembles beaconing behavior, although the connection purpose cannot be confirmed through static evidence alone. |
| Dot-prefixed file | /tmp/.mhs_session | Hidden files are commonly overlooked
