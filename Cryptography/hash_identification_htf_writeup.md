# Hash Identification — Hack the Flag Writeup

**Challenge:** Hash Identification  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{123456}`  

---

## Description

Identify the hash type and crack it:
`8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92`

This is a very common password. Submit as `ctf{the_password}`.

---

## Hints

> First identify the hash type by its length (64 hex chars = 256 bits). Then try an online hash lookup.

---

## Solution

### Step 1: Identify the hash type by length

Count the hex characters: **64 hex chars = 256 bits → SHA-256**

| Hash Length (hex chars) | Hash Type |
|------------------------|-----------|
| 32 | MD5 |
| 40 | SHA-1 |
| **64** | **SHA-256** ← this one |
| 96 | SHA-384 |
| 128 | SHA-512 |

### Step 2: Use hash-identifier or hashid

```bash
hash-identifier
# Paste the hash → Result: SHA-256
```

Or:

```bash
hashid "8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92"
# [+] SHA-256
```

### Step 3: Look up the hash online (rainbow table)

Go to 👉 https://crackstation.net/

Paste the hash → Result: **`123456`**

### Step 4: Verify the answer

```bash
echo -n "123456" | sha256sum
# 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92  -
```

Or in Python:

```python
import hashlib
print(hashlib.sha256(b"123456").hexdigest())
# 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
```

Match confirmed ✅

---

## Flag

```
ctf{123456}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `hash-identifier` / `hashid` | Identifies hash algorithm by length/format |
| CrackStation | Online rainbow table lookup |
| `sha256sum` | Verify hash on command line |
| Python `hashlib` | Compute/verify hash in Python |

---

## Key Takeaways

- Hash length identifies the algorithm: **64 hex chars = SHA-256**
- SHA-256 is a **one-way** function — you can't reverse it mathematically
- But common passwords have pre-computed hashes stored in **rainbow tables**
- `123456` is the #1 most-used password in breach databases — never use it!
