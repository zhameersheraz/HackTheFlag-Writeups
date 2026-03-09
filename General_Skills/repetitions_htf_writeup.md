# repetitions - Hack the Flag Writeup

**Challenge:** repetitions  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_9b59b35c}`

---

## Description

Can you make sense of this file?

---

## Hints

1. decode decode decode decode

---

## The File Contents

The downloaded file contains:
```
VmpGU1EyRXlUWGxTYmxKVVYwZFNWbGxyV21GV1JteDBUbFpPYWxKdFVsaFpWVlUxWVZaS1ZWWnVh
RmRXZWtab1dWWmtSMk5yTlZWWApiVVpUVm10d1VWZFdVa2RpYlZaWFZtNVdVZ3BpU0VKeldWUkNk
...
```

It's Base64 — but the hint says "decode decode decode decode" — meaning it's **Base64 encoded multiple times**!

---

## Solution

### Step 1: Decode 5 Times in One Command

Keep piping `base64 -d` until you get readable output:

```bash
echo "VmpGU1EyRXlUWGxTYmxKVVYwZFNWbGxyV21GV1JteDBUbFpPYWxKdFVsaFpWVlUxWVZaS1ZWWnVh..." | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
```

After 5 decodes, output:
```
cGljb0NURntiYXNlNjRfbjNzdDNkX2RpYzBkIW44X2Qwd25sMDRkM2RfOWI1OWIzNWN9Cg==
```

### Step 2: Decode One More Time

```bash
echo "cGljb0NURntiYXNlNjRfbjNzdDNkX2RpYzBkIW44X2Qwd25sMDRkM2RfOWI1OWIzNWN9Cg==" | base64 -d
```

Output:
```
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_9b59b35c}
```

Got the flag! 🎯

---

## What is Nested Base64?

Base64 is an encoding — not encryption. You can encode something in Base64 multiple times:

```
Flag → Base64 → Base64 → Base64 → Base64 → Base64 → Base64 → (the file)
                                                                    ↓
                                          decode 6 times to get back the flag
```

The hint "decode decode decode decode" was telling you to decode multiple times — the challenge is called "repetitions" because you repeat the same decode step!

---

## Common Mistake to Avoid

Forgetting `-d` encodes instead of decodes:

```bash
echo "..." | base64      # ❌ ENCODES it (makes it longer/wrong!)
echo "..." | base64 -d   # ✅ DECODES it
```

You can chain as many `| base64 -d` as needed until the output looks like readable text.

---

## Flag

```
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_9b59b35c}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `base64 -d` | Decode Base64 — piped 6 times to unwrap all layers |

---

## Key Takeaways

- **Nested Base64** = same encoding applied multiple times — just keep decoding until you get readable text
- Pipe `| base64 -d` multiple times in one command to decode all layers at once
- The hint "decode decode decode decode" = a direct clue to decode multiple times!
- `base64` without `-d` = encodes; `base64 -d` = decodes — always use `-d` to decode
