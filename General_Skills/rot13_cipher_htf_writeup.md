# ROT13 Cipher - Hack the Flag Writeup

**Challenge:** ROT13 Cipher  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{rot13_is_not_encryption}`

---

## Description

A classic cipher was used to encode this message: `pgs{ebg13_vf_abg_rapelcgvba}`. Decode it!

---

## Hints

1. ROT13 shifts each letter by 13 positions in the alphabet.

---

## Solution

### Step 1: Go to rot13.com

Go to 👉 https://rot13.com/

### Step 2: Paste the Cipher Text

Paste the encoded message into the input box:
```
pgs{ebg13_vf_abg_rapelcgvba}
```

### Step 3: Get the Flag

The output instantly shows:
```
ctf{rot13_is_not_encryption}
```

Got the flag! 🎯

---

## What is ROT13?

ROT13 = **ROT**ate **13** positions in the alphabet.

Each letter is shifted 13 places forward:

```
A → N      N → A
B → O      O → B
C → P      P → C
...
```

So `p` becomes `c`, `g` becomes `t`, `s` becomes `f` → `pgs` becomes `ctf`!

Since the alphabet has 26 letters, shifting by 13 twice brings you back to the start. That means **ROT13 decodes itself** — you use the exact same operation to encode AND decode.

```
pgs{ebg13_vf_abg_rapelcgvba}
           ↓ ROT13
ctf{rot13_is_not_encryption}
```

---

## Why It's Called "Not Encryption"

The flag itself says the answer — `rot13_is_not_encryption`. ROT13 is just a simple letter shift with **no key** — anyone who knows ROT13 (which is everyone in security) can decode it instantly. It provides zero real security, which is why it's used for fun/obfuscation only, not actual protection.

---

## Flag

```
ctf{rot13_is_not_encryption}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| rot13.com | Paste cipher text → instantly decoded |

---

## Key Takeaways

- **ROT13** = shift every letter by 13 positions — same operation to encode and decode
- `pgs` → `ctf`, `ebg13` → `rot13` (the `{}` and numbers stay unchanged!)
- rot13.com or CyberChef both decode it in one click — no setup needed
- ROT13 is NOT real encryption — it has no key and anyone can reverse it instantly
