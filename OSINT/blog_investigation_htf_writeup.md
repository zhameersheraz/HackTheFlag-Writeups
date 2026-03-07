# Blog Investigation - Hack the Flag Writeup

**Challenge:** Blog Investigation  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{0s1nt_m4st3r_d3t3ct1v3}`

---

## Description

A hacker's personal blog contains hidden information. Investigate the page thoroughly — check the source code, HTML comments, meta tags, and hidden elements. OSINT is about finding information in plain sight!

---

## Hints

1. View the page source (Ctrl+U). Check HTML comments, meta tags, hidden elements (display:none), and the JavaScript code.

---

## Solution

### Step 1: Open the Personal Blog

I clicked the **personal blog** link from the challenge page. It opened a simple blog by "Alex Johnson" — a cybersecurity enthusiast from San Francisco.

The page looked normal at first glance:
- About Me section with GitHub, email, and Twitter
- A recent blog post titled "My Journey in CTF"
- A contact section

Nothing suspicious on the surface. Time to dig deeper.

### Step 2: Notice the Footer Hint

At the very bottom of the page, the footer had a suspicious message:

```
© 2024 Alex Johnson. Built with ❤️ and too many comments left in the source code.
```

**"too many comments left in the source code"** — that's a big hint! 👀

### Step 3: Inspect the Page Source (My Method)

I opened **Browser DevTools** by pressing `F12`, then clicked the **Elements** tab and navigated to the `<script>` section.

Inside the JavaScript, I found **commented-out code** that was hidden from the normal page view:

```javascript
// TODO: Clean up old code
// var secretFlag = "ctf{0s1nt_m4st3r_d3t3ct1v3}";
// console.log("Debug mode enabled");
```

The flag was sitting right there in a JavaScript comment! 🎯

### Step 4: Extract the Flag

```
var secretFlag = "ctf{0s1nt_m4st3r_d3t3ct1v3}";
                   ↑
              Flag is here!
```

Got the flag! 🎯

---

## Why This Works

### What are HTML/JS Comments?

Comments are notes developers leave in code — they're invisible on the webpage but fully visible in the source code. Anyone can read them by inspecting the page.

| Comment Type | Syntax | Where it appears |
|--------------|--------|-----------------|
| HTML comment | `<!-- text -->` | HTML source |
| JavaScript comment | `// text` | Inside `<script>` tags |
| CSS comment | `/* text */` | Inside `<style>` tags |

In this challenge, a developer "forgot" to remove a `secretFlag` variable that was commented out in the JavaScript — a very realistic mistake that happens in real websites too!

### What to Always Check on a Web Page

When investigating a webpage in OSINT/web challenges, always look at:

```
1. Page Source (Ctrl+U)     → HTML comments, hidden meta tags
2. <script> tags            → JavaScript variables, commented code
3. <head> section           → Meta tags, author info, keywords
4. Hidden elements          → style="display:none" or hidden inputs
5. Footer                   → Often has hints or extra info
6. Browser DevTools (F12)   → Full element tree, network requests
```

---

## Alternative Methods

### Method 1: View Page Source (Ctrl+U)
1. Press `Ctrl+U` on the blog page
2. Press `Ctrl+F` and search for `ctf`
3. The flag shows up immediately inside the `<script>` block

### Method 2: Browser DevTools → Sources Tab
1. Press `F12` to open DevTools
2. Click the **Sources** tab
3. Open the page's HTML file
4. Search for `ctf` or `flag` using `Ctrl+F`

### Method 3: curl on Kali Linux
```bash
curl https://[blog-url] | grep -i "flag\|ctf"
```

Output:
```
// var secretFlag = "ctf{0s1nt_m4st3r_d3t3ct1v3}";
```

### Method 4: wget + grep
```bash
wget -q -O - https://[blog-url] | grep "ctf{"
```

---

## Flag

```
ctf{0s1nt_m4st3r_d3t3ct1v3}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Browser DevTools (F12) | Inspect the page's HTML and JavaScript source |
| Elements Tab | Navigate the full HTML structure |
| Ctrl+U (View Source) | Quick way to read raw page source |
| Footer hint | Tipped off that comments were left in the code |

---

## Key Takeaways

- **Always read the page source** — what you see on screen is not the full picture
- JavaScript comments (`//`) are invisible on the page but visible in source code
- Footers and `<meta>` tags often contain extra information in CTF challenges
- `Ctrl+U` or `F12 → Elements` are your best friends in web OSINT challenges
- In real life, developers sometimes accidentally leave API keys, passwords, or debug info in commented-out code — this is a real vulnerability!
