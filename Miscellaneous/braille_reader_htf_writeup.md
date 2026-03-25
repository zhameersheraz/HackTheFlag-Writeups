# Braille Reader - Hack the Flag Writeup

**Challenge:** Braille Reader  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{braille_reader}`

---

## Description

The following Braille characters spell out the flag: `⠉⠞⠋⠐⠃⠗⠁⠊⠇⠇⠑⠀⠗⠑⠁⠙⠑⠗⠐⠂`. Decode the Braille to find the flag. Submit your answer in the format `ctf{decoded_text}`.

---

## Hints

1. Look up a Braille alphabet chart to decode each character.

---

## Solution

### Step 1: Find a Braille Translator

I went to 👉 https://www.brailletranslator.org/ and pasted in the Braille characters from the challenge.

### Step 2: Get the Translation

The translator returned:

```
ctf(braille reader)
```

### Step 3: Convert to Proper Flag Format

The Braille translator uses `()` for curly braces and a space instead of underscore — this is just how the Grade 2 Braille translator renders the flag format. Converting to the correct CTF format:

```
( → {
) → }
  → _   (space becomes underscore)
```

Result:

```
ctf{braille_reader}
```

Got the flag! 🎯

---

## Why This Works — Braille Encoding

### What is Braille?

Braille is a tactile writing system used by visually impaired people. Each character is represented by a pattern of raised dots arranged in a 2×3 grid (6 dots total), giving 64 possible combinations.

```
Braille cell layout:
  Dot 1  Dot 4
  Dot 2  Dot 5
  Dot 3  Dot 6
```

In digital form, Braille uses Unicode characters in the range U+2800 to U+28FF, which is why the challenge shows those dot-pattern symbols.

### Visual of the Decode

```
Braille: ⠉ ⠞ ⠋ ⠐⠃ ⠗ ⠁ ⠊ ⠇ ⠇ ⠑ ⠀ ⠗ ⠑ ⠁ ⠙ ⠑ ⠗ ⠐⠂
           ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓
Text:    c  t  f  (  b  r  a  i  l  l  e     r  e  a  d  e  r  )
                    ↓                    ↓
Format fix:         {                    }  + space → _
                    ↓
Flag:    ctf{braille_reader}  🎯
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| brailletranslator.org | Pasted the Braille characters and got the decoded text |

---

## Key Takeaways

- **Braille translators** online handle the hard work — no need to memorize the dot patterns
- The translator outputs `()` instead of `{}` and spaces instead of `_` — always adjust to CTF flag format
- Braille is a substitution system — each dot pattern maps to one character
- In CTFs, Braille usually appears as Unicode dot characters (⠉⠞⠋...) — paste them directly into a translator
- Good tool to bookmark: https://www.brailletranslator.org/
