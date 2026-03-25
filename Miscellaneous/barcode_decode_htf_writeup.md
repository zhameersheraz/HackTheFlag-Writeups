# Barcode Decode - Hack the Flag Writeup

**Challenge:** Barcode Decode  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{barc0d3_d3c0d3d}`

---

## Description

Download the barcode image and scan it. The result might need further decoding before you get the flag.

---

## Hints

1. Scan the barcode first, then look at what encoding the result might be in.

---

## Solution

### Step 1: Download the Barcode Image

I downloaded the challenge file `barcode_challenge.png` from the challenge page.

### Step 2: Scan the Barcode with zbarimg

On Kali Linux, `zbarimg` can decode barcodes directly from the command line:

```bash
zbarimg barcode_challenge.png
```

Output:
```
CODE-128:Q1RGe2JhcmMwZDNfZDNjMGQzZH0=
scanned 1 barcode symbols from 1 images in 0.02 seconds
```

The barcode type is **CODE-128** and the content is:
```
Q1RGe2JhcmMwZDNfZDNjMGQzZH0=
```

That `=` at the end — classic Base64 padding!

### Step 3: Decode the Base64

```bash
echo "Q1RGe2JhcmMwZDNfZDNjMGQzZH0=" | base64 -d
```

Output:
```
CTF{barc0d3_d3c0d3d}
```

Got the flag! 🎯

---

## Why This Works — Barcode + Base64

### Two Layers to Solve

This challenge had two encoding layers:

```
barcode_challenge.png
        ↓ zbarimg (scan barcode)
Q1RGe2JhcmMwZDNfZDNjMGQzZH0=   ← Base64 string
        ↓ base64 -d
CTF{barc0d3_d3c0d3d}            ← flag! 🎯
```

### What is CODE-128?

CODE-128 is a high-density linear barcode format that can encode all 128 ASCII characters. It's commonly used in shipping labels and inventory systems. Unlike QR codes, it's a 1D barcode (just vertical bars).

### What is zbarimg?

`zbarimg` is a command-line tool in Kali Linux that reads barcodes and QR codes from image files. It supports CODE-128, QR codes, EAN, UPC, and many more formats.

```bash
# Install if not present
sudo apt install zbar-tools

# Scan any barcode/QR image
zbarimg image.png
```

---

## Alternative Methods

```bash
# Method 1: zbarimg (Kali) — used in this solve
zbarimg barcode_challenge.png | grep -oP '(?<=:).*' | base64 -d

# Method 2: Online barcode scanner
# Go to: https://online-barcode-reader.inliteresearch.com/
# Upload the image → copy the result → paste into base64decode.org

# Method 3: Python with pyzbar
python3 -c "
from pyzbar.pyzbar import decode
from PIL import Image
import base64

img = Image.open('barcode_challenge.png')
result = decode(img)[0].data.decode()
print(base64.b64decode(result).decode())
"
```

---

## Flag

```
ctf{barc0d3_d3c0d3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `zbarimg` (Kali Linux) | Scanned the CODE-128 barcode and extracted the Base64 string |
| `base64 -d` | Decoded the Base64 string to reveal the flag |

---

## Key Takeaways

- Always check if barcode output looks like Base64 — the `=` padding is the giveaway
- `zbarimg` is the fastest barcode scanner in Kali — works on PNG, JPG, and most image formats
- Two-layer challenges: scan first, then decode the output
- `zbarimg image.png | awk -F: '{print $2}' | base64 -d` — one-liner to do both steps at once
- Online alternative: https://online-barcode-reader.inliteresearch.com/ for when you don't have Kali handy
