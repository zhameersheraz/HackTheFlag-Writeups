# QR Code Challenge - Hack the Flag Writeup

**Challenge:** QR Code Challenge  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{qr_c0d3_sc4nn3d}`

---

## Description

Download the QR code image and scan it to find the flag.

---

## Hints

1. Use any QR code scanner app or website to decode the image.

---

## Solution

### Step 1: Download the Challenge File

Download the provided file `qr_challenge.png` from the challenge page.

### Step 2: Decode the QR Code (My Method)

I used **zbarimg** on Kali Linux — a command line tool that can scan and decode barcodes and QR codes from image files.

```bash
zbarimg qr_challenge.png
```

Output:

```
QR-Code:ctf{qr_c0d3_sc4nn3d}
scanned 1 barcode symbols from 1 images in 0.02 seconds
```

Got the flag! 🎯

---

## Why This Works

A **QR code** (Quick Response code) is just a 2D barcode that stores encoded text. When you scan it, it decodes back to whatever was stored inside — in this case, the flag.

`zbarimg` reads the image file, detects the QR code pattern, and outputs the decoded text directly in the terminal. No need to open a browser or phone camera.

---

## Alternative Methods

### Method 1: webqr.com (Online, Easiest)
1. Go to 👉 https://webqr.com
2. Click **Upload** and select `qr_challenge.png`
3. The flag appears instantly on screen

### Method 2: zxing.org Decoder
1. Go to 👉 https://zxing.org/w/decode.jspx
2. Upload `qr_challenge.png`
3. Click **Submit**
4. Result shows: `ctf{qr_c0d3_sc4nn3d}`

### Method 3: 4qrcode.com
1. Go to 👉 https://4qrcode.com/scan-qr-code.php
2. Upload the image
3. Flag is decoded and displayed

### Method 4: Your Phone Camera
1. Open your phone's camera app
2. Point it at the QR code on your screen
3. It will automatically read it and show the flag

### Method 5: Python (zxing library)
```python
import zxing

reader = zxing.BarCodeReader()
barcode = reader.decode("qr_challenge.png")
print(barcode.parsed)
```

Output:
```
ctf{qr_c0d3_sc4nn3d}
```

---

## Flag

```
ctf{qr_c0d3_sc4nn3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| zbarimg (Kali Linux) | Decode QR code directly from terminal |
| webqr.com | Online QR code scanner |
| zxing.org | Online barcode/QR decoder |

---

## Key Takeaways

- QR codes are just encoded text — any scanner can read them
- `zbarimg` is the fastest way to decode QR codes on Linux (no browser needed)
- Online tools like webqr.com work just as well if you don't have Linux
- Always try `zbarimg` first in CTFs — it's instant and works on any image format
