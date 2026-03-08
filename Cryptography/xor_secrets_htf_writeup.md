# XOR Secrets - Hack the Flag Writeup

**Challenge:** XOR Secrets  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{x0r_is_r3versible}`

---

## Description

The flag was XORed with the repeating key `KEY`. The hex-encoded result is:
```
28313f303d69391a30381a2b78333c39363029293c36
```

---

## Hints

1. XOR is its own inverse. XOR the ciphertext with the same key to get the plaintext.

---

## Solution

### Step 1: Open CyberChef

Go to 👉 https://gchq.github.io/CyberChef/

### Step 2: Add the "From Hex" Recipe Step

1. In the **Operations** search box on the left, type `From Hex`
2. Drag **From Hex** into the Recipe panel
3. Leave **Delimiter** as `Auto`

### Step 3: Add the "XOR" Recipe Step

1. Search for `XOR` in Operations
2. Drag **XOR** into the Recipe (below From Hex)
3. Set the settings exactly like this:
   - **Key:** `KEY`
   - **Key encoding dropdown:** `UTF8` ← ⚠️ MUST be UTF8!
   - **Scheme:** `Standard`

> ⚠️ **The common mistake:** The dropdown next to the Key defaults to `HEX`. If you leave it as HEX, CyberChef treats `KEY` as a hex string and gives garbage output like `&?1>3g7...`. Change it to **UTF8** because our key is plain text!

### Step 4: Paste Input and Bake

In the **Input** box on the right, paste:
```
28313f303d69391a30381a2b78333c39363029293c36
```

Click **BAKE!** ✅

**Output:**
```
ctf{x0r_is_r3versible}
```

Got the flag! 🎯

---

## Why This Works — What is XOR?

**XOR (⊕) = Exclusive OR** — a bitwise operation:

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

The golden rule of XOR: **XOR is its own inverse!**

```
plaintext  XOR  key  =  ciphertext
ciphertext XOR  key  =  plaintext   ← same key, same operation, reverses it!
```

That's why decryption is identical to encryption — just XOR again with the same key.

---

## What is Repeating Key XOR?

The key `KEY` is only 3 characters, but the message is 22 characters long. The key **wraps around and repeats**:

```
Message:   c  t  f  {  x  0  r  _  i  s  _  r  3  v  e  r  s  i  b  l  e  }
Key:       K  E  Y  K  E  Y  K  E  Y  K  E  Y  K  E  Y  K  E  Y  K  E  Y  K
           ↑              ↑              ↑              ↑
        repeats every 3 positions!
```

The `From Hex` step converts the hex string to raw bytes first, then XOR does the decryption.

---

## CyberChef Settings — Correct vs Wrong

| Setting | ✅ Correct | ❌ Wrong |
|---------|-----------|---------|
| Key | `KEY` | `KEY` |
| Key encoding | **UTF8** | ~~HEX~~ |
| Output | `ctf{x0r_is_r3versible}` | `&?1>3g7...` (garbage!) |

**Rule of thumb:**
- Key is **plain text** (letters/words) → use **UTF8**
- Key is **hex digits** (like `4b4559`) → use **HEX**

---

## Step-by-Step Byte Decryption Table

Each cipher byte XOR'd with its key byte:

| # | Cipher (hex) | Key letter | Result (hex) | Character |
|---|-------------|------------|-------------|-----------|
| 0 | 0x28 | K (0x4b) | 0x63 | **c** |
| 1 | 0x31 | E (0x45) | 0x74 | **t** |
| 2 | 0x3f | Y (0x59) | 0x66 | **f** |
| 3 | 0x30 | K (0x4b) | 0x7b | **{** |
| 4 | 0x3d | E (0x45) | 0x78 | **x** |
| 5 | 0x69 | Y (0x59) | 0x30 | **0** |
| 6 | 0x39 | K (0x4b) | 0x72 | **r** |
| 7 | 0x1a | E (0x45) | 0x5f | **_** |
| 8 | 0x30 | Y (0x59) | 0x69 | **i** |
| 9 | 0x38 | K (0x4b) | 0x73 | **s** |
| 10 | 0x1a | E (0x45) | 0x5f | **_** |
| 11 | 0x2b | Y (0x59) | 0x72 | **r** |
| 12 | 0x78 | K (0x4b) | 0x33 | **3** |
| 13 | 0x33 | E (0x45) | 0x76 | **v** |
| 14 | 0x3c | Y (0x59) | 0x65 | **e** |
| 15 | 0x39 | K (0x4b) | 0x72 | **r** |
| 16 | 0x36 | E (0x45) | 0x73 | **s** |
| 17 | 0x30 | Y (0x59) | 0x69 | **i** |
| 18 | 0x29 | K (0x4b) | 0x62 | **b** |
| 19 | 0x29 | E (0x45) | 0x6c | **l** |
| 20 | 0x3c | Y (0x59) | 0x65 | **e** |
| 21 | 0x36 | K (0x4b) | 0x7d | **}** |

Reading all: **`ctf{x0r_is_r3versible}`** ✅

---

## Alternative Method: Python

```python
ciphertext_hex = '28313f303d69391a30381a2b78333c39363029293c36'
key = 'KEY'

ciphertext = bytes.fromhex(ciphertext_hex)
key_bytes = key.encode()  # UTF8 encode — same as CyberChef's UTF8 setting!

plaintext = ''
for i, byte in enumerate(ciphertext):
    key_byte = key_bytes[i % len(key_bytes)]  # repeating: K,E,Y,K,E,Y,...
    plaintext += chr(byte ^ key_byte)

print(plaintext)  # ctf{x0r_is_r3versible}
```

`key.encode()` in Python does the same thing as setting UTF8 in CyberChef — it converts the text key into raw bytes before XORing.

---

## Flag

```
ctf{x0r_is_r3versible}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef (From Hex → XOR UTF8) | Main solve method |
| Python `bytes.fromhex()` + XOR loop | Alternative method |

---

## Key Takeaways

- **XOR is its own inverse** — use the same key to both encrypt AND decrypt
- **Repeating key XOR** = short key cycles (K→E→Y→K→E→Y→...) across the whole message
- In CyberChef XOR: **always use UTF8** when the key is plain text — HEX gives garbage!
- `key.encode()` in Python = UTF8 encoding = same as CyberChef's UTF8 option
- If the key was unknown, repeating XOR is crackable via frequency analysis — but here the key was given!
