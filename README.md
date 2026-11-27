# 🏥 Mediroza General Hospital — Web Application Penetration Testing

**Networkwalks Cybersecurity Internship · Batch B084 · Week 4**

> Black-box web application assessment conducted for the authorised educational exercise. Target: `https://medirozahospital.com`.

## Project details

| Item | Details |
|---|---|
| Target | `https://medirozahospital.com` |
| Assessment | Black-box web application penetration test |
| Tester | Shanice Williams |
| Programme | Networkwalks Cybersecurity Internship |
| Batch / Week | B084 / Week 4 |

## Executive summary

The assessment identified information disclosure and exposure of sensitive application and database files. Three encrypted patient-report PDFs were retrieved and successfully opened after password recovery. A publicly accessible SQL backup was also discovered. Its `staff` table contained 30 employee records, including job titles, departments, contact fields, national ID fields, monthly salary values in ZAR and employment dates. Its `shareholders` table contained 10 records with shareholder names, ownership percentages, number of shares and share classes.

## Scope and rules

- **In scope:** `https://medirozahospital.com` and the assignment activities explicitly authorised for this target.
- **Out of scope:** social engineering, denial-of-service testing, and activity outside the authorised target.
- Evidence is limited to the activities and results documented below. Tool version checks are not represented as completed scans.

## Milestones completed

### Milestone 1 — Initial access / patient report retrieval

Three encrypted PDF lab reports were retrieved: `patient_report_1.pdf`, `patient_report_2.pdf`, and `patient_report_3.pdf`. The files were transferred from Kali Linux to the Windows shared folder. See [A01–A03](evidence/).

### Milestone 2 — PDF password recovery

Hashes were extracted from the encrypted PDFs using Networkwalks Hash Calculator. Password recovery was completed using Networkwalks Password Cracker, and all three PDFs were opened successfully. See [A04–A09](evidence/).

### Milestone 3 — Critical data exposure

The publicly accessible SQL backup `mediroza_db_backup_2019.sql` was discovered under `/old/`. The backup included: **30 staff records** with monthly salary values and related employee fields, and **10 shareholder records** with share percentage, shares held and share class. The supplied screenshots document both required tasks. See [D01 — employee salaries](evidence/D01-hospital-employee-salaries.png) and [D02 — shareholder details](evidence/D02-hospital-shareholder-details.png).

### Milestone 4 — Reporting

This repository documents the assessment, evidence, impact and recommended remediation.

## Findings and risk summary

| ID | Finding | Severity | Evidence |
|---|---|---:|---|
| F01 | Information disclosure through `robots.txt` | Medium | [B01](evidence/B01-robots-txt-disclosure.png) |
| F02 | Publicly accessible directory listings and application paths | High | [B03](evidence/B03-old-directory-sql-backup.png), [B04](evidence/B04-patient-directory-listing.png), [B05](evidence/B05-staff-directory-listing.png) |
| F03 | Patient report PDF files exposed | High | [A03](evidence/A03-windows-patient-pdf-files.png) |
| F04 | Weak password protection on PDF reports | High | [A04–A09](evidence/) |
| F05 | Publicly accessible SQL database backup exposing staff and shareholder data | Critical | [B03](evidence/B03-old-directory-sql-backup.png), [D01](evidence/D01-hospital-employee-salaries.png), [D02](evidence/D02-hospital-shareholder-details.png) |

Severity ratings are assessment judgements based on the demonstrated exposure and potential confidentiality impact.

## Impact

The exposed backup disclosed employee compensation and personal/contact data, as well as shareholder ownership information. The PDF exposure created an additional confidentiality risk because the files could be retrieved and their password protection was defeated in the exercise.

## Recommendations

1. Remove SQL backups, reports and other sensitive files from public web directories.
2. Disable directory listing at the web-server level.
3. Restrict access to patient and staff portals and enforce server-side authorisation on every request.
4. Use strong, unique encryption passwords for exported documents and manage keys securely.
5. Review access logs and determine whether exposed records were accessed by unauthorised parties.
6. Rotate credentials or secrets that may have been included in exposed backups.
7. Establish a secure backup process that stores backups outside the web root with least-privilege access.

## Evidence index

All screenshots are stored in the [`evidence/`](evidence/) folder. Select a filename to open an individual screenshot.

### Appendix A — Patient PDF retrieval and password recovery

