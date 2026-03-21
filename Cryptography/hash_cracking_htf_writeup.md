# Hash Cracking - Hack the Flag Writeup

**Challenge:** Hash Cracking  
**Category:** Cryptography  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{password}`

---

## Description

We found a password hash: `5f4dcc3b5aa765d61d8327deb882cf99`. This is an MD5 hash of a very common password. Find the original password and submit as `ctf{the_password}`.

---

## Hints

1. This is an MD5 hash. Try searching for it on an online hash lookup database like CrackStation.

---

## Solution

### Step 1: Identify the Hash

The challenge gave us this hash:

```
5f4dcc3b5aa765d61d8327deb882cf99
```

The hint confirmed it's an **MD5** hash — a 32-character hex string.

### Step 2: Crack it with CrackStation

1. Went to 👉 https://crackstation.net
2. Pasted the hash: `5f4dcc3b5aa765d61d8327deb882cf99`
3. Completed the CAPTCHA
4. Clicked **Crack Hashes**

Result:

```
Hash                               Type   Result
5f4dcc3b5aa765d61d8327deb882cf99  md5    password ✅
```

The plaintext password is: **password**

### Step 3: Build the Flag

The challenge said to submit as `ctf{the_password}`, so:

```
ctf{password}
```

Got the flag! 🎯

---

## Why This Works — MD5 Rainbow Tables

### What is MD5?

MD5 is a one-way hashing algorithm — it converts any input into a fixed 32-character hex string. You **cannot reverse** it mathematically.

```
"password"  →  MD5  →  5f4dcc3b5aa765d61d8327deb882cf99
```

But the same input **always produces the same hash** — so attackers precompute huge databases of common words and their MD5 hashes. This is called a **rainbow table** or **lookup table**.

### How CrackStation Works

CrackStation has a massive precomputed database of billions of hashes built from dictionary words, common passwords, and wordlists. When you paste a hash, it just does a database lookup — no actual "cracking" needed. For weak passwords like `password`, it finds the result instantly.

### Visual of the Attack

```
5f4dcc3b5aa765d61d8327deb882cf99
              ↓
CrackStation lookup table
              ↓
"password" ← found in <1 second! ✅
              ↓
ctf{password}  🎯
```

---

## Alternative Methods

```bash
# Method 1: Hashcat with rockyou wordlist (Kali Linux)
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
# Result: password

# Method 2: John the Ripper
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --show --format=raw-md5 hash.txt
# Result: password

# Method 3: Python — verify the hash yourself
python3 -c "import hashlib; print(hashlib.md5(b'password').hexdigest())"
# Output: 5f4dcc3b5aa765d61d8327deb882cf99 ✅
```

---

## Real-World Impact

MD5 is **completely broken** for password storage. If a database is breached and passwords are stored as plain MD5:

```
5f4dcc3b5aa765d61d8327deb882cf99  →  "password"   (instant)
e10adc3949ba59abbe56e057f20f883e  →  "123456"      (instant)
25f9e794323b453885f5181f1b624d0b  →  "123456789"   (instant)
```

Any common password is cracked in milliseconds. Modern password storage should use **bcrypt**, **scrypt**, or **Argon2** with a unique salt per user — these are intentionally slow and resistant to rainbow tables.

---

## Flag

```
ctf{password}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CrackStation (crackstation.net) | Looked up the MD5 hash and instantly returned the plaintext: `password` |

---

## Key Takeaways

- **MD5 is not secure** for password storage — it's too fast and rainbow tables exist for common words
- CrackStation cracks common MD5 hashes instantly using precomputed lookup tables
- Always try CrackStation first for any hash challenge before breaking out hashcat
- The format `ctf{plaintext}` is a common CTF flag style — always re-read the submission instructions
- Real password hashes should use **bcrypt/scrypt/Argon2** with salting, not MD5
