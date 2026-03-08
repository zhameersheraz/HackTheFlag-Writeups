# Substitute Me! - Hack the Flag Writeup

**Challenge:** Substitute Me!  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 300 pts  
**Flag:** `ASK{APOSTLE_SHE_KING}`

---

## Description

Substitute Cipher Text Alphabet to Seize the Flag! Flag format is `ASK{...}`

---

## Hints

1. crpytogram (cryptogram)

---

## Solution

### Step 1: Extract the Cipher Text from the RAR File

I downloaded the RAR file and extracted it. Inside was the challenge:

```
SPM{SQUPOVR_PZR_MEDC}
```

This is the **encrypted flag** — every letter has been swapped with a different letter using a **substitution cipher**.

### Step 2: Identify the Flag Format Clue

The challenge tells us the flag format is `ASK{...}`.

The cipher text starts with `SPM{...}`.

This immediately gives us 3 free letter mappings! We know:
```
S → A
P → S  
M → K
```

(cipher letter → plain letter)

### Step 3: Use quipqiup to Crack the Rest

I went to 👉 https://quipqiup.com/

- **Puzzle box:** `SQUPOVR PZR MEDC`
- **Clues box:** `S=A P=S M=K`

The clues tell quipqiup the 3 letters we already know. It uses those as anchors to solve the rest automatically.

**quipqiup's top result:**
```
APOSTLE SHE KING
```

### Step 4: Assemble the Flag

Putting it all together:

```
SPM  { SQUPOVR _ PZR _ MEDC }
ASK  { APOSTLE _ SHE _ KING }
```

Flag: `ASK{APOSTLE_SHE_KING}` ✅

---

## Full Decryption Table

Here's every letter mapping used in this cipher:

| Cipher | Plain | | Cipher | Plain |
|--------|-------|-|--------|-------|
| S | A | | P | S |
| Q | P | | Z | H |
| U | O | | R | E |
| O | T | | M | K |
| V | L | | E | I |
| C | G | | D | N |

---

## Why This Works — What is a Substitution Cipher?

A **substitution cipher** replaces each letter with a different letter consistently throughout the message. Every A becomes the same cipher letter, every B becomes the same cipher letter, and so on.

```
Plain:   A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
Cipher:  ? ? G N I ? ? Z ? ? M L K D T S Q E A ? O V ? ? ? ?
```

Unlike the Caesar cipher (which just shifts all letters by a fixed number), substitution ciphers use a **scrambled alphabet** — making them harder to crack manually.

### How quipqiup Cracks It

quipqiup uses **frequency analysis + dictionary matching**:

1. In English, some letters appear more often (E, T, A, O, I, N...)
2. It looks at which cipher letters appear most often and guesses they map to common letters
3. It tries to match patterns to real English words from a dictionary
4. The clues we provided (`S=A P=S M=K`) gave it anchor points to solve faster

---

## Step-by-Step Breakdown

```
Cipher:  S  P  M  {  S  Q  U  P  O  V  R  _  P  Z  R  _  M  E  D  C  }
         ↓  ↓  ↓     ↓  ↓  ↓  ↓  ↓  ↓  ↓     ↓  ↓  ↓     ↓  ↓  ↓  ↓
Plain:   A  S  K  {  A  P  O  S  T  L  E  _  S  H  E  _  K  I  N  G  }
```

**SPM → ASK** (the flag format wrapper, gave us first 3 mappings for free!)

**SQUPOVR → APOSTLE**
- S=A, Q=P, U=O, P=S, O=T, V=L, R=E

**PZR → SHE**
- P=S, Z=H, R=E

**MEDC → KING**
- M=K, E=I, D=N, C=G

---

## How to Use quipqiup Effectively

1. Go to 👉 https://quipqiup.com/
2. Paste the **cipher text only** (without flag braces): `SQUPOVR PZR MEDC`
3. In Clues, enter known mappings: `S=A P=S M=K`
4. Click **Solve**
5. Look at the top results — the best scoring one is usually correct
6. Check if it makes real English sense!

> 💡 **Pro tip:** Even without clues, quipqiup can often crack substitution ciphers automatically. Try it with no clues first — if you get readable English, you're done!

---

## Alternative Tools

### Method 1: dcode.fr (Manual + Auto solver)
1. Go to 👉 https://www.dcode.fr/substitution-cipher
2. Paste cipher text
3. Use "Automatic Decryption" with known pairs
4. Manually adjust letters if needed

### Method 2: CyberChef
1. Go to 👉 https://gchq.github.io/CyberChef/
2. Search "Substitute" → set cipher alphabet
3. Once you know the full mapping, use it to decode

### Method 3: Python (once you have the full mapping)
```python
cipher = "SPM{SQUPOVR_PZR_MEDC}"

mapping = {
    'S':'A', 'P':'S', 'M':'K',
    'Q':'P', 'U':'O', 'O':'T',
    'V':'L', 'R':'E', 'Z':'H',
    'E':'I', 'D':'N', 'C':'G'
}

result = ''.join(mapping.get(c, c) for c in cipher)
print(result)  # ASK{APOSTLE_SHE_KING}
```

---

## Flag

```
ASK{APOSTLE_SHE_KING}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `strings` on RAR | Extracted the cipher text `SPM{SQUPOVR_PZR_MEDC}` |
| Flag format `ASK{}` vs `SPM{}` | Gave 3 free letter mappings: S=A, P=S, M=K |
| quipqiup.com | Auto-solved the rest using frequency analysis + our 3 known clues |

---

## Key Takeaways

- **Substitution cipher** = every letter is replaced by a fixed different letter
- **Flag format is your best friend** — `ASK{} vs SPM{}` gave 3 free mappings instantly!
- Enter those free mappings as clues in quipqiup to solve the rest automatically
- quipqiup is the #1 tool for substitution cipher CTF challenges — bookmark it!
- The shorter the cipher text, the harder it is — this one had 3 words which was enough for quipqiup to crack it
