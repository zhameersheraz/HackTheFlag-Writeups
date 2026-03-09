# Morse Code - Hack the Flag Writeup

**Challenge:** Morse Code  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `CTF{M0RS3_C0D3}`

---

## Description

Decode this Morse code message:
```
-.-. - ..-. { -- ----- .-. ... ..--- --...- -.-. ----- -.. ...-- }
```
Each group of dots and dashes represents a character. Decode the full message to find the flag.

---

## Solution

### Step 1: Go to a Morse Code Decoder

Go to 👉 https://morsedecoder.com/ (or https://morsecode.world/international/translator.html)

### Step 2: Paste the Morse Code in the RIGHT Box

> ⚠️ **Important:** Paste the morse code into the **Morse Code** box (bottom), NOT the Text box (top)! If you paste into the Text box, it tries to convert text TO morse and gives a huge wrong output.

Paste into the **Morse Code** input:
```
-.-. - ..-. { -- ----- .-. ... ..--- --...- -.-. ----- -.. ...-- }
```

### Step 3: Read the Output

The decoder outputs:
```
CTF#M0RS3_C0D3#
```

### Step 4: Fix the `#` Symbols → Flag!

The `#` symbols appear because **`{` and `}` have no standard Morse code** — the decoder can't translate them, so it uses `#` as a placeholder instead.

Since we know the flag format is `CTF{...}`, just replace both `#` with `{` and `}`:

```
CTF#M0RS3_C0D3#
    ↓
CTF{M0RS3_C0D3}
```

Submit: `CTF{M0RS3_C0D3}` ✅

Got the flag! 🎯

---

## Why Your Other Guesses Were Wrong

The Morse code decoded to uppercase with specific leet substitutions:

```
CTF{M0RS3_C0D3}  ✅ correct
ctf{m0rs3_c0d3}  ❌ wrong case
ctf{M0RS3_C0D3}  ❌ wrong case for ctf part
```

The flag uses **uppercase** — `CTF` not `ctf`, `M0RS3` not `m0rs3`. Always submit exactly what the decoder gives you!

---

## What is Morse Code?

Morse code represents letters using **dots (.) and dashes (-)**:

| Character | Morse |
|-----------|-------|
| C | `-.-. ` |
| T | `-` |
| F | `..-.` |
| M | `--` |
| 0 | `-----` |
| R | `.-.` |
| S | `...` |
| 3 | `...--` |
| D | `-..` |

Each letter is separated by a space, each word by a longer gap.

---

## Step-by-Step Summary

| Step | Action |
|------|--------|
| 1 | Go to morsedecoder.com |
| 2 | Paste morse into the **Morse Code box** (NOT the Text box!) |
| 3 | Read output: `CTF#M0RS3_C0D3#` |
| 4 | Replace `#` → `{` and `}` (no standard morse for these) |
| 5 | Submit `CTF{M0RS3_C0D3}` |

---

## Flag

```
CTF{M0RS3_C0D3}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| morsedecoder.com | Paste morse code → decoded text |

---

## Key Takeaways

- Paste the morse into the **Morse Code box**, not the Text box — wrong box gives wrong output!
- `{` and `}` have no standard Morse code — decoders show `#` as placeholder → replace with `{}`
- Submit the flag **exactly as decoded** — case matters! `CTF` not `ctf`
- Always try the exact decoded output first before guessing leet variations
