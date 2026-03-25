# Pig Latin Flag - Hack the Flag Writeup

**Challenge:** Pig Latin Flag  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{pig_latin_solved}`

---

## Description

Translate from Pig Latin: `tcfay{igpay_atinlay_olvedsay}`. In Pig Latin, the first consonant cluster moves to the end and "ay" is added.

---

## Hints

1. Reverse the Pig Latin: remove "ay" from the end, move the last consonant cluster back to the front.

---

## Solution

### Step 1: Understand the Pig Latin Rules

**Encoding (forward):** Take the first consonant cluster, move it to the end, add "ay".

**Decoding (reverse):** Remove "ay" from the end, move the trailing consonant cluster back to the front.

### Step 2: Decode Each Word

Breaking the encoded message `tcfay{igpay_atinlay_olvedsay}` into parts:

**`tcfay` → `ctf`**
```
tcfay → remove "ay" → tcf → move consonant cluster back → ctf
```

**`igpay` → `pig`**
```
igpay → remove "ay" → igp → move "p" back to front → pig
```

**`atinlay` → `latin`**
```
atinlay → remove "ay" → atinl → move "l" back to front → latin
```

**`olvedsay` → `solved`**
```
olvedsay → remove "ay" → olveds → move "s" back to front → solved
```

### Step 3: Put It Together

```
tcfay { igpay _ atinlay _ olvedsay }
 ctf  {  pig  _  latin  _  solved  }
= ctf{pig_latin_solved}
```

Got the flag! 🎯

---

## Why This Works — Pig Latin Rules

### How Pig Latin Encoding Works

Pig Latin is a language game where words are altered following a simple rule:

**Words starting with consonants:**
```
Move the first consonant cluster to the end, then add "ay"

pig    → ig   + p   + ay = igpay
latin  → atin + l   + ay = atinlay
solved → olved + s  + ay = olvedsay
ctf    → tf   + c   + ay = tcfay  (all consonants — rearranged)
```

**Words starting with vowels:**
```
Just add "ay" to the end (no letters move)

apple → appleay
```

### Visual of the Decode

```
Encoded:  tcfay  {  igpay  _  atinlay  _  olvedsay  }
           ↓         ↓           ↓           ↓
Step:    -ay+fix   -ay+p→front  -ay+l→front  -ay+s→front
           ↓         ↓           ↓           ↓
Decoded:  ctf    {   pig   _   latin   _   solved   }
```

---

## Alternative Methods

```python
# Python — reverse Pig Latin decoder
def decode_pig_latin(word):
    if word.endswith('ay'):
        word = word[:-2]          # remove "ay"
        # find where vowels start from the end
        vowels = 'aeiou'
        i = len(word) - 1
        while i >= 0 and word[i] not in vowels:
            i -= 1
        # move trailing consonants to front
        return word[i+1:] + word[:i+1]
    return word

words = ['tcfay', 'igpay', 'atinlay', 'olvedsay']
decoded = [decode_pig_latin(w) for w in words]
print('ctf{' + '_'.join(decoded[1:]) + '}')
# Output: ctf{pig_latin_solved}
```

---

## Flag

```
ctf{pig_latin_solved}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Pig Latin rules | Removed "ay" and moved trailing consonant clusters back to the front of each word |

---

## Key Takeaways

- **Pig Latin decoding** = remove `ay` from end, move the trailing consonant cluster back to the front
- Work word by word — split on `{`, `_`, `}` separators
- `igpay` → `pig`, `atinlay` → `latin`, `olvedsay` → `solved`
- The `ctf{` prefix is encoded too — always decode everything including the wrapper
- Pig Latin is a classic CTF Misc challenge — memorize the decode rule: **remove "ay", move end consonants to front**
