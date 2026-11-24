# Networkwalks-B084-Week4-Mediroza-Penetration-TestingWeek 4 cybersecurity internship project documenting an authorized web application penetration testing assessment, findings, tools, screenshots, and recommendations.

# Mediroza General Hospital – Penetration Testing Assessment

![Cybersecurity](https://img.shields.io/badge/Focus-Cybersecurity-blue)
![Project](https://img.shields.io/badge/Project-Web%20Application%20Pentesting-red)
![Networkwalks](https://img.shields.io/badge/Program-Networkwalks-darkblue)
![Status](https://img.shields.io/badge/Status-Completed-success)

## Project Overview

This repository documents my Week 4 practical cybersecurity internship assessment with Networkwalks.

The project involved an authorized black-box penetration testing assessment of the assigned Mediroza General Hospital web application.

The assessment focused on identifying authentication weaknesses, exposed directories, document security issues, metadata information disclosure, and database backup exposure.

**Assessment period:** 23–24 November 2026  
**Assessment type:** Authorized black-box penetration testing  
**Environment:** Kali Linux and web browser

## Objectives

- Perform reconnaissance and service discovery.
- Identify accessible directories and files.
- Test authentication and login functionality.
- Investigate SQL injection and username enumeration.
- Examine PDF password protection.
- Inspect document metadata.
- Investigate exposed database backup files.
- Document findings and recommend remediation.

## Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Domain information gathering |
| Nmap | Service and version detection |
| Gobuster | Directory discovery |
| SecLists / rockyou.txt | Password-testing wordlist |
| Browser Developer Tools | Web application inspection |
| Manual SQLi testing | Authentication testing |
| pdfcrack | PDF password testing |
| qpdf | PDF inspection |
| ExifTool | Metadata examination |
| wget | File retrieval |
| grep and sed | Text searching and examination |

John the Ripper was attempted but did not work successfully during the exercise. pdfcrack was used for the successful PDF password testing.

## Assessment Milestones and Evidence

### M1 – Initial Access and Reconnaissance

Testing identified SQL injection behaviour, username enumeration, access to the reports portal, and an old directory.

**Screenshots:**

1. ![SQL injection syntax error](screenshots/Finding1_SQLi_SyntaxError.png)

2. ![Username enumeration](screenshots/Finding2_UsernameEnumeration.png)

3. ![Reports portal access](screenshots/Finding3_SuccessfulAccess_PortalReports.png)

4. ![Gobuster directory discovery](screenshots/Finding4_GobusterScan_OldDirectoryDiscovered.png)

### M2 – PDF Password Testing and Document Examination

The three PDF reports were examined, and pdfcrack was used to test their password strength.

**Screenshots:**

5. ![PDF password testing report one](screenshots/Finding5_PdfCrack_Report1.png)

6. ![PDF password testing reports two and three](screenshots/Finding6_PdfCrack_Reports2and3.png)

7. ![Opened report one](screenshots/Finding7_OpenedReport_1.png)

8. ![Opened report two](screenshots/Finding8_OpenedReport_2.png)

9. ![Opened report three](screenshots/Finding9_OpenedReport_3.png)

### M3 – Metadata Examination and Backup Discovery

ExifTool was used to examine PDF metadata. The metadata provided a lead for further investigation of the old directory and exposed database backup.

**Screenshots:**

10. ![Exposed database backup](screenshots/Finding10_OldDirectory_BackupExposed.png)

11. ![Metadata investigation](screenshots/Finding11_Metadata_CriticalLead.png)

### M4 – Database Backup Analysis

The exposed SQL backup was examined, revealing staff and shareholder table information.

**Screenshots:**

12. ![Shareholder table analysis](screenshots/Finding12_Shareholders_TableData.png)

13. ![Staff table analysis](screenshots/Finding13_Staff_TableData.png)

## Key Findings

- SQL injection in the patient login functionality.
- Username enumeration.
- Unauthorized access to the patient reports portal.
- Weak PDF password protection.
- Old directory and database backup exposure.
- PDF metadata information disclosure.
- Exposure of staff and shareholder database records.

## Recommendations

- Use parameterized queries to prevent SQL injection.
- Enforce authentication and authorization checks.
- Protect sensitive reports from unauthorized access.
- Use strong document passwords and appropriate encryption.
- Remove database backups from public directories.
- Disable directory listing.
- Remove sensitive metadata from documents.
- Review exposed services and apply secure configurations.

## Learning Outcomes

This assessment helped me gain practical experience in:

- Network and service scanning.
- Web application testing.
- SQL injection identification.
- Username enumeration.
- Directory discovery.
- PDF password testing.
- Metadata examination.
- Database backup analysis.
- Technical documentation and evidence management.

## Project Documentation

- **[Detailed Penetration Testing Report](Penetration_Testing_Report.md)**
- **Screenshots:** `screenshots/`

## Disclaimer

This project documents an authorized educational cybersecurity assessment. Publicly shared evidence should exclude recovered passwords, confidential medical information, personal identifiers, and other sensitive records unless their publication has been explicitly approved.

---

**Networkwalks Cybersecurity Internship – Week 4**
