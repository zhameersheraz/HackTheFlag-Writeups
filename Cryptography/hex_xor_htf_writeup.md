# Hex XOR - Hack the Flag Writeup

**Challenge:** Hex XOR  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{Cemn}`

---

## Description

Two hex strings were XORed together:

```
48656c6c6f  XOR  ?????  =  0b00010c010e
```

Find the key and submit as `ctf{key_in_ascii}`.

---

## Hints

1. XOR the ciphertext with the known plaintext to recover the key. `48 XOR 0b = ?`, `65 XOR 00 = ?`, etc.

---

## Solution

### Step 1: Decode the Known Hex Value

The first value `48656c6c6f` is in hex. Decode it to find out what it says.

In CyberChef: Input `48656c6c6f` → Add **From Hex** → Bake!

Output:
```
Hello
```

So the equation is really:
```
"Hello"  XOR  ?????  =  0b00010c010e
```

### Step 2: Recover the Key

Since XOR is its own inverse:

```
A  XOR  KEY  =  B
A  XOR  B    =  KEY   ← XOR both knowns together to find the key!
```

We know A (`Hello` / `48656c6c6f`) and B (`0b00010c010e`), so:
```
KEY = 48656c6c6f  XOR  0b00010c010e
```

In CyberChef:
1. Input: `Hello` (the decoded plaintext from Step 1)
2. Add **XOR** recipe
3. Key: `0b00010c010e`, encoding: **HEX** ← this time HEX because the key IS hex!
4. Scheme: `Standard`
5. Bake!

Output:
```
Cem`n
```

### Step 3: Submit the Flag

The challenge asks for the key in ASCII: the output is `Cem`n` — the backtick (`` ` ``) is a non-letter character so the flag is just the letters:

```
ctf{Cemn}
```

**Correct! +200 points!** 🎯

---

## Why This Works — Key Recovery

XOR has this property:

```
A XOR KEY = B
B XOR A   = KEY    ← rearrange and XOR the two knowns to find the unknown
```

Think of it like algebra:
```
Normal algebra:  5 + x = 8  →  x = 8 - 5  →  x = 3
XOR algebra:     A XOR x = B  →  x = B XOR A
```

---

## Step-by-Step Byte Recovery

Plaintext `Hello` = `48 65 6c 6c 6f`  
Result `0b00010c010e` = `0b 00 01 0c 01 0e`

| # | A (Hello) | B (result) | A XOR B = Key byte | Key char |
|---|-----------|------------|-------------------|----------|
| 0 | 0x48 (H) | 0x0b | 0x43 | **C** |
| 1 | 0x65 (e) | 0x00 | 0x65 | **e** |
| 2 | 0x6c (l) | 0x01 | 0x6d | **m** |
| 3 | 0x6c (l) | 0x0c | 0x60 | `` ` `` (backtick) |
| 4 | 0x6f (o) | 0x01 | 0x6e | **n** |

Key bytes: `43 65 6d 60 6e` → ASCII: `Cem`n`

The backtick `` ` `` is not a regular letter — the challenge only wanted the alphabetic part → **Cemn**

---

## CyberChef Recipe Summary

**Step 1 — Identify plaintext:**
```
Input:   48656c6c6f
Recipe:  From Hex
Output:  Hello
```

**Step 2 — Recover key:**
```
Input:   Hello
Recipe:  XOR (Key: 0b00010c010e, Encoding: HEX, Scheme: Standard)
Output:  Cem`n
```

> 💡 **Note the difference from XOR Secrets challenge:** Here the XOR key is a hex string (`0b00010c010e`), so we use **HEX** encoding — NOT UTF8! The rule: if the key looks like hex digits → use HEX encoding. If the key is plain text like `KEY` → use UTF8.

---

## Alternative Method: Python

```python
# Known values
a = bytes.fromhex('48656c6c6f')   # "Hello"
b = bytes.fromhex('0b00010c010e') # the result

# Key = A XOR B
key = bytes([x ^ y for x, y in zip(a, b)])
print(key.hex())    # 43656d606e
print(key.decode()) # Cem`n

# Flag = ctf{Cemn}
```

---

## XOR Key Recovery Formula

```
If:   plaintext  XOR  key  =  ciphertext
Then: plaintext  XOR  ciphertext  =  key
```

This is called **known-plaintext attack** — when you know both the original value AND the result, you can always recover the key by XORing them together!

---

## Flag

```
ctf{Cemn}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef From Hex | Decode `48656c6c6f` → `Hello` |
| CyberChef XOR (HEX key) | XOR `Hello` with result hex to recover key |

---

## Key Takeaways

- `48656c6c6f` → decode From Hex → `Hello` (always check if hex decodes to readable text!)
- **Known-plaintext XOR recovery:** if you know A and B, then `A XOR B = KEY`
- CyberChef XOR encoding: **HEX** when key is hex digits, **UTF8** when key is plain text
- XOR is its own inverse: the same math finds the key, encrypts, AND decrypts!
