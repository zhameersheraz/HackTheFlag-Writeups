# Caesar Salad - Hack the Flag Writeup

**Challenge:** Caesar Salad  
**Category:** Cryptography  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{caesar_cipher_cracked}`

---

## Description

Decrypt this Caesar cipher message: `fwi{fdhvdu_flskhu_fudfnhg}`. The shift value is a small number.

---

## Hints

1. Caesar cipher shifts letters by a fixed number. Try shift values 1-25.

---

## Solution

### Step 1: Use the caesar Command

I used the `caesar` command on Kali Linux which tries all 25 possible shifts at once:

```bash
echo "fwi{fdhvdu_flskhu_fudfnhg}" | caesar
```

**Output (looking for the readable line):**
```
ctf{caesar_cipher_cracked}
```

Got the flag! 🎯

---

## Why This Works

### What is the Caesar Cipher?

The **Caesar cipher** shifts each letter by a fixed number of positions in the alphabet. To decrypt it, you just shift in the opposite direction.

The `caesar` command tries all 25 possible shifts and prints all results — you just look for the one that makes sense.

In this case the shift was **3**:
```
f → c (shift back 3)
w → t (shift back 3)
i → f (shift back 3)
```

So `fwi` becomes `ctf`!

### Simple Breakdown

```
Get ciphertext: fwi{fdhvdu_flskhu_fudfnhg}
      |
      v
Run: echo "ciphertext" | caesar
      |
      v
Look for the readable output
      |
      v
Flag found: ctf{caesar_cipher_cracked}
```

---

## Alternative Method: dcode.fr

1. Go to https://www.dcode.fr/caesar-cipher
2. Paste `fwi{fdhvdu_flskhu_fudfnhg}` in the input
3. Click **Decrypt** and it will try all shifts automatically

---

## Commands Used

```bash
echo "fwi{fdhvdu_flskhu_fudfnhg}" | caesar
```

---

## Flag

```
ctf{caesar_cipher_cracked}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `caesar` (bsdgames) | Try all 25 Caesar cipher shifts |

---

## Key Takeaways

- Caesar cipher is one of the oldest and simplest ciphers
- It just shifts each letter by a fixed number
- The `caesar` command tries all 25 shifts so you don't need to know the shift value
- `fwi` decoding to `ctf` is the classic giveaway for Caesar cipher CTF challenges
