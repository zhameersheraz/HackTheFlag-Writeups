# Base64 Decode - Hack the Flag Writeup

**Challenge:** Base64 Decode  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{base64_dec0ding_is_easy}`

---

## Description

We intercepted an encoded message: `Y3Rme2Jhc2U2NF9kZWMwZGluZ19pc19lYXN5fQ==`. Can you decode it?

---

## Hints

1. Base64 is a common encoding scheme. Try an online decoder or use the command line.

---

## Solution

### Method 1: Kali Terminal (Fastest)

```bash
echo "Y3Rme2Jhc2U2NF9kZWMwZGluZ19pc19lYXN5fQ==" | base64 -d
```

Output:
```
ctf{base64_dec0ding_is_easy}
```

Got the flag! 🎯

### Method 2: base64decode.org

1. Go to 👉 https://www.base64decode.org/
2. Paste: `Y3Rme2Jhc2U2NF9kZWMwZGluZ19pc19lYXN5fQ==`
3. Click Decode → `ctf{base64_dec0ding_is_easy}`

---

## What is Base64?

Base64 converts binary/text into a safe string of letters, numbers, `+`, `/`, and `=`. It's used to safely transfer data — not to hide it securely (anyone can decode it instantly!).

You can recognize Base64 by the `==` padding at the end.

---

## Flag

```
ctf{base64_dec0ding_is_easy}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `echo "..." \| base64 -d` | Decode Base64 in one command on Kali |
| base64decode.org | Online alternative |

---

## Key Takeaways

- `echo "..." | base64 -d` decodes any Base64 string instantly in terminal
- Base64 strings often end with `==` — easy to recognize!
- Base64 is **encoding**, not **encryption** — no key needed, anyone can decode it
