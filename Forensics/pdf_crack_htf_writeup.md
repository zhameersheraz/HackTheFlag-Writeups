# PDF CRACK - Hack the Flag Writeup

**Challenge:** PDF CRACK  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctk{Th3_Past_Doesnt_call_back!}`

---

## Description

Can you crack this PDF?

---

## Hints

1. maybe research? note: flag format is `ctk{}`

---

## Solution

### Step 1: Open the PDF — It's Password Protected!

I downloaded the file `Bank_Account.pdf` and tried to open it. A password prompt appeared:

```
This file is password protected. Please enter a password to open the file.
```

No password = can't read it. Time to crack it!

### Step 2: Extract the Hash with pdf2john

Just like cracking a ZIP file, the process is:
1. Pull the password hash out of the PDF
2. Crack the hash using a wordlist

`pdf2john` extracts the hash from the PDF:

```bash
pdf2john 1772692767_Bank_Account.pdf > pdf_hash.txt
```

This saves the PDF's password hash into `pdf_hash.txt`.

### Step 3: Crack the Hash with John the Ripper

Now use **John the Ripper** with the `rockyou.txt` wordlist:

```bash
john pdf_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

Output:
```
Loaded 1 password hash (PDF [MD5 SHA2 RC4/AES 32/64])
spongebob        (1772692767_Bank_Account.pdf)
1g 0:00:00:00 DONE
```

Password cracked: **`spongebob`** ✅

> ⚠️ **If you see "No password hashes left to crack"** — John already cracked it before and cached the result! Run `john pdf_hash.txt --show` to see the saved password. If you want to crack from scratch, delete the cache first with `rm ~/.john/john.pot`.

### Step 4: Verify the Password

```bash
john pdf_hash.txt --show
```

Output:
```
1772692767_Bank_Account.pdf:spongebob

1 password hash cracked, 0 left
```

### Step 5: Open the PDF with the Password

I opened `Bank_Account.pdf` in the browser/PDF viewer and typed `spongebob` when prompted.

The PDF opened and showed only one line of text:

```
ctk{Th3_Past_Doesnt_call_back!}
```

Got the flag! 🎯

---

## Why This Works

### How PDF Password Protection Works

When you set a password on a PDF, it encrypts the content and stores a **hash** of the password inside the file. When you enter the password, it hashes your input and checks if it matches.

The hash is publicly visible inside the PDF file — anyone can extract it! The security only comes from the password being hard to guess.

```
PDF file contains:
├── Encrypted content  ← can't read without password
└── Password hash      ← this is extractable!
       ↓
pdf2john extracts the hash
       ↓
john + rockyou.txt tries millions of passwords
       ↓
Finds "spongebob" matches the hash ✅
       ↓
Use "spongebob" to decrypt and open the PDF
```

### Why Did rockyou.txt Work?

`rockyou.txt` contains **14 million real passwords** leaked from a 2009 data breach. `spongebob` is a common word that appears in the list — so it was cracked instantly (under 1 second)!

---

## This Challenge vs ZIP CRACKER

You might notice this is very similar to the ZIP Cracker challenge:

| Step | ZIP Cracker | PDF CRACK |
|------|-------------|-----------|
| Extract hash | `zip2john bob.zip` | `pdf2john file.pdf` |
| Crack hash | `john hash.txt --wordlist=rockyou.txt` | `john hash.txt --wordlist=rockyou.txt` |
| Open file | `7z x bob.zip -p121212` | Open PDF and enter `spongebob` |
| Password found | `121212` | `spongebob` |

The process is exactly the same — just different file types! The `john` + `rockyou.txt` combo works for ZIP, PDF, RAR, and many more!

---

## Alternative Methods

### Method 1: hashcat (Kali)
```bash
# Extract hash first
pdf2john Bank_Account.pdf > pdf_hash.txt

# Crack with hashcat (mode 10500 = PDF 1.4-1.6)
hashcat -m 10500 pdf_hash.txt /usr/share/wordlists/rockyou.txt
```

### Method 2: Online PDF Password Remover
1. Go to 👉 https://www.ilovepdf.com/unlock_pdf
2. Upload the locked PDF
3. It tries common passwords automatically
4. If cracked, downloads an unlocked version

### Method 3: pdfcrack (Kali)
```bash
pdfcrack -f Bank_Account.pdf -w /usr/share/wordlists/rockyou.txt
```
Output:
```
found user-password: 'spongebob'
```

---

## Flag

```
ctk{Th3_Past_Doesnt_call_back!}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `pdf2john` | Extract the password hash from the PDF |
| `john` + rockyou.txt | Crack the hash → password is `spongebob` |
| Browser PDF viewer | Open the PDF with the cracked password |

---

## Key Takeaways

- PDF password protection stores a **crackable hash** inside the file itself
- `pdf2john` → `john` is the same combo as `zip2john` → `john` — just for PDFs
- `rockyou.txt` cracks common passwords like `spongebob` in under a second
- The flag was **inside** the PDF — you couldn't see it without cracking the password first
- `ilovepdf.com` is a no-install online alternative if you don't have Kali
