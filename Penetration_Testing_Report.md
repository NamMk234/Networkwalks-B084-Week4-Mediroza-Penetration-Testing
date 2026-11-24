# MEDIROZA GENERAL HOSPITAL
## External Web Application Penetration Testing Report

**Networkwalks Cybersecurity Internship – Week 4**

| Project detail | Information |
|---|---|
| Assessment target | `https://medirozahospital.com` |
| Assessment type | Authorized black-box web application penetration test |
| Testing period | 23–24 November 2026 |
| Assessment window | Five-day assigned testing window |
| Environment | Kali Linux and web browser |
| Report date | 24 November 2026 |
| Report status | Completed |

---

## 1. Executive Summary

This report documents an authorized black-box penetration testing assessment conducted against the assigned Mediroza General Hospital web application as part of the Networkwalks Cybersecurity Internship.

The purpose of the assessment was to identify security weaknesses in the exposed web application, evaluate authentication and access controls, examine publicly accessible files, and determine whether sensitive information could be accessed through the identified weaknesses.

The assessment identified several security issues, including SQL injection in the patient login functionality, username enumeration, access to protected pathology reports, weak PDF password protection, directory exposure, metadata information disclosure, and an exposed database backup.

The most significant finding was an SQL injection vulnerability affecting the patient login endpoint. Testing demonstrated that the weakness could be used to bypass the expected authentication process and access the reports portal.

Three PDF pathology reports were subsequently obtained and examined. Password testing with pdfcrack demonstrated that the documents were protected using weak passwords. Further examination of the PDF metadata revealed information that helped guide the investigation towards an exposed database backup.

The database backup contained staff and shareholder records, demonstrating the potential impact of the information disclosure.

Overall, the assessment demonstrated how weaknesses in authentication, document protection, directory configuration, and information handling can combine to expose sensitive information.

## 2. Assessment Objectives

The assessment aimed to:

- Identify exposed services and gather information about the target.
- Discover accessible directories and files.
- Examine the web application's authentication functionality.
- Test for SQL injection and username enumeration.
- Determine whether protected reports could be accessed.
- Examine PDF password protection and encryption settings.
- Inspect PDF metadata for information disclosure.
- Investigate the exposed database backup.
- Identify the potential impact of the findings.
- Recommend practical security improvements.

## 3. Scope and Authorization

The assessment was conducted against the assigned training target:

`https://medirozahospital.com`

The engagement was described as an authorized black-box assessment within the assigned five-day testing window. Practical testing documented in this report took place on 23 and 24 November 2026.

The assessment focused on externally accessible web application functionality, exposed services, directories, documents, and database backup information.

Testing activities included reconnaissance, service scanning, directory discovery, manual web application testing, PDF examination, and database backup analysis.

No claim is made that the assessment covered the hospital's entire internal infrastructure or source code.

## 4. Methodology

The assessment followed a structured testing process.

### 4.1 Reconnaissance and information gathering

Initial information gathering was performed to understand the target and identify externally visible services.

WHOIS was used for domain-registration information, while Nmap was used to identify exposed services and perform service/version detection.

### 4.2 Directory and file discovery

Gobuster was used to identify directories and potentially accessible files.

The discovery of an old directory provided an additional area for investigation.

### 4.3 Web application testing

The patient login functionality was examined using a web browser and developer tools. Manual testing identified SQL injection behaviour and username enumeration.

### 4.4 Document examination

The pathology PDF reports were examined using qpdf and ExifTool. pdfcrack was used for password testing, with a wordlist from SecLists, including rockyou.txt.

John the Ripper was also attempted, but it did not work successfully in this exercise. pdfcrack was therefore used for the successful password-testing process.

### 4.5 Database backup investigation

The exposed SQL backup was downloaded for examination within the assigned assessment. Command-line utilities, including grep and sed, were used to search and review relevant portions of the file.

## 5. Tools Used

| Tool | Purpose |
|---|---|
| WHOIS | Domain-registration information gathering |
| Nmap | Network scanning and service/version detection |
| Gobuster | Directory and file discovery |
| SecLists / rockyou.txt | Wordlist used during password testing |
| Browser Developer Tools | Inspection of web application behaviour |
| Manual SQL injection testing | Testing the patient login functionality |
| pdfcrack | PDF password testing and recovery |
| qpdf | PDF structure and encryption inspection |
| ExifTool | PDF metadata examination |
| wget | Downloading accessible files for examination |
| grep | Searching file contents for relevant information |
| sed | Reviewing and processing text output |
| John the Ripper | Attempted, but unsuccessful during this exercise |

