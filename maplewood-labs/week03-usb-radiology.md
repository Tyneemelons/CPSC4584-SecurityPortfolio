# Week 3: Unauthorized USB Drive in Radiology

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** September 14, 2026  
**Analyst:** Tyree Anderson 
**Incident ID:** INC-2026-0914-001  

---

## Incident Summary

An unmarked and unauthorized USB flash drive was discovered plugged into workstation MHS-RAD-WS-03 in the Radiology Imaging Suite at Maplewood Health System. Because the device was found in a restricted clinical area and its contents were unknown, the workstation was isolated and the incident was escalated while preserving the original evidence for future forensic examination.

---

## Chain of Custody

Chain of custody is important because it documents who handled evidence, when it was transferred, and what actions were performed. In this incident, the USB drive was removed by a facilities technician, logged and tagged by the IT Helpdesk, and transferred to SOC custody without being connected to another system, helping preserve the integrity of the evidence.

---

## Key Encoding Finding

**String Found:** Y3VybCAtcyAtbyAvZGV2L251bGw=

**Encoding Type:** Base64

**Decoded Content:** `curl -s -o /dev/null`

**Significance:** The decoded string is a curl command fragment that suppresses output and discards returned data. During an investigation, analysts may encounter encoded strings that conceal commands or configuration information. However, this fragment alone does not prove malicious activity because legitimate administrative scripts can use the same options. Additional context, such as the destination URL, execution history, and related logs, would be required before determining intent.

---

## Terminal Commands Used

| Command | Purpose |
|---------|---------|
| `echo "unauthorized access" \| base64` | Demonstrated how plain text can be converted into Base64 format. Base64 changes the representation of data but does not provide encryption or confidentiality. |
| `echo "Y3VybCAtcyAtbyAvZGV2L251bGw=" \| base64 -d` | Decoded a Base64 string and revealed a curl command fragment, showing how encoded content can be interpreted during an investigation. |
| `xxd .bashrc \| head -6` | Displayed file data in hexadecimal and ASCII formats. This demonstrated how analysts inspect file contents, offsets, and byte values while looking for signatures or other indicators. |
| `strings .bashrc \| grep -i "path\|export\|alias"` | Filtered readable strings for specific keywords, allowing relevant configuration entries to be identified more efficiently during analysis. |

---

## Escalation Recommendation

I recommend escalating this incident to Tier 2 for deeper investigation. The strongest evidence is that an unauthorized and unmarked USB device was found connected to a restricted Radiology workstation with access to clinical systems. Known facts include the documented chain of custody, workstation isolation, and the absence of outbound alerts during the monitoring window. However, important questions remain unanswered, including who connected the device, how long it was present, whether any files were executed, and whether any data was accessed or transferred. As a Tier 1 analyst, I cannot perform forensic acquisition of the original media or conduct staff interviews, making escalation necessary.

---

## Reflection and Career Connection

This lab reinforced the importance of evidence preservation, chain of custody procedures, and separating facts from assumptions during an investigation. The terminal exercises provided hands-on practice with identifying Base64 encoding, decoding command fragments, examining files with xxd, and locating relevant information using strings and grep. These skills are valuable for a Tier 1 SOC analyst because they support safe evidence review and help analysts gather information without modifying original evidence. The exercise also highlighted the importance of escalation when available information is insufficient to determine the full scope or impact of an incident.

---

*CPSC 4584 | Governors State University | Fall 2026*
