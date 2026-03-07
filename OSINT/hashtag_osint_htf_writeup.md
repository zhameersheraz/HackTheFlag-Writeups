# Hashtag_osint - Hack the Flag Writeup

**Challenge:** Hashtag_osint  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 250 pts  
**Flag:** `ctf{You_GoT_m3_!}`

---

## Description

Find and determine encrypted HTTP "GET" Method token url query value to Seize the Throne?

---

## Hints

1. Look closely at the HTTP GET requests in the logs. One of them contains a suspicious query parameter that doesn't look like a normal timestamp or ID.

---

## Solution

### Step 1: Download and Open the Log File

I downloaded the challenge file — a web server log file containing HTTP request records from a CTF server.

### Step 2: Scan the GET Requests for Suspicious Parameters

Most query parameters in the logs looked like normal timestamps or IDs:

```
?_=1634873048440   ← normal Unix timestamp
?_=1634873048441   ← normal Unix timestamp
?_=1634873056706   ← normal Unix timestamp
?_=1634873061293   ← normal Unix timestamp
```

But one line stood out immediately:

```
GET /plugins/challenges/assets/view.js?_=Y3Rre1lvdV9Hb1RfbTNfIX0
```

The value `Y3Rre1lvdV9Hb1RfbTNfIX0` doesn't look like a timestamp — it's much longer and uses letters mixed with numbers. That's a **Base64 string**! 🔍

### Step 3: Decode the Base64 String (My Method)

I used the `base64` command on Kali Linux to decode it:

```bash
echo "Y3Rre1lvdV9Hb1RfbTNfIX0=" | base64 -d
```

Output:
```
ctf{You_GoT_m3_!}
```

Got the flag! 🎯

---

## Why This Works

### What is Base64?

Base64 is an encoding scheme that converts binary or text data into a string of letters, numbers, `+`, and `/`. It's commonly used to safely transmit data in URLs, emails, and APIs.

**How to spot Base64:**
- Only uses characters: `A-Z`, `a-z`, `0-9`, `+`, `/`, `=`
- Often ends with `=` or `==` (padding)
- Length is always a multiple of 4
- Looks "random" but not hex (no spaces, no special chars)

```
Y3Rre1lvdV9Hb1RfbTNfIX0=
↑
Letters + numbers, no spaces = likely Base64!
```

### Why Was It Hidden in a URL Parameter?

The attacker (or challenge creator) encoded the flag as Base64 and hid it inside a `_=` query parameter — a common cache-busting parameter that's easy to overlook in long log files. Most analysts would skip past it thinking it's just another timestamp!

### The Suspicious Line vs Normal Lines

```
Normal:    ?_=1634873048440        ← 13 digits, pure numbers = Unix timestamp
Malicious: ?_=Y3Rre1lvdV9Hb1RfbTNfIX0  ← letters + numbers = Base64!
```

---

## Alternative Methods

### Method 1: base64decode.org (Online, Easiest)
1. Go to 👉 https://www.base64decode.org
2. Paste: `Y3Rre1lvdV9Hb1RfbTNfIX0=`
3. Click **Decode**
4. Result: `ctf{You_GoT_m3_!}`

### Method 2: CyberChef
1. Go to 👉 https://gchq.github.io/CyberChef/
2. Search for **"From Base64"** and drag it into the Recipe
3. Paste `Y3Rre1lvdV9Hb1RfbTNfIX0=` in the Input
4. Click **BAKE!**
5. Output: `ctf{You_GoT_m3_!}`

### Method 3: grep + base64 on Kali (One-liner)
```bash
# First find the suspicious parameter
grep -oP '_=\K[A-Za-z0-9+/=]{20,}' access.log

# Then decode it
grep -oP '_=\K[A-Za-z0-9+/=]{20,}' access.log | base64 -d
```

Output:
```
ctf{You_GoT_m3_!}
```

### Method 4: Python
```python
import base64

encoded = "Y3Rre1lvdV9Hb1RfbTNfIX0="
decoded = base64.b64decode(encoded).decode()
print(decoded)
```

Output:
```
ctf{You_GoT_m3_!}
```

---

## Flag

```
ctf{You_GoT_m3_!}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Manual log analysis | Spotted the odd query parameter among normal timestamps |
| `base64 -d` (Kali Linux) | Decoded the Base64 string in terminal |
| base64decode.org | Online Base64 decoder |
| CyberChef | Alternative decode using "From Base64" recipe |

---

## Key Takeaways

- Always scan log files for query parameters that **don't match the expected format** (e.g. letters where timestamps should be)
- Base64 strings are recognizable: letters + numbers, often ending in `=`, no spaces
- `echo "string" | base64 -d` is the fastest way to decode Base64 on Linux
- Attackers and CTF creators hide data in plain sight — URL parameters, headers, and comments are all fair game
- In real investigations, log files are gold — they record every request, including ones with hidden encoded payloads
