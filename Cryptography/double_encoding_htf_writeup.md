# Double Encoding - Hack the Flag Writeup

**Challenge:** Double Encoding  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{tw0_layers_dec0ded}`

---

## Description

This string has been encoded twice — first Base64, then Base64 again: `WTNSbWUzUjNNRjlzWVhsbGNuTmZaR1ZqTUdSbFpIMD0=`. Peel back the layers to find the flag.

---

## Hints

1. Decode Base64 once to get another Base64 string. Decode that again.

---

## Solution

### Step 1: First Base64 Decode

I pasted the encoded string into the terminal and decoded it once:

```bash
echo "WTNSbWUzUjNNRjlzWVhsbGNuTmZaR1ZqTUdSbFpIMD0=" | base64 -d
```

Output:
```
Y3Rme3R3MF9sYXllcnNfZGVjMGRlZH0=
```

Still looks like Base64 — the `=` padding at the end is a dead giveaway. Time to decode again!

### Step 2: Second Base64 Decode

```bash
echo "Y3Rme3R3MF9sYXllcnNfZGVjMGRlZH0=" | base64 -d
```

Output:
```
ctf{tw0_layers_dec0ded}
```

Got the flag! 🎯

---

## Why This Works — Double Base64 Encoding

### What is Base64?

Base64 is an encoding scheme (NOT encryption) that converts binary data into a text-safe format using 64 characters (A-Z, a-z, 0-9, +, /). It's commonly used to transmit data over text-based protocols.

```
Original text  →  Base64 encode  →  encoded string
encoded string →  Base64 encode  →  double encoded string
```

Since Base64 output is still valid text, you can Base64 encode it again — creating a double-encoded string. To decode it you just reverse the process twice.

### Visual of the Decode

```
WTNSbWUzUjNNRjlzWVhsbGNuTmZaR1ZqTUdSbFpIMD0=
                    ↓ base64 -d (Layer 1)
Y3Rme3R3MF9sYXllcnNfZGVjMGRlZH0=
                    ↓ base64 -d (Layer 2)
ctf{tw0_layers_dec0ded}  🎯
```

### How to Spot Base64

- Characters: A-Z, a-z, 0-9, +, /
- Ends with `=` or `==` padding
- Length is always a multiple of 4
- Typically 33% longer than the original data

---

## Commands Used

```bash
# Layer 1 decode
echo "WTNSbWUzUjNNRjlzWVhsbGNuTmZaR1ZqTUdSbFpIMD0=" | base64 -d
# Output: Y3Rme3R3MF9sYXllcnNfZGVjMGRlZH0=

# Layer 2 decode
echo "Y3Rme3R3MF9sYXllcnNfZGVjMGRlZH0=" | base64 -d
# Output: ctf{tw0_layers_dec0ded}

# One-liner — pipe both decodes together
echo "WTNSbWUzUjNNRjlzWVhsbGNuTmZaR1ZqTUdSbFpIMD0=" | base64 -d | base64 -d
# Output: ctf{tw0_layers_dec0ded}
```

---

## Alternative Methods

```python
# Python solution
import base64

encoded = "WTNSbWUzUjNNRjlzWVhsbGNuTmZaR1ZqTUdSbFpIMD0="

layer1 = base64.b64decode(encoded).decode()
print("Layer 1:", layer1)

layer2 = base64.b64decode(layer1).decode()
print("Layer 2:", layer2)
# Output: ctf{tw0_layers_dec0ded}
```

```bash
# CyberChef method
# Go to https://gchq.github.io/CyberChef/
# Add "From Base64" recipe twice → paste the string → Bake!
```

---

## Flag

```
ctf{tw0_layers_dec0ded}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `base64 -d` (Kali terminal) | Decoded the first layer to reveal another Base64 string, then decoded again to get the flag |

---

## Key Takeaways

- **Base64 is encoding, not encryption** — it's trivially reversible
- Double encoding just means decoding **twice** — pipe two `base64 -d` commands together
- The `=` padding at the end is the tell-tale sign of Base64
- Always try `base64 -d` when you see a long string of random-looking alphanumeric characters in CTFs
- CyberChef's "Magic" operation can auto-detect and peel multiple encoding layers automatically
- The one-liner `| base64 -d | base64 -d` handles any number of layers cleanly
