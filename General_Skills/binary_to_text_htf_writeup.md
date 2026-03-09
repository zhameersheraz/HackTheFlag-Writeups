# Binary to Text - Hack the Flag Writeup

**Challenge:** Binary to Text  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{bits_and_bytes}`

---

## Description

Decode this binary message:
```
01100011 01110100 01100110 01111011 01100010 01101001 01110100 01110011
01011111 01100001 01101110 01100100 01011111 01100010 01111001 01110100
01100101 01110011 01111101
```

---

## Hints

1. Each group of 8 bits (0s and 1s) represents one ASCII character. Convert each byte to decimal, then to its character.

---

## Solution

### Step 1: Open CyberChef

Go to 👉 https://gchq.github.io/CyberChef/

### Step 2: Paste the Binary

Paste the full binary string into the **Input** box.

### Step 3: Use Magic

1. Search for `Magic` in Operations
2. Drag **Magic** into the Recipe
3. Click **BAKE!**

CyberChef Magic automatically detects the encoding and shows:

```
From_Binary('Space') → ctf{bits_and_bytes}
```

Click **From_Binary('Space')** in the results to load it as the recipe.

**Output:**
```
ctf{bits_and_bytes}
```

Got the flag! 🎯

---

## What is Binary?

Computers store everything as **bits** — 0s and 1s. Each group of 8 bits is called a **byte**, and each byte maps to one character via the **ASCII** standard.

```
01100011 = 99  = 'c'
01110100 = 116 = 't'
01100110 = 102 = 'f'
01111011 = 123 = '{'
01100010 = 98  = 'b'
  ...and so on
```

Reading all bytes: **`ctf{bits_and_bytes}`** ✅

---

## Alternative Method: CyberChef From Binary (Manual)

If Magic doesn't work, do it manually:

1. Go to CyberChef
2. Search **From Binary** → drag into Recipe
3. Set **Delimiter** to `Space`
4. Paste binary → Bake!

---

## Alternative Method: Python

```python
binary = "01100011 01110100 01100110 01111011 01100010 01101001 01110100 01110011 01011111 01100001 01101110 01100100 01011111 01100010 01111001 01110100 01100101 01110011 01111101"

chars = [chr(int(b, 2)) for b in binary.split()]
print(''.join(chars))  # ctf{bits_and_bytes}
```

---

## Flag

```
ctf{bits_and_bytes}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef Magic | Auto-detect encoding → decode binary instantly |

---

## Key Takeaways

- Binary = groups of 8 bits (a byte) → each byte = one character
- CyberChef **Magic** auto-detects binary and decodes it in one click — always try this first!
- **From Binary** with `Space` delimiter is the manual CyberChef method
- Numbers and `{}` stay the same — only letters are encoded in binary
