# Octal Decode - Hack the Flag Writeup

**Challenge:** Octal Decode  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{oct4l_d3c0d3}`

---

## Description

Decode this octal string to ASCII:
```
143 164 146 173 157 143 164 64 154 137 144 63 143 60 144 63 175
```
Each number is an octal (base-8) representation of an ASCII character.

---

## Hints

1. Convert each octal number to decimal first, then to ASCII. For example, octal 143 = decimal 99 = "c".

---

## Solution

### Step 1: Open CyberChef

Go to 👉 https://gchq.github.io/CyberChef/

### Step 2: Use Magic

1. Paste the octal string into the **Input** box:
   ```
   143 164 146 173 157 143 164 64 154 137 144 63 143 60 144 63 175
   ```
2. Search **Magic** → drag into Recipe → click **BAKE!**

CyberChef Magic detects it as octal and shows:

```
From_Octal('Space') → ctf{oct4l_d3c0d3}
```

Click the recipe to load it. **Output: `ctf{oct4l_d3c0d3}`** ✅

Got the flag! 🎯

---

## What is Octal?

Octal = **base-8** number system (digits 0–7). Computers sometimes represent characters in octal.

Each octal number → convert to decimal → look up ASCII character:

| Octal | Decimal | Character |
|-------|---------|-----------|
| 143 | 99 | **c** |
| 164 | 116 | **t** |
| 146 | 102 | **f** |
| 173 | 123 | **{** |
| 157 | 111 | **o** |
| ... | ... | ... |

Reading all: **`ctf{oct4l_d3c0d3}`** ✅

---

## Alternative Method: CyberChef From Octal (Manual)

If Magic doesn't work:
1. Search **From Octal** → drag into Recipe
2. Set **Delimiter** to `Space`
3. Paste numbers → Bake!

---

## Flag

```
ctf{oct4l_d3c0d3}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef Magic | Auto-detects octal → decodes instantly |

---

## Key Takeaways

- **Octal** = base-8 numbers — each one maps to an ASCII character
- CyberChef **Magic** auto-detects octal just like it did for binary — always try Magic first!
- Manual path: **From Octal** with `Space` delimiter in CyberChef
- Octal 143 = decimal 99 = `c` (the hint example told you the first character = start of `ctf`!)
