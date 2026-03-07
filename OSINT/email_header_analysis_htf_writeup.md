# Email Header Analysis - Hack the Flag Writeup

**Challenge:** Email Header Analysis  
**Category:** OSINT  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{3m41l_h34d3r_4n4lys1s}`

---

## Description

Download the email file below. Analyze the email headers to find a flag hidden within the custom headers. Not all important information is in the email body.

---

## Hints

1. Email headers contain metadata about the message path. The X-Mailer header identifies the sending software.

---

## Solution

### Step 1: Download the Challenge File

Download the provided email file from the challenge page.

### Step 2: Open and Read the File (My Method)

I opened the raw email file and read through all the headers. The hint mentioned the **X-Mailer** header, so I scanned for it:

```
Delivered-To: analyst@security-team.local
Received: from mail.evil.com (192.168.1.100)
From: "John Smith" <john@legitimate-company.com>
To: victim@company.com
Subject: Urgent: Account Verification Required
X-Mailer: ctf{3m41l_h34d3r_4n4lys1s}   ← FLAG IS HERE!
X-Originating-IP: 192.168.1.100
X-Spam-Status: Yes, score=8.5
Return-Path: <bounces@evil.com>
```

The flag was hiding in the `X-Mailer` field — a custom header that normally identifies what email software sent the message.

Got the flag! 🎯

### Step 3: On Kali Linux (Quick Method)

You can also use `grep` to find it instantly without reading the whole file:

```bash
grep "X-Mailer" email.eml
```

Output:
```
X-Mailer: ctf{3m41l_h34d3r_4n4lys1s}
```

Or search for the flag format directly:
```bash
grep "ctf{" email.eml
```

Output:
```
X-Mailer: ctf{3m41l_h34d3r_4n4lys1s}
```

---

## Why This Works

### What are Email Headers?

Email headers are metadata automatically added to every email. They record the full journey of the message — who sent it, where it went, what software handled it, and more. Most email apps hide them by default, but they're always there in the raw file.

Common email headers:

| Header | What it means |
|--------|--------------|
| `From` | Who sent the email |
| `To` | Who received it |
| `Received` | Every server the email passed through |
| `X-Mailer` | What software sent the email |
| `X-Originating-IP` | The original sender's IP address |
| `Return-Path` | Where bounced emails go |
| `X-Spam-Status` | Spam score assigned by mail filters |

Headers starting with **`X-`** are **custom headers** — anyone can add their own. In this challenge, the CTF creator hid the flag inside `X-Mailer`.

### Bonus: This Email is a Phishing Email!

Looking at the headers reveals several red flags:

```
From:    john@legitimate-company.com   ← Claims to be legitimate
Sent by: mail.evil.com (192.168.1.100) ← Actually sent from evil.com!
Subject: Urgent: Account Verification  ← Classic phishing tactic
X-Spam-Status: Yes, score=8.5         ← Spam filters already flagged it
Return-Path: bounces@evil.com         ← Bounces go to evil.com, not the sender
```

The body also contains a phishing link to `http://evil-phishing-site.com/login`. A real-world analyst would use exactly this process to investigate suspicious emails!

---

## Alternative Methods

### Method 1: MXToolbox Email Header Analyzer
1. Go to 👉 https://mxtoolbox.com/EmailHeaders.aspx
2. Paste the full email content into the box
3. Click **Analyze Header**
4. Scroll through the parsed headers and find `X-Mailer`

### Method 2: Google Admin Toolbox
1. Go to 👉 https://toolbox.googleapps.com/apps/messageheader/
2. Paste the raw email headers
3. Click **Analyze**
4. Find the `X-Mailer` field in the results

### Method 3: Mail Header Analyzer (mha.danno.no)
1. Go to 👉 https://mha.danno.no
2. Paste the raw headers
3. Results show all headers in a clean, readable format

### Method 4: strings command (Kali)
```bash
strings email.eml | grep "ctf"
```

Output:
```
X-Mailer: ctf{3m41l_h34d3r_4n4lys1s}
```

---

## Flag

```
ctf{3m41l_h34d3r_4n4lys1s}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `grep` (Kali Linux) | Search for the flag directly in the file |
| MXToolbox | Online email header analyzer |
| Google Admin Toolbox | Parse and visualize email headers |
| Manual inspection | Read the raw file and scan for `X-` headers |

---

## Key Takeaways

- Email headers contain hidden metadata — always check them in OSINT challenges
- Headers starting with `X-` are **custom headers** and great hiding spots for flags
- `grep "ctf{" file.eml` is the fastest way to find a flag in a text file
- The same technique is used in real cybersecurity to **analyze phishing emails**
- Always check: `From` vs `Received` mismatch, `X-Spam-Status`, and `Return-Path` for suspicious emails