## 6. Technical Findings

### Finding 1: SQL Injection in Patient Login

**Affected endpoint:** `/patient/login.php`

**Severity:** Critical

**Description:**

Testing of the patient login functionality identified SQL injection behaviour. The application did not properly handle user-supplied input before processing it in the backend database query.

Manual testing demonstrated that the weakness could be used to bypass the expected authentication process.

**Impact:**

An attacker could potentially bypass authentication and gain access to functionality intended for authenticated users. In this assessment, the weakness allowed access to the patient reports portal.

**Evidence:** `Finding1_SQLi_SyntaxError.png`

**Recommendation:**

- Use parameterized queries and prepared statements.
- Validate and safely handle all user-supplied input.
- Avoid constructing SQL queries through direct string concatenation.
- Apply least-privilege permissions to database accounts.
- Use generic authentication error messages.
- Conduct security testing after remediation.

### Finding 2: Username Enumeration

**Severity:** Medium

**Description:**

The login functionality returned responses that could help distinguish valid usernames from invalid ones.

Differences in application responses can allow an attacker to identify potentially valid accounts.

**Impact:**

Username enumeration could make password-guessing and targeted account attacks easier.

**Evidence:** `Finding2_UsernameEnumeration.png`

**Recommendation:**

- Use consistent login error messages.
- Avoid revealing whether a username exists.
- Implement rate limiting and account protection mechanisms.
- Monitor repeated authentication failures.

### Finding 3: Access to Patient Reports

**Severity:** Critical

**Description:**

Following the successful authentication bypass, the reports portal became accessible and three pathology PDF reports were obtained for examination.

This demonstrated that the application did not adequately protect access to sensitive reports after the authentication weakness was exploited.

**Impact:**

Unauthorized access to pathology reports could expose confidential medical information and affect patient privacy.

**Evidence:**

- `Finding3_SuccessfulAccess_PortalReports.png`
- Sanitized versions of the three opened-report screenshots.

**Recommendation:**

- Enforce server-side authentication and authorization checks.
- Verify that each user is permitted to access the requested document.
- Prevent direct access to sensitive files through predictable URLs.
- Store confidential documents outside publicly accessible directories.
- Log and monitor access to sensitive reports.

### Finding 4: Directory Discovery and Exposure

**Affected location:** `/old/`

**Severity:** High

**Description:**

Gobuster identified an old directory during the directory discovery stage.

Further investigation showed that the directory exposed files that should not have been publicly accessible, including a database backup.

**Impact:**

Exposed directories can reveal old application files, backups, and other information that may assist an attacker.

**Evidence:** `Finding4_GobusterScan_OldDirectoryDiscovered.png`

**Recommendation:**

- Remove unnecessary directories and files from the web root.
- Disable directory listing.
- Restrict access to administrative and backup locations.
- Review web server configuration.
- Regularly scan for unintended file exposure.

### Finding 5: Weak PDF Password Protection

**Severity:** High

**Description:**

The three pathology PDF reports were password-protected. PDF examination was performed using qpdf, and password testing was conducted using pdfcrack with a wordlist from SecLists.

The testing demonstrated that the document passwords were weak and could be recovered.

The recovered credentials are intentionally not reproduced in this report.

**Impact:**

Weak document passwords reduce the protection provided by password-protected files. If an attacker obtains a copy of a document, weak passwords may allow its contents to be accessed.

**Evidence:**

- `Finding5_PdfCrack_Report1.png`
- `Finding6_PdfCrack_Reports2and3.png`
- Sanitized PDF examination and hash evidence, where included.

**Recommendation:**

- Use strong, unique passwords for sensitive documents.
- Use modern encryption settings supported by the document format and software.
- Do not rely on weak or predictable passwords.
- Restrict access to sensitive reports independently of PDF passwords.
- Review encryption settings using appropriate PDF inspection tools.

### Finding 6: PDF Metadata Information Disclosure

**Severity:** Medium

**Description:**

ExifTool was used to examine metadata contained in the PDF documents.

The metadata examination revealed a comment containing information that provided a useful lead for further investigation into the old directory and database backup.

This demonstrated that document metadata can unintentionally disclose information about internal files or application structure.

**Impact:**

Metadata can reveal internal naming conventions, file locations, comments, software details, and other information that may assist further investigation.

**Evidence:** `Finding11_Metadata_CriticalLead.png`

**Recommendation:**

- Remove unnecessary metadata before publishing documents.
- Review document properties and comments.
- Avoid including internal paths or backup references in public documents.
- Apply document sanitization procedures before files are distributed.

