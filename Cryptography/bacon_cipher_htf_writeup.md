# Bacon Cipher - Hack the Flag Writeup

**Challenge:** Bacon Cipher  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{baconencrypted}`

---

## Description

Francis Bacon invented a cipher using two symbols. Decode: `AAAAB AAAAA AAABA ABBBA ABBAB AABAA ABBAB AAABA BAAAB BBAAA ABBBB BAABB AABAA AAABB` (A=0, B=1, 5-bit binary for letter position). Decode the message and submit as `ctf{decoded_text}`.

---

## Hints

1. Each 5-letter group represents a letter. AAAAA=A(0), AAAAB=B(1), etc. Convert each group to a number and then to a letter.

---

## Solution

### Step 1: Use dcode.fr Bacon Cipher Decoder

I went to 👉 https://www.dcode.fr/bacon-cipher and pasted in the encoded message:

```
AAAAB AAAAA AAABA ABBBA ABBAB AABAA ABBAB AAABA BAAAB BBAAA ABBBB BAABB AABAA AAABB
```

The decoder returned:

```
BACONENCRYPTED
```

### Step 2: Format the Flag

I tried several formats before finding the correct one:

```
ctf{BACONENCRYPTED}    ❌ Wrong
ctf{BACON_ENCRYPTED}   ❌ Wrong
ctf{bacon_encrypted}   ❌ Wrong
ctf{baconencrypted}    ✅ Correct!
```

The flag format was all **lowercase with no underscore**.

```
ctf{baconencrypted}
```

Got the flag! 🎯

---

## Why This Works — Bacon's Cipher

### What is Bacon's Cipher?

Invented by Francis Bacon in 1605, Bacon's cipher encodes each letter as a 5-character sequence of two symbols (A and B). With A=0 and B=1, each group is essentially a **5-bit binary number** that maps to a letter position:

```
AAAAA = 00000 = 0  → A
AAAAB = 00001 = 1  → B
AAABA = 00010 = 2  → C
AAABB = 00011 = 3  → D
...and so on
```

### Full Decode Table (first few)

| Group | Binary | Position | Letter |
|-------|--------|----------|--------|
| AAAAA | 00000 | 0 | A |
| AAAAB | 00001 | 1 | B |
| AAABA | 00010 | 2 | C |
| AAABB | 00011 | 3 | D |
| AABAA | 00100 | 4 | E |
| AABAB | 00101 | 5 | F |
| AABBA | 00110 | 6 | G |
| AABBB | 00111 | 7 | H |
| ABAAA | 01000 | 8 | I |

### Manual Decode of the Message

```
AAAAB → B
AAAAA → A
AAABA → C
ABBBA → O (01110 = 14)
ABBAB → N (01101 = 13)
AABAA → E (00100 = 4)
ABBAB → N
AAABA → C
BAAAB → R (10001 = 17)
BBAAA → Y (11000 = 24)
ABBBB → P (01111 = 15)
BAABB → T (10011 = 19)
AABAA → E
AAABB → D (00011 = 3)

= BACONENCRYPTED
```

---

## Alternative Method (Python)

```python
# Bacon cipher decoder
bacon_map = {format(i, '05b').replace('0','A').replace('1','B'): chr(65+i) 
             for i in range(26)}

message = "AAAAB AAAAA AAABA ABBBA ABBAB AABAA ABBAB AAABA BAAAB BBAAA ABBBB BAABB AABAA AAABB"
decoded = ''.join(bacon_map[group] for group in message.split())
print(decoded)           # BACONENCRYPTED
print(f"ctf{{{decoded.lower()}}}")  # ctf{baconencrypted}
```

---

## Flag Format Lesson

This challenge taught an important CTF lesson — **always try lowercase with no separators first** when the flag format isn't explicitly specified:

```
ctf{BACONENCRYPTED}    ❌
ctf{BACON_ENCRYPTED}   ❌
ctf{bacon_encrypted}   ❌
ctf{baconencrypted}    ✅  ← all lowercase, no underscore
```

When in doubt: try `lowercase → lowercase_with_underscores → UPPERCASE → original case`

---

## Flag

```
ctf{baconencrypted}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| dcode.fr/bacon-cipher | Decoded the Bacon cipher groups to get BACONENCRYPTED |

---

## Key Takeaways

- **Bacon's cipher** = each letter encoded as 5 A/B characters (5-bit binary with A=0, B=1)
- Fastest solve: paste into https://www.dcode.fr/bacon-cipher
- Always try **all lowercase, no underscore** as the default CTF flag format
- If `ctf{RESULT}` doesn't work, try `ctf{result}` before adding underscores
- Bacon's cipher is purely a substitution — it encodes letter positions, not actual encryption
