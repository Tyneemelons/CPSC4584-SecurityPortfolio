# Week 6: Deleted Files on a Terminated Employee's Laptop

**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 5, 2026
**Analyst:** Anderson Tyree
**Case ID:** FOR-2026-1005-001

---

## Incident Summary

A Maplewood billing coordinator retained possession of a company laptop after termination due to a coordination failure between HR and IT. During a subsequent forensic investigation, file deletion activity was identified during the custody gap, and five files were recovered from unallocated disk space through forensic analysis.

---

## Chain of Custody Status

**Gap Period:** Oct 3, 2:47 PM to Oct 5, 9:30 AM (approximately 43 hours)

**Gap Significance:** The device remained outside Maplewood custody during this period, reducing confidence in activities that occurred on the laptop before its return. The custody gap does not invalidate the recovered evidence, but it must be documented because the analyst cannot independently verify who used the device or what actions occurred while it was outside organizational control.

**Documented By:** Analyst on receipt of device

---

## Recovered File Evidence Classification

| Filename | Type | Evidentiary Value | Finding |
|----------|------|-------------------|---------|
| insurance_export_final.xlsx | Spreadsheet | High | Recovered completely and modified shortly before deletion. May contain business-related data relevant to the investigation timeline. |
| mhs_billing_schema_db.sql | SQL script | High | Recovered completely and appears related to Maplewood billing systems. Modification and deletion timestamps may assist timeline reconstruction. |
| vendor_contact_list_external.docx | Document | Medium | Recovered completely and may provide context regarding external contacts, although additional review
