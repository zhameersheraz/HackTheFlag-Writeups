# URL Encoding - Hack the Flag Writeup

**Challenge:** URL Encoding  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{url_3nc0d1ng}`

---

## Description

This URL-encoded string contains the flag:
```
%63%74%66%7B%75%72%6C%5F%33%6E%63%30%64%31%6E%67%7D
```

---

## Hints

1. URL encoding uses `%XX` where XX is the hex value of the character. You can paste this into a URL decoder.

---

## Solution

### Step 1: Go to urldecoder.org

Go to 👉 https://www.urldecoder.org/

### Step 2: Paste the Encoded String

Paste into the input box:
```
%63%74%66%7B%75%72%6C%5F%33%6E%63%30%64%31%6E%67%7D
```

### Step 3: Click Decode

Output:
```
ctf{url_3nc0d1ng}
```

Got the flag! 🎯

---

## What is URL Encoding?

URLs can only contain certain safe characters. Special characters like `{`, `}`, spaces, etc. must be **encoded** as `%XX` where `XX` is the hex value of the character.

```
%63 = 99  = 'c'
%74 = 116 = 't'
%66 = 102 = 'f'
%7B = 123 = '{'
%75 = 117 = 'u'
...and so on
```

You've actually seen URL encoding before — remember from the Command Injection challenge where `;` showed up as `%3B` in the URL bar? Same thing!

---

## Alternative Method: Browser Console

Press **F12** → Console tab → type:

```javascript
decodeURIComponent('%63%74%66%7B%75%72%6C%5F%33%6E%63%30%64%31%6E%67%7D')
```

Output: `ctf{url_3nc0d1ng}`

---

## Flag

```
ctf{url_3nc0d1ng}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| urldecoder.org | Paste encoded string → decoded flag instantly |

---

## Key Takeaways

- URL encoding = `%XX` where XX is the hex value of the character
- `%7B` = `{` and `%7D` = `}` — you'll see these a lot in web challenges
- urldecoder.org or browser console `decodeURIComponent()` both decode instantly
- You already saw URL encoding in the Command Injection challenge — `;` appeared as `%3B` in the URL!
