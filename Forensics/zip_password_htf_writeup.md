# ZIP Password - Hack the Flag Writeup

**Challenge:** ZIP Password  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{z1p_cr4ck3d_0p3n}`

---

## Description

Download the password-protected ZIP file. The password is a common 4-digit PIN. Crack it to find the flag inside.

---

## Hints

1. The password is a 4-digit number. Try common PINs like 1234, 0000, or use a tool like fcrackzip or John the Ripper.

---

## Solution

### Step 1: Download the ZIP File

I downloaded `locked.zip`. Opening it required a password.

### Step 2: Check What's Inside

```bash
zip -sf locked.zip
```

Output:
```
Archive contains:
  secret_flag.txt
```

There's a `secret_flag.txt` inside — that's what we need to get to.

### Step 3: Crack the Password

The hint says it's a **4-digit PIN**. There are only 10,000 possibilities (0000–9999). I used `fcrackzip` with a brute-force attack:

```bash
fcrackzip -b -c '1' -l 4-4 -u locked.zip
```

What these flags mean:
- `-b` = brute-force mode (try all combinations)
- `-c '1'` = character set: digits only (0-9)
- `-l 4-4` = length exactly 4 characters
- `-u` = unzip to verify the correct password

Output:
```
PASSWORD FOUND!!!!: pw == 0000
```

The password was **`0000`** — one of the most common PINs!

### Step 4: Extract the ZIP

```bash
unzip -P 0000 locked.zip
```

Output:
```
Archive:  locked.zip
  inflating: secret_flag.txt
```

### Step 5: Read the Flag

```bash
cat secret_flag.txt
```

Output:
```
Congratulations! You cracked the ZIP password!

Flag: ctf{z1p_cr4ck3d_0p3n}
```

Got the flag! 🎯

---

## Why `0000` Was Cracked Instantly

A 4-digit PIN has only **10,000 possible combinations** (0000 to 9999). That sounds like a lot, but computers can try thousands per second. The most common 4-digit PINs in order of popularity are:

```
1. 1234   ← most common
2. 0000   ← second most common (this challenge!)
3. 1111
4. 1212
5. 7777
6. 1004
7. 2000
8. 4444
9. 2222
10. 6969
```

`0000` was found in under a second because brute force tools try common PINs first before iterating through all 10,000.

---

## Alternative Methods

### Method 1: fcrackzip — Brute Force (Most Common)
```bash
# Digits only, 4 characters
fcrackzip -b -c '1' -l 4-4 -u locked.zip
```

### Method 2: John the Ripper (zip2john method — same as ZIP CRACKER challenge)
```bash
zip2john locked.zip > zip_hash.txt
john zip_hash.txt --mask=?d?d?d?d
# ?d = digit, so ?d?d?d?d = 4 digits
```

### Method 3: Python Brute Force Script
```python
import zipfile

zf = zipfile.ZipFile("locked.zip")
for i in range(10000):
    pin = f"{i:04d}"  # formats as 0000, 0001, 0002...
    try:
        zf.extractall(pwd=pin.encode())
        print(f"Password found: {pin}")
        break
    except:
        pass
```

### Method 4: yourpasswordiscorrect.com (Online)
1. Go to 👉 https://www.yourpasswordiscorrect.com/
2. Upload the ZIP
3. Select digit-only, 4-character brute force
4. Wait for it to crack online (no installation needed)

### Method 5: Try Common PINs Manually First
If it's a CTF, try these by hand first before running tools:
```
1234, 0000, 1111, 2580, 1337, 9999, 4321
```
`0000` and `1234` crack most CTF ZIP challenges instantly!

---

## This Challenge vs ZIP CRACKER Challenge

You've seen a similar challenge before — here's how they compare:

| Challenge | Password Type | Tool Used | Password Found |
|-----------|--------------|-----------|---------------|
| **ZIP CRACKER** | Common word (rockyou.txt) | `zip2john` + `john` | `121212` |
| **ZIP Password** | 4-digit PIN | `fcrackzip -b` | `0000` |

**Key difference:**
- Wordlist attack (`rockyou.txt`) = tries common dictionary words
- Brute force (`-b`) = tries every possible combination systematically

---

## Flag

```
ctf{z1p_cr4ck3d_0p3n}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `fcrackzip -b` | Brute-force all 4-digit number combinations |
| Python `zipfile` | Alternative brute-force script (found password = `0000`) |
| `unzip -P 0000` | Extract the ZIP with the cracked password |

---

## Key Takeaways

- **4-digit PINs have only 10,000 combinations** — computers crack these in under a second
- `fcrackzip -b -c '1' -l 4-4 -u` = brute-force all 4-digit numbers
- Always try `0000`, `1234`, `1111` manually first — they cover a huge percentage of PINs
- The difference from ZIP CRACKER: brute-force = all combos, wordlist = dictionary words
- Never use 4-digit PINs for anything important — they are trivially crackable!
