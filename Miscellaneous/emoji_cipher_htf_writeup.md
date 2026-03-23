# Emoji Cipher - Hack the Flag Writeup

**Challenge:** Emoji Cipher  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{3m0j1_sp4k}`

---

## Description

Decode this emoji message where each emoji represents a letter:

**Key:**
🐱=c, 🌲=t, 🎭=f, 🔑={, 🦊=3, 📷=m, ⭕=0, 🎵=j, 🔒=1, 🌊=_, 🏰=s, 🔓=}, ☀️=p, 🎯=4, 🔥=k

**Message:** 🐱🌲🎭🔑🦊📷⭕🎵🔒🌊🏰☀️🎯🔥🔓

---

## Hints

1. Replace each emoji with its corresponding letter using the key provided.

---

## Solution

### Step 1: Write Out the Key

I listed each emoji and its corresponding character from the key:

| Emoji | Character |
|-------|-----------|
| 🐱 | c |
| 🌲 | t |
| 🎭 | f |
| 🔑 | { |
| 🦊 | 3 |
| 📷 | m |
| ⭕ | 0 |
| 🎵 | j |
| 🔒 | 1 |
| 🌊 | _ |
| 🏰 | s |
| ☀️ | p |
| 🎯 | 4 |
| 🔥 | k |
| 🔓 | } |

### Step 2: Decode the Message

I went through the message emoji by emoji and replaced each one with its letter:

```
🐱 🌲 🎭 🔑 🦊 📷 ⭕ 🎵 🔒 🌊 🏰 ☀️ 🎯 🔥 🔓
 c   t   f   {   3   m   0   j   1   _   s   p   4   k   }
```

Result:

```
ctf{3m0j1_sp4k}
```

Got the flag! 🎯

---

## Why This Works — Substitution Cipher

### What is a Substitution Cipher?

A substitution cipher replaces each character in the message with a different symbol using a fixed key. This is one of the oldest and simplest forms of encryption.

In this challenge, the "alphabet" was emojis instead of letters — but the concept is identical:

```
Normal substitution cipher:  A→X, B→Q, C→M...
Emoji cipher:                 🐱→c, 🌲→t, 🎭→f...
```

As long as you have the key, decoding is just a direct lookup — no math involved.

### Visual of the Decode

```
Message:   🐱  🌲  🎭  🔑  🦊  📷  ⭕  🎵  🔒  🌊  🏰  ☀️  🎯  🔥  🔓
Key maps:   c   t   f   {   3   m   0   j   1   _   s   p   4   k   }
Result:    c   t   f   {   3   m   0   j   1   _   s   p   4   k   }
           = ctf{3m0j1_sp4k}  🎯
```

---

## Alternative Methods

```python
# Python solution — map emojis to characters and decode
key = {
    '🐱': 'c', '🌲': 't', '🎭': 'f', '🔑': '{',
    '🦊': '3', '📷': 'm', '⭕': '0', '🎵': 'j',
    '🔒': '1', '🌊': '_', '🏰': 's', '☀️': 'p',
    '🎯': '4', '🔥': 'k', '🔓': '}'
}

message = ['🐱','🌲','🎭','🔑','🦊','📷','⭕','🎵','🔒','🌊','🏰','☀️','🎯','🔥','🔓']
flag = ''.join(key[e] for e in message)
print(flag)
# Output: ctf{3m0j1_sp4k}
```

---

## Flag

```
ctf{3m0j1_sp4k}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Pen and paper | Mapped each emoji to its letter using the provided key |

---

## Key Takeaways

- **Substitution ciphers** replace characters with other symbols using a fixed key
- With the key provided, decoding is just a direct one-to-one lookup — no complex math
- Emoji ciphers are a fun twist on classic substitution ciphers
- In CTFs, always map out the full key table first before decoding — it's much easier than going back and forth
- The Python dictionary approach is a clean way to decode any substitution cipher programmatically