| File | Description |
|---|---|
| [A01-virtualbox-shared-folder-configuration.png](evidence/A01-virtualbox-shared-folder-configuration.png) | VirtualBox shared-folder configuration showing the Windows folder configured for file exchange with Kali Linux. |
| [A02-kali-linux-file-transfer.png](evidence/A02-kali-linux-file-transfer.png) | Kali Linux terminal showing the shared-folder location used during file transfer. |
| [A03-windows-patient-pdf-files.png](evidence/A03-windows-patient-pdf-files.png) | Windows folder showing the three patient PDF report files transferred from the virtual machine. |
| [A04-pdf-hash-extraction.png](evidence/A04-pdf-hash-extraction.png) | Networkwalks Hash Calculator indicating that the PDF was encrypted and a hash was extracted. |
| [A05-pdf-password-recovery.png](evidence/A05-pdf-password-recovery.png) | Networkwalks Password Cracker showing a successful password recovery result. |
| [A06-pathology-report-1-opened.png](evidence/A06-pathology-report-1-opened.png) | First pathology report opened successfully. |
| [A07-second-password-recovery.png](evidence/A07-second-password-recovery.png) | Networkwalks Password Cracker showing another successful password recovery result. |
| [A08-pathology-report-2-opened.png](evidence/A08-pathology-report-2-opened.png) | Second pathology report opened successfully. |
| [A09-pathology-report-3-opened.png](evidence/A09-pathology-report-3-opened.png) | Third pathology report opened successfully. |

### Appendix B — Reconnaissance and database backup discovery

| File | Description |
|---|---|
| [B01-robots-txt-disclosure.png](evidence/B01-robots-txt-disclosure.png) | robots.txt output identifying /patient/, /staff/, and /old/, and referencing the sitemap. |
| [B02-sitemap-discovery.png](evidence/B02-sitemap-discovery.png) | sitemap.xml output listing the public index, about, doctors, and contact pages. |
| [B03-old-directory-sql-backup.png](evidence/B03-old-directory-sql-backup.png) | Publicly accessible /old/ directory listing exposing mediroza_db_backup_2019.sql. |
| [B04-patient-directory-listing.png](evidence/B04-patient-directory-listing.png) | /patient/ directory listing showing reports and application-related files. |
| [B05-staff-directory-listing.png](evidence/B05-staff-directory-listing.png) | /staff/ directory listing exposing the staff login page. |

### Appendix C — Additional reconnaissance and application inspection

| File | Description |
|---|---|
| [C01-whois-command-attempt.png](evidence/C01-whois-command-attempt.png) | WHOIS command attempt in Kali Linux; the command shown did not return WHOIS data. |
| [C02-whois-domain-nameservers.png](evidence/C02-whois-domain-nameservers.png) | WHOIS lookup output showing domain registration and nameserver information. |
| [C03-curl-http-response-headers.png](evidence/C03-curl-http-response-headers.png) | cURL HTTP response headers showing HTTP 200 and LiteSpeed server information. |
| [C04-dns-resolution-nslookup.png](evidence/C04-dns-resolution-nslookup.png) | NSLookup result resolving medirozahospital.com to 199.188.201.16. |
| [C05-wafw00f-version-check.png](evidence/C05-wafw00f-version-check.png) | WAFW00F version check; confirms tool version, not a completed WAF detection scan. |
| [C06-dnsrecon-version-check.png](evidence/C06-dnsrecon-version-check.png) | DNSRecon version check; confirms installed version, not completed DNS enumeration. |
| [C07-patient-login-invalid-username.png](evidence/C07-patient-login-invalid-username.png) | Patient login page displaying an invalid-username response during application inspection. |
| [C08-staff-login-invalid-credentials.png](evidence/C08-staff-login-invalid-credentials.png) | Staff login page displaying its generic invalid-credentials response. |
| [C10-patient-login-page-inspection.png](evidence/C10-patient-login-page-inspection.png) | Patient login page captured during application inspection. |

### Appendix D — Milestone 3 confidential data exposure

| File | Description |
|---|---|
| [D01-hospital-employee-salaries.png](evidence/D01-hospital-employee-salaries.png) | SQL backup staff table showing employee records and monthly salary values. |
| [D02-hospital-shareholder-details.png](evidence/D02-hospital-shareholder-details.png) | SQL backup shareholders table showing shareholder names, ownership percentages, shares held and share classes. |

## Conclusion

The assessment demonstrated significant confidentiality weaknesses, particularly the publicly accessible SQL backup and the exposure of patient report documents. The critical backup exposure should be prioritised for remediation, followed by access-control and document-protection improvements.

---

**Educational use:** This repository records the authorised Networkwalks B084 Week 4 exercise. Do not reuse testing techniques against systems without explicit written permission.
