# Inspect Element - Hack the Flag Writeup

**Challenge:** Inspect Element  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{view_source_master}`

---

## Description

Every website has secrets hidden in its source code. The flag is hidden in this very page! Right-click and inspect the HTML source to find it.

---

## Hints

1. Try right-clicking the page and viewing the source code.

---

## Solution

### Step 1: Open the Challenge Page

I opened the challenge page on the Hack the Flag website.

### Step 2: Open Browser DevTools

I right-clicked anywhere on the page and selected **Inspect** to open the browser DevTools.

### Step 3: Find the Flag in the HTML

Inside the **Elements** tab, I looked through the HTML source and found the flag hidden inside an HTML comment:

```html
<!-- ctf{view_source_master} -->
```

The flag was hidden using `display:none` and inside a comment so it would not be visible on the page.

Got the flag! 🎯

---

## Why This Works

Developers sometimes accidentally leave sensitive information in HTML comments or hidden elements. These are not visible to regular users but anyone can see them by opening DevTools or viewing the page source.

### Simple Breakdown

```
Open challenge page
      |
      v
Right-click and Inspect
      |
      v
Look through HTML in Elements tab
      |
      v
Flag found inside HTML comment!
```

---

## Alternative Method: View Page Source

Instead of DevTools, you can also press **Ctrl+U** to open the raw page source and search for the flag:

```
Ctrl+U to open page source
Ctrl+F to search
Type "ctf{" to find the flag
```

---

## Flag

```
ctf{view_source_master}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Browser DevTools (F12) | Inspect the HTML source code |

---

## Key Takeaways

- Never hide sensitive info in HTML comments — anyone can read them with DevTools
- `display:none` only hides elements visually, the text is still in the source code
- Always check page source and HTML comments in Web/General Skills CTF challenges
- Press **F12** or right-click and Inspect to open DevTools quickly
