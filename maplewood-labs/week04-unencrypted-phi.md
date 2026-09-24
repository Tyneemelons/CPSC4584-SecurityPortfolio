# Week 4: Unencrypted Patient Records on a Shared Drive

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** September 24, 2026  
**Analyst:** Tyree Anderson
**Audit ID:** AUD-2026-0921-001

---

## Incident Summary

An internal audit identified archived patient records containing PHI stored in plaintext on a shared drive. Around 8,247 patient records were within the scope of the exposure, and broad access permissions increased the risk of unauthorized access or modification.

---

## HIPAA Compliance Assessment

| Requirement | Status | Finding |
|-------------|----------|----------|
| Encryption at Rest | REQUIRES REVIEW | HIPAA treats encryption as an addressable implementation specification. Archived PHI was stored in plaintext, creating additional risk if unauthorized users gained access to the shared drive. |
| Access Controls | CONTROL FAILURE | Read and write permissions were overly broad and allowed access beyond what would be expected under least-privilege principles. |
| Audit Controls | CONTROL FAILURE | Available logging wasn't sufficient enough to determine historical access activity, limiting the organization's ability to reconstruct prior events. |

---

## Cryptographic Controls Evaluated

**Base64 encoding:** Not encryption and provides no meaningful confidentiality. Base64 only changes how data is represented and can be easily decoded without a secret key.

**Caesar cipher:** A weak classical cipher that can be defeated with modern tools and simple frequency analysis. It does not provide adequate protection for sensitive healthcare information.

**Modern encryption at rest:** Maplewood should evaluate a recognized encryption standard such as AES. Proper encryption at rest helps protect PHI by making stored data unreadable without the appropriate decryption keys and supports stronger protection of sensitive patient information.

---

## Hashing Commands Practiced

| Command | Purpose | Output Length |
|----------|----------|----------|
| `echo -n "patient records" \| sha256sum` | Demonstrated how hashing creates a repeatable integrity baseline for known data. | 64 hex characters |
| `echo -n "patient records" \| md5sum` | Demonstrated why analysts compare hashing algorithms and why MD5 is considered weaker than SHA-256. | 32 hex characters |
| `sha256sum .bashrc` | Demonstrated how file hashes can be recorded to verify evidence integrity and confirm that authorized copies match the source file. | 64 hex characters |

---

## Escalation Summary

The confirmed findings include plaintext storage of archived PHI, approximately 8,247 records within scope, overly broad read and write permissions, and insufficient logging to reconstruct historical access activity. Based on the available evidence, the presence of PHI and the lack of encryption increase organizational risk. However, it remains unknown whether unauthorized access actually occurred because historical access data cannot be fully reconstructed. Leadership, privacy, security, and legal personnel should review the findings to determine breach-notification obligations, regulatory requirements, remediation priorities, and long-term security improvements.

---

*CPSC 4584 | Governors State University | Fall 2026*
