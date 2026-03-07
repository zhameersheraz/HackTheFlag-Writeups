# ZIP CRACKER - Hack the Flag Writeup

**Challenge:** ZIP CRACKER  
**Category:** Forensics  
**Difficulty:** Hard  
**Points:** 400 pts  
**Flag:** `CTK{The_more_the_merrier!}`

---

## Description

Can you crack the zip file?

---

## Hints

1. john2rip

---

## Solution

### Step 1: Extract the RAR File

The downloaded challenge file was a `.rar` containing two files inside:

```bash
unrar x 1772784644_Zip_Cracking.rar
```

Output:
```
Extracting  bob.zip   OK
Extracting  README.md OK
All OK
```

Two files extracted:
- `bob.zip` — the password-protected ZIP we need to crack
- `README.md` — challenge info

### Step 2: Extract the Hash from the ZIP

The hint said **john2rip** — that's a hint to use `zip2john`, which pulls the password hash out of the ZIP file so we can crack it.

```bash
zip2john bob.zip > hash.txt
```

This saves the ZIP's password hash into `hash.txt`.

### Step 3: Crack the Hash with John the Ripper

Now use **John the Ripper** with the `rockyou.txt` wordlist to brute-force the password:

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

Output:
```
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1])
121212           (bob.zip/bob.txt)
1g 0:00:00:00 DONE
```

Password cracked: **`121212`** ✅

### Step 4: Verify the Password

```bash
john hash.txt --show
```

Output:
```
bob.zip/bob.txt:121212:bob.txt:bob.zip

1 password hash cracked, 0 left
```

### Step 5: Extract the ZIP

First I tried `unzip` — but it failed with an unsupported compression error:

```bash
unzip -P 121212 bob.zip
# skipping: bob.txt  unsupported compression method 99
```

> ⚠️ This happens because the ZIP uses **AES-256 encryption** which `unzip` doesn't support. Use `7z` instead!

So I used **7-Zip** instead:

```bash
7z x bob.zip -p121212
```

Output:
```
Everything is Ok
Size: 26
```

### Step 6: Read the Flag

```bash
cat bob.txt
```

Output:
```
CTK{The_more_the_merrier!}
```

Got the flag! 🎯

---

## Why This Works

### What is zip2john?

`zip2john` extracts the encrypted password hash from inside a ZIP file. You can't read the hash directly — it's buried in the ZIP format. This tool pulls it out in a format that John the Ripper can understand.

```
bob.zip (locked)
    |
    ▼
zip2john → hash.txt (the password in hash form)
    |
    ▼
john + rockyou.txt → 121212 (the actual password)
    |
    ▼
7z x bob.zip -p121212 → bob.txt → FLAG!
```

### Why Did unzip Fail?

Regular `unzip` only supports basic ZIP encryption. This ZIP used **AES-256** (compression method 99) — a stronger encryption that `unzip` can't handle. **7-Zip** supports AES-256, so it works perfectly.

| Tool | Supports AES-256 ZIP? |
|------|----------------------|
| `unzip` | ❌ No |
| `7z` | ✅ Yes |
| WinRAR | ✅ Yes |

### What is rockyou.txt?

`rockyou.txt` is a famous wordlist containing **14 million real passwords** leaked from the RockYou data breach in 2009. It's included in Kali Linux and is the go-to wordlist for CTF password cracking. `121212` is a common numeric pattern, so it was found instantly!

---

## Alternative Methods

### Method 1: fcrackzip (Kali)
```bash
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt bob.zip
```
Output:
```
PASSWORD FOUND!!!!: pw == 121212
```

### Method 2: hashcat
```bash
# First get the hash with zip2john
zip2john bob.zip > hash.txt

# Crack with hashcat (mode 13600 = WinZip AES)
hashcat -m 13600 hash.txt /usr/share/wordlists/rockyou.txt
```

### Method 3: Online ZIP Cracker
1. Go to 👉 https://www.lostmypass.com/file-types/zip/
2. Upload `bob.zip`
3. It tries common passwords automatically

---

## Flag

```
CTK{The_more_the_merrier!}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `unrar` | Extract the RAR to get bob.zip |
| `zip2john` | Pull the password hash out of the ZIP |
| `john` + rockyou.txt | Crack the hash → password is `121212` |
| `7z` | Extract the AES-256 encrypted ZIP (unzip failed!) |

---

## Key Takeaways

- `zip2john` → `john` is the standard combo for cracking password-protected ZIPs in CTFs
- Always try `7z` if `unzip` fails — AES-256 ZIPs need 7-Zip
- `rockyou.txt` is at `/usr/share/wordlists/rockyou.txt` on Kali — always try it first
- Simple numeric passwords like `121212` are in rockyou.txt and crack in under a second
- The hint **"john2rip"** was a direct clue pointing to `zip2john` + John the Ripper!
