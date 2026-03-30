# Timestamp Analysis — Hack the Flag Writeup

**Challenge:** Timestamp Analysis  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{t1m3st0mp_d3tect3d}`  

---

## Description

Download the forensic report. A suspicious file has timestamps where the modified date is BEFORE the created date! What anti-forensic technique was used to alter these timestamps? Identify the technique and submit the flag.

---

## Hints

> When a file's modified date is before its creation date, it means the timestamps were tampered with.

---

## Solution

### Step 1: Download and inspect the file

```bash
wget <challenge-file-url> -O timestamp_report.txt
exiftool timestamp_report.txt
```

```
File Type  : TXT
Line Count : 20
Word Count : 81
```

### Step 2: Read the report

```bash
strings timestamp_report.txt
```

### Output:

```
=== File Metadata Report ===
Filename: confidential_report.docx
File Size: 245,760 bytes
MD5: a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4

=== Timestamps (NTFS) ===
Created:       2024-03-15 13:37:00 UTC
Modified:      2024-01-01 00:00:00 UTC   <-- SUSPICIOUS!
Accessed:      2024-03-15 14:00:00 UTC
MFT Modified:  2024-03-15 13:37:05 UTC

=== Analysis ===
ALERT: Modified timestamp is BEFORE the Created timestamp!
This is impossible under normal circumstances.
The $SI and $FN timestamps do not match.
This is a clear indicator of timestamp manipulation.

Anti-forensic technique detected: TIMESTOMPING
Flag: ctf{t1m3st0mp_d3tect3d}
```

---

## What is Timestomping?

**Timestomping** is an anti-forensic technique where an attacker **modifies a file's timestamps** to hide when malicious activity occurred or make a file appear older/newer than it is.

### Why is this impossible under normal circumstances?

A file's **Modified** date cannot logically be earlier than its **Created** date — you can't edit a file before it exists.

| Timestamp | Value | Notes |
|-----------|-------|-------|
| Created | 2024-03-15 13:37:00 | When the file first appeared |
| Modified | 2024-01-01 00:00:00 | **BEFORE created — impossible!** |
| MFT Modified | 2024-03-15 13:37:05 | Harder to fake — NTFS record change time |

The mismatch between `$STANDARD_INFORMATION` ($SI) and `$FILE_NAME` ($FN) timestamps in NTFS is the dead giveaway. Tools like **Volatility**, **Autopsy**, and **Plaso** detect this automatically.

---

## Flag

```
ctf{t1m3st0mp_d3tect3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `strings` | Read the text report |
| `exiftool` | Inspect file metadata |
| Autopsy / Volatility | Real-world NTFS timestamp analysis |

---

## Key Takeaways

- **Timestomping** = deliberately altering file timestamps to mislead investigators
- A Modified date **before** a Created date is physically impossible under normal use
- NTFS stores timestamps in two places ($SI and $FN); attackers often only change $SI
- This is a core concept in **Digital Forensics & Incident Response (DFIR)**
