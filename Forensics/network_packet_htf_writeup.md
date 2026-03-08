# Network Packet - Hack the Flag Writeup

**Challenge:** Network Packet  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{n3tw0rk_c4ptur3d}`

---

## Description

Download the network capture below. A captured packet shows a suspicious request with an encoded token. Decode it to find the flag.

---

## Hints

1. The token value looks like Base64 encoding. Decode it to reveal the flag.

---

## Solution

### Step 1: Read the Network Capture File

I opened `network_capture.txt` and read through the packets. Most were normal HTTP traffic — but **Packet #3** looked suspicious:

```
--- Packet #3 ---
TCP 192.168.1.100:44833 -> 10.0.0.5:80
GET /secret?token=Y3Rme24zdHcwcmtfYzRwdHVyM2R9 HTTP/1.1
Host: internal-server.local
Cookie: session=abc123
```

There's a URL parameter:
```
token=Y3Rme24zdHcwcmtfYzRwdHVyM2R9
```

That long string after `token=` looks like **Base64** — random-looking letters and numbers, ends with `=` padding signs are a giveaway.

### Step 2: Decode the Base64 Token

**Method 1 — Kali terminal (fastest):**

```bash
echo "Y3Rme24zdHcwcmtfYzRwdHVyM2R9" | base64 -d
```

Output:
```
ctf{n3tw0rk_c4ptur3d}
```

Got the flag! 🎯

---

## Understanding the Network Capture File

The file was a **plain text packet capture** — a simplified log of network traffic showing each packet's source, destination, and content.

Here's what each packet meant:

| Packet | What happened |
|--------|--------------|
| #1 | Client `192.168.1.100` requests a normal webpage from `10.0.0.5` |
| #2 | Server responds with 200 OK |
| **#3** | **Client requests `/secret?token=<encoded>` — suspicious!** |
| #4 | Server responds with 200 OK |
| #5 | Client starts a TLS (encrypted HTTPS) connection |

Packet #3 is the odd one — why is it requesting `/secret` and sending a token in the URL? That's what we investigate!

---

## How to Spot Base64

Base64 is a very recognizable encoding. The giveaways are:

```
Y3Rme24zdHcwcmtfYzRwdHVyM2R9
  ↑
• Only uses A-Z, a-z, 0-9, +, /
• Often ends with = or == padding
• Length is always a multiple of 4
• Looks like random letters/numbers but structured
```

Compared to other encodings:
| Encoding | Looks like | Example |
|----------|-----------|---------|
| Base64 | Mix of upper/lower/numbers, ends `=` | `Y3Rme24zdHcwcmtf...` |
| Hex | Only 0-9 and a-f | `63746600...` |
| Binary | Only 0s and 1s | `01100011 01110100...` |
| ROT13 | Shifted letters | `pgsjah...` |

When you see a long string in a CTF that looks like Base64, **always try decoding it first!**

---

## What Actually Happened in the Capture?

Someone was sending a secret token hidden in a URL parameter using Base64 encoding. Normally, sensitive tokens should **never** be in a URL because:

1. URLs appear in **server logs** — anyone with log access sees the token
2. URLs appear in **browser history** — anyone with device access sees the token
3. URLs are sent in **HTTP Referer headers** — third-party sites can see it

The attacker/developer tried to hide the flag by encoding it in Base64, but Base64 is **not encryption** — it's just encoding. Anyone who captures this packet can instantly decode it!

---

## Alternative Methods

### Method 1: CyberChef (Online)
1. Go to 👉 https://gchq.github.io/CyberChef/
2. Search **"From Base64"** and drag to Recipe
3. Paste: `Y3Rme24zdHcwcmtfYzRwdHVyM2R9`
4. Result: `ctf{n3tw0rk_c4ptur3d}` ✅

### Method 2: Python
```python
import base64
token = "Y3Rme24zdHcwcmtfYzRwdHVyM2R9"
print(base64.b64decode(token).decode())
```
Output: `ctf{n3tw0rk_c4ptur3d}`

### Method 3: Kali Terminal
```bash
echo "Y3Rme24zdHcwcmtfYzRwdHVyM2R9" | base64 -d
```

### Method 4: Real Wireshark (for real .pcap files)
In actual CTF challenges, packet captures are usually `.pcap` or `.pcapng` files opened in **Wireshark**:
1. Open Wireshark → File → Open → load `.pcap`
2. Filter: `http` to show only HTTP traffic
3. Look for unusual GET requests with encoded parameters
4. Right-click → Follow → HTTP Stream to see the full request
5. Copy the Base64 token and decode it

---

## Flag

```
ctf{n3tw0rk_c4ptur3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Read the `.txt` file | Found suspicious `token=` parameter in Packet #3 |
| `echo "..." \| base64 -d` | Decoded the Base64 token to get the flag |

---

## Key Takeaways

- In packet captures, look for **encoded values** in URL parameters, cookies, and headers
- **Base64 giveaways**: mixed upper/lowercase letters + numbers, ends in `=`, looks structured
- `echo "..." | base64 -d` is the fastest way to decode Base64 on Kali
- Base64 is **NOT encryption** — it's just encoding! Anyone can decode it instantly
- Real packet captures use Wireshark (`.pcap` files) — filter by `http` to find suspicious requests
- Sensitive tokens should **never** be in URLs — use POST body or encrypted headers instead
