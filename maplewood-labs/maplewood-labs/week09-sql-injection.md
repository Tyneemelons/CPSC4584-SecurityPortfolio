# Week 9: SQL Injection Attempt on Appointment Scheduling System
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 26, 2026
**Analyst:** Tyree Anderson
**Case ID:** SQLI-2026-1024-001

---

## Incident Summary

On October 24, 2026, six search strings were submitted to the Maplewood appointment scheduling system from a single source IP address during a 47-minute period. The sequence progressed from normal user input to increasingly suspicious SQL injection testing, culminating in a targeted UNION SELECT attempt referencing patient information and requiring escalation for further review.

---

## SQL Injection Log Analysis

| Entry | Time | Decoded Input | Type | Severity |
|-------|------|---------------|------|----------|
| 1 | 02:14:07 | Johnson | Benign Input | Info |
| 2 | 02:14:52 | ' | Quote Probe | Medium |
| 3 | 02:18:31 | Smith' -- | Comment Syntax Test | High |
| 4 | 02:23:14 | ' OR '1'='1 | Tautology Pattern | High |
| 5 | 02:48:39 | ' UNION SELECT NULL,NULL,NULL -- | UNION Injection Attempt | Critical |
| 6 | 03:01:22 | ' UNION SELECT patient_name,date_of_birth,ssn FROM patients -- | Targeted PHI Extraction Attempt | Critical |

---

## Attack Progression Summary

**Entry 1:** The value "Johnson" appears to be normal user input and establishes a baseline for comparison with later entries. No indicators of SQL injection activity are present in this entry.

**Entry 2:** The single quote character appears to be a quote probe designed to test how the application handles unexpected SQL syntax. A successful response could reveal whether user input is being incorporated directly into database queries.

**Entry 3:** The input `Smith' --` includes SQL comment syntax. This pattern may be testing whether a quote can terminate part of a query and whether the remainder of the statement can be ignored by the database engine.

**Entry 4:** The input `' OR '1'='1` is a tautology pattern commonly associated with SQL injection attempts. If successful, this type of input could alter application behavior by causing conditions to evaluate as true.

**Entry 5:** The input `' UNION SELECT NULL,NULL,NULL --` appears to be a UNION-based SQL injection attempt. The use of NULL values may be testing the number and structure of columns required for a successful UNION query.

**Entry 6:** The input `' UNION SELECT patient_name,date_of_birth,ssn FROM patients --` is a targeted attempt to access sensitive patient information. The use of specific column names suggests possible schema knowledge or educated guessing by the actor. However, the access log only confirms what was submitted and does not prove that the query executed successfully or that any patient data was returned.

---

## Remediation Summary

**Primary application control:** The recommended primary defense is the use of parameterized queries or prepared statements. Parameterization separates SQL code from user-supplied values and prevents input from being interpreted as executable SQL syntax.

**Input handling:** Input validation should verify that search values match expected formats and lengths. Input validation improves application reliability and reduces attack surface but should not be considered a substitute for parameterized queries.

**Database permissions:** The database account should follow the principle of least privilege. Permissions should be restricted so the application can access only the data and operations required for its intended function.

**Detection and monitoring:** Logging, alerting, database activity monitoring, and a Web Application Firewall should be considered as defense-in-depth measures. These controls can help identify suspicious activity and support incident investigations.

---

## Escalation Summary

The investigation confirmed that six inputs consistent with progressive SQL injection testing were submitted through the Maplewood appointment scheduling application. The application brief states that user input is incorporated into SQL queries without parameterization, creating a SQL injection vulnerability. The database account also possesses SELECT permissions on both the appointments and patients tables, increasing potential impact if exploitation occurred. However, the access log only confirms the submitted inputs and does not prove that the database executed the injected SQL or returned patient information. Before privacy or legal reviewers can determine whether a disclosure occurred, the DBA team must review database query logs and other server-side evidence to verify execution results and determine whether any patient records were accessed or returned.

---
*CPSC 4584 | Governors State University | Fall 2026*