### Finding 7: Exposed Database Backup

**Affected file:** `/old/mediroza_db_backup_2019.sql`

**Severity:** Critical

**Description:**

Further investigation of the old directory identified an exposed SQL database backup.

The backup was accessible through the web application and was downloaded for examination within the assigned assessment.

Review of the database contents identified staff and shareholder information.

The staff records included personal and employment-related information, while the shareholder table contained shareholder details and ownership percentages.

**Impact:**

Public exposure of a database backup can lead to significant information disclosure. Depending on the information stored, this can affect employee privacy, organizational confidentiality, and the security of the wider application.

**Evidence:**

- `Finding10_OldDirectory_BackupExposed.png`
- `Finding12_Shareholders_TableData.png`
- `Finding13_Staff_TableData.png`

**Recommendation:**

- Store database backups outside the public web root.
- Restrict backup access to authorized administrators.
- Remove unnecessary historical backups from production servers.
- Disable directory listing.
- Apply appropriate access controls to backup storage.
- Encrypt backups at rest and in transit.
- Review backup retention and deletion procedures.
- Investigate whether exposed information was accessed by unauthorized parties.

## 7. Nmap Service Discovery

Nmap service/version detection identified the following exposed services during the assessment:

| Port | Service identified |
|---:|---|
| 21 | FTP |
| 25 | SMTP |
| 26 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 143 | IMAP |
| 443 | HTTPS |
| 465 | SMTP over SSL/TLS |
| 587 | SMTP submission |
| 993 | IMAPS |
| 995 | POP3S |

One additional service response was not identified by Nmap.

**Security observations:**

- Exposed services should be reviewed to confirm that they are required.
- Unnecessary services should be disabled.
- Services should be kept updated and securely configured.
- Administrative access should be restricted.
- Mail and file-transfer services should use secure configurations.

Service discovery alone does not prove that a service is vulnerable. Further authorized testing would be required to establish specific weaknesses.

## 8. Risk Summary

| Finding | Severity |
|---|---|
| SQL injection in patient login | Critical |
| Access to patient reports | Critical |
| Username enumeration | Medium |
| Directory and backup exposure | High / Critical |
| Weak PDF password protection | High |
| PDF metadata information disclosure | Medium |
| Exposed database backup | Critical |

The severity ratings reflect the potential impact and access demonstrated during the assessment. The exposed database backup and authentication bypass present particularly serious risks because they can lead to unauthorized access to sensitive information.

## 9. Overall Recommendations

The organization should prioritize the following actions:

1. Remediate SQL injection through parameterized queries and secure database access.
2. Enforce server-side authentication and authorization checks.
3. Restrict access to patient reports and other confidential documents.
4. Replace weak document passwords with strong protection.
5. Remove exposed database backups from public directories.
6. Disable directory listing and review web server configuration.
7. Remove unnecessary metadata from documents.
8. Review username enumeration and authentication error handling.
9. Review exposed network services and disable unnecessary services.
10. Implement security logging, monitoring, and regular vulnerability assessments.
11. Review whether any sensitive information was accessed and follow applicable incident-response and data-protection procedures.

## 10. Limitations

This assessment was conducted within the assigned training scope and time window.

The results represent observations made during the testing period. They do not establish that every component of the target infrastructure was assessed.

No claim is made that remediation was implemented or independently verified during this engagement.

Service identification results should not be interpreted as proof that every discovered service was vulnerable.

## 11. Conclusion

The assessment demonstrated how multiple security weaknesses can combine to expose sensitive information.

SQL injection in the patient login functionality enabled access to the reports portal. The PDF password testing demonstrated weaknesses in document protection, while metadata examination provided additional information that helped guide the investigation towards an exposed database backup.

The database analysis demonstrated the potential consequences of storing backup files in publicly accessible locations.

This exercise provided practical experience in reconnaissance, service scanning, web application testing, directory discovery, PDF password testing, metadata analysis, database examination, and technical reporting.

It also reinforced the importance of secure authentication, proper file handling, strong document protection, and responsible management of sensitive information.

## 12. Evidence and Documentation

The assessment is supported by 13 screenshots documenting the testing process and results.

The screenshots are organized into four milestones:

- M1: Initial access and reconnaissance.
- M2: PDF password testing and document examination.
- M3: Metadata examination and backup discovery.
- M4: Database backup analysis.

Sensitive document contents, recovered passwords, personal identifiers, and confidential records should be excluded from any public version unless their disclosure has been explicitly approved.

---

**End of Report**

*Networkwalks Cybersecurity Internship – Week 4*
