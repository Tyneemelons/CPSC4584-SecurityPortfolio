# Week 5: Weak Password Policy Exposes Billing Portal

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** October 1, 2026  
**Analyst:** Tyree Anderson  
**Incident ID:** INC-2026-0928-001

---

## Incident Summary

A credential stuffing attack targeted the Maplewood billing portal and ultimately succeeded against the billing_admin_03 account. The attacker used a compromised and reused password to gain authenticated access and viewed insurance records for 3,247 patients, including names, dates of birth, insurance policy numbers, and claim histories.

---

## TLS Assessment

**TLS Version:** TLS 1.3  
**Status:** Compliant

**What TLS Protected:** Data in transit between the client and server. TLS 1.3 encrypted communications and protected the confidentiality and integrity of information moving across the network.

**What TLS Did Not Protect:** Authentication. The attacker successfully authenticated using a compromised credential, so TLS protected the communication channel but could not determine whether the authenticated user was legitimate.

---

## Authentication Controls Gap Analysis

| Control | Required | Status | Finding |
|---------|----------|--------|---------|
| MFA | Maplewood sensitive-account standard | Not Implemented | A password alone was sufficient for access. MFA would have required an additional authentication factor and reduced the effectiveness of the compromised password. |
| Failed Attempt Protection | Account-based throttling and alerting | Not Implemented | The system allowed 847 failed login attempts over 72 hours without throttling or generating an SOC alert. |
| Password Policy | NIST SP 800-63B-4 aligned | Needs Improvement | The password satisfied local complexity requirements but was a known compromised credential and was not screened against blocklists. |
| Compromised Credential Response | Detect and invalidate confirmed compromised authenticators | Not Implemented | The credential appeared in a known breach, but no process identified the exposure or forced a password change. |
| Automated Attack Controls | Throttling, bot detection, or adaptive controls as appropriate | Not Implemented | The distributed attack originated from 12 IP addresses and was not slowed, challenged, or detected by automated controls. |

---

## OpenSSL Commands Practiced

| Command | Purpose |
|---------|---------|
| openssl genrsa -out private_key.pem 2048 | Generated a new 2048-bit RSA private key and saved it as private_key.pem. |
| openssl rsa -in private_key.pem -pubout | Derived the corresponding public key from the private key and output it in PEM format. |
| openssl rsa -in private_key.pem -text -noout | Displayed the RSA key structure, including modulus information and other key details for inspection. |
| cat public_key.pem | Displayed the PEM-formatted public key file contents. |

---

## Escalation Summary

**Confirmed Findings:** The billing_admin_03 account was successfully accessed using a compromised password that had previously appeared in a leaked credential set. Prior to the successful login, 847 failed authentication attempts were observed across 12 source IP addresses during a 72-hour period. The attacker maintained access for 47 minutes and viewed insurance records belonging to 3,247 patients. The billing portal successfully established TLS 1.3, and there is no evidence that TLS was compromised.

**Authentication Control Gaps:** MFA was not implemented, compromised-password screening was not performed, failed-attempt protections were absent, automated attack controls were not implemented, and no established process existed to invalidate confirmed compromised credentials.

**Questions Requiring Leadership Review:** Determine notification and regulatory obligations, assess the full scope of patient impact, evaluate implementation of MFA for privileged accounts, review password-screening and rate-limiting controls, and consider additional authentication monitoring capabilities.

**Tier 1 Analyst Limitation:** The analyst's role is to document evidence, preserve the timeline, and escalate findings through approved procedures. Decisions regarding account changes, policy modifications, remediation actions, and regulatory responses require authorized leadership and change-management approval.

---

*CPSC 4584 | Governors State University | Fall 2026*
