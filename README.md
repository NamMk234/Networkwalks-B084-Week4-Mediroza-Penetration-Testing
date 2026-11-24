# Mediroza General Hospital – Penetration Testing Assessment

![Cybersecurity](https://img.shields.io/badge/Focus-Cybersecurity-blue)
![Project](https://img.shields.io/badge/Project-Web%20Application%20Pentesting-red)
![Networkwalks](https://img.shields.io/badge/Program-Networkwalks-darkblue)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

**Networkwalks Cybersecurity Internship | Week 4**

---

## Project Overview

This repository documents an authorized black-box web application penetration testing assessment conducted against the assigned Mediroza General Hospital training target as part of the Networkwalks Cybersecurity Internship (Week 4).

The assessment focused on identifying security weaknesses in the externally accessible web application — examining authentication mechanisms, discovering exposed directories and files, testing document security, and investigating the potential impact of identified vulnerabilities on patient data and organizational records.

---

## Assessment Details

| Detail | Information |
|---|---|
| Target | `https://medirozahospital.com` |
| Assessment Type | Black-Box Web Application Penetration Testing |
| Internship | Networkwalks Cybersecurity Internship |
| Week | Week 4 |
| Testing Period | 23–24 November 2026 |
| Testing Environment | Kali Linux |
| Scope | Authorized testing limited to the assigned target domain |

---

## Objectives

- Perform reconnaissance and service discovery.
- Identify accessible directories and application endpoints.
- Examine authentication and access-control weaknesses.
- Test for SQL injection and username enumeration.
- Investigate access to protected patient pathology reports.
- Examine PDF encryption strength and password protection.
- Analyze document metadata for information disclosure.
- Investigate exposed database backup files.
- Document findings and recommend appropriate remediation measures.

---

## Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Domain registration information gathering |
| Nmap | Network scanning and service enumeration |
| Gobuster | Directory and file discovery |
| SecLists / rockyou.txt | Wordlists for enumeration and password testing |
| Browser Developer Tools | Web application inspection and request analysis |
| Manual SQL injection testing | Authentication bypass and injection testing |
| qpdf | PDF structure and encryption inspection |
| pdfcrack | PDF password testing (successful) |
| John the Ripper | Attempted PDF password recovery (unsuccessful — format mismatch) |
| ExifTool | Document metadata examination |
| wget | File retrieval |
| grep / sed | Database backup examination and text extraction |

---

## Assessment Milestones

### Milestone 1 — Initial Access and Reconnaissance

Performed reconnaissance using WHOIS and Nmap. Used Gobuster with SecLists wordlists to discover application endpoints, including an `/old` directory and a staff login page. Examined the patient login form and identified SQL injection behaviour, username enumeration, and authentication bypass.

**Key evidence:**
- `Evidence/Finding1_SQLi_SyntaxError.png` — SQL injection syntax error response
- `Evidence/Finding2_UsernameEnumeration.png` — differential error messages confirming username enumeration
- `Evidence/Finding3_SuccessfulAccess_PortalReports.png` — successful authentication bypass and portal access
- `Evidence/Finding4_GobusterScan_OldDirectoryDiscovered.png` — Gobuster scan results showing exposed `/old` directory

---

### Milestone 2 — PDF Password Testing and Document Examination

Downloaded three patient pathology PDF reports from the portal. Used qpdf to inspect encryption type, then pdfcrack with the rockyou.txt wordlist to test password strength. All three reports were protected with weak, commonly used passwords and were successfully opened.

**Key evidence:**
- `Evidence/Finding5_PdfCrack_Report1.png` — pdfcrack output for patient report 1
- `Evidence/Finding6_PdfCrack_Reports2and3.png` — pdfcrack output for patient reports 2 and 3
- `Evidence/Finding7_OpenedReport_Thompson.png` — decrypted patient report opened

---

### Milestone 3 — Metadata Examination and Database Backup Discovery

Used ExifTool to inspect metadata on the decrypted PDF reports. The metadata contained an internal staff comment referencing an `/old` directory backup path. Navigated to the exposed `/old/` directory and found directory listing enabled, which revealed a full SQL database backup file. Downloaded and examined the backup using grep and sed, extracting staff salary records and shareholder details.

**Key evidence:**
- `Evidence/Finding8_OldDirectory_BackupExposed.png` — `/old/` directory listing with exposed backup file
- `Evidence/Finding9_Metadata_CriticalLead.png` — ExifTool metadata output showing internal comment
- `Evidence/Finding10_Staff_TableData.png` — extracted staff records from database backup
- `Evidence/Finding11_Shareholders_TableData.png` — extracted shareholder records from database backup

---

### Milestone 4 — Penetration Testing Report

Documented all identified vulnerabilities with supporting evidence, risk ratings (using a Critical / High / Medium / Low scale), CWE and OWASP Top 10 (2021) references, and remediation recommendations. The complete technical report is linked below.

---

## Key Findings Summary

| Ref | Finding | Severity | Status |
|---|---|---|---|
| F1 | SQL Injection — Patient Login Authentication Bypass | 🔴 Critical | Exploited |
| F2 | Weak PDF Password Protection — Patient Pathology Reports | 🟠 High | Exploited |
| F3 | Exposed Database Backup and Directory Listing — `/old/` | 🔴 Critical | Exploited |
| F4 | Username Enumeration — Patient Login | 🟡 Medium | Confirmed |
| F5 | PDF Metadata Information Disclosure | 🟡 Medium | Confirmed |

---

## Key Takeaways and Remediation

- **SQL Injection:** The patient login form was vulnerable to authentication bypass via SQL injection. Remediation requires replacing dynamic SQL queries with parameterized queries or prepared statements, and enforcing strict server-side input validation.
- **Weak PDF Passwords:** All three patient pathology reports used simple, dictionary-listed passwords. Remediation requires enforcing a strong password policy for all protected documents, using randomly generated passwords of sufficient length and complexity.
- **Exposed Database Backup:** An unauthenticated SQL backup containing staff and shareholder records was retrievable from a publicly accessible directory. Remediation requires removing database backups from web-accessible directories, disabling directory listing, and storing backups in secured, access-controlled locations.
- **Username Enumeration:** Differential error messages on the login page disclosed whether a submitted username existed. Remediation requires returning generic error messages regardless of whether the username or password was incorrect.
- **Metadata Disclosure:** Internal staff comments embedded in PDF metadata disclosed a sensitive internal path. Remediation requires stripping document metadata before distributing externally accessible files.

---

## Repository Structure

```
Mediroza-Pentest/
│
├── README.md
├── Penetration-Testing-Report.md
│
└── Evidence/
    ├── Finding1_SQLi_SyntaxError.png
    ├── Finding2_UsernameEnumeration.png
    ├── Finding3_SuccessfulAccess_PortalReports.png
    ├── Finding4_GobusterScan_OldDirectoryDiscovered.png
    ├── Finding5_PdfCrack_Report1.png
    ├── Finding6_PdfCrack_Reports2and3.png
    ├── Finding7_OpenedReport_Thompson.png
    ├── Finding8_OldDirectory_BackupExposed.png
    ├── Finding9_Metadata_CriticalLead.png
    ├── Finding10_Staff_TableData.png
    └── Finding11_Shareholders_TableData.png
```

---

## Project Documentation

**[View the Complete Penetration Testing Report](Penetration-Testing-Report.md)**

The full technical report covers:
- Executive Summary and overall risk rating
- Scope, authorization and methodology
- Detailed findings with proof-of-exploitation screenshots
- CWE and OWASP Top 10 (2021) vulnerability references
- Risk ratings table
- Recommendations and remediation priorities

---

## Learning Outcomes

This assessment provided practical exposure to:

- Conducting domain reconnaissance and network service enumeration.
- Identifying exposed application endpoints through directory discovery.
- Understanding and exploiting authentication weaknesses via SQL injection.
- Performing username enumeration through differential application responses.
- Examining PDF encryption types and testing password strength.
- Analyzing document metadata for information disclosure.
- Investigating exposed database backups and extracting structured records.
- Documenting technical security findings and recommending remediation.

---

## Disclaimer

This repository documents an authorized cybersecurity training assessment conducted within the scope provided for the Networkwalks Cybersecurity Internship. All testing was performed against an assigned training target with explicit authorization.

The findings are presented for educational and security-improvement purposes. Recovered credentials, confidential medical information, and personal identifiers are not included in this public repository.

---

**Networkwalks Cybersecurity Internship — Week 4**  
*Mediroza General Hospital — Authorized Black-Box Web Application Penetration Testing*