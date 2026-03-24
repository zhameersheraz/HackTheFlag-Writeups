# NATO Phonetic - Hack the Flag Writeup

**Challenge:** NATO Phonetic  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{oscar}`

---

## Description

Decode this NATO phonetic alphabet message: `Charlie Tango Foxtrot Oscar Sierra Charlie Alpha Romeo`. Take the first letter of each word to form the answer. Submit as `ctf{answer_lowercase}`.

---

## Hints

1. NATO phonetic alphabet: Alpha=A, Bravo=B, Charlie=C... Take the first letter of each word. But wait — the middle letters spell something!

---

## Solution

### Step 1: Take the First Letter of Each Word

Using the NATO phonetic alphabet, each word's first letter gives us one character:

```
Charlie  → C
Tango    → T
Foxtrot  → F
Oscar    → O
Sierra   → S
Charlie  → C
Alpha    → A
Romeo    → R
```

All together: **C T F O S C A R**

### Step 2: Spot the Flag Structure

The first three letters already spell `CTF` — that's the flag prefix! The hint says *"the middle letters spell something"* — reading the remaining letters:

```
C T F { O S C A R }
        ↑ ↑ ↑ ↑ ↑
        O S C A R  =  oscar
```

The words after **Foxtrot** — `Oscar Sierra Charlie Alpha Romeo` — spell out **OSCAR**.

### Step 3: Build the Flag

```
ctf{oscar}
```

Got the flag! 🎯

---

## Why This Works — NATO Phonetic Alphabet

### What is the NATO Phonetic Alphabet?

The NATO phonetic alphabet assigns a unique word to each letter so that letters can be communicated clearly over radio or phone without confusion (e.g., "B" and "D" sound similar, but "Bravo" and "Delta" don't):

| Letter | NATO Word | Letter | NATO Word |
|--------|-----------|--------|-----------|
| A | Alpha | N | November |
| B | Bravo | O | Oscar |
| C | Charlie | P | Papa |
| D | Delta | Q | Quebec |
| E | Echo | R | Romeo |
| F | Foxtrot | S | Sierra |
| G | Golf | T | Tango |
| H | Hotel | U | Uniform |
| I | India | V | Victor |
| J | Juliet | W | Whiskey |
| K | Kilo | X | X-ray |
| L | Lima | Y | Yankee |
| M | Mike | Z | Zulu |

### Visual of the Decode

```
Message:  Charlie  Tango  Foxtrot  Oscar  Sierra  Charlie  Alpha  Romeo
Letters:     C       T       F       O       S       C       A      R
             ↓       ↓       ↓       ↓       ↓       ↓       ↓      ↓
Result:   c  t  f  {  o  s  c  a  r  }  =  ctf{oscar}  🎯
```

---

## Flag

```
ctf{oscar}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| NATO phonetic alphabet knowledge | Took the first letter of each word to spell out ctf{oscar} |

---

## Key Takeaways

- The NATO phonetic alphabet assigns a word to each letter — just take the **first letter** of each word
- The message `Charlie Tango Foxtrot` = `CTF` — always a great sign you're on the right track!
- `Oscar Sierra Charlie Alpha Romeo` = `OSCAR` — the hidden answer inside the flag braces
- Memorizing the NATO alphabet is useful for CTFs — it appears frequently in Misc and Cryptography challenges
- Real-world use: military, aviation, emergency services use it to spell out call signs and identifiers clearly
