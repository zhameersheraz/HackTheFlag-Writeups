# PNG Chunk Analysis - Hack the Flag Writeup

**Challenge:** PNG Chunk Analysis  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{png_chunk_s3cret}`

---

## Description

PNG files store data in chunks (IHDR, IDAT, IEND, etc.). A custom text chunk called `tEXt` can store arbitrary key-value pairs. What tool would you use to inspect PNG chunks? The flag is hidden in a tEXt chunk of the provided image.

---

## Hints

1. Use pngcheck or a hex editor to inspect the raw PNG chunks. Look for tEXt chunks between IHDR and IDAT.

---

## Solution

### Step 1: Download the PNG File

I downloaded `png_chunks.png` from the challenge page.

### Step 2: Run exiftool (My Method)

The challenge says to use `pngcheck` or a hex editor, but `exiftool` reads PNG metadata too — and it's already installed on Kali!

```bash
exiftool png_chunks.png
```

Output:
```
File Name           : png_chunks.png
File Type           : PNG
Image Width         : 1
Image Height        : 1
Bit Depth           : 8
Color Type          : RGB
Flag                : ctf{png_chunk_s3cret}   ← FLAG IS HERE!
Image Size          : 1x1
Megapixels          : 0.000001
```

The flag was sitting right there in a custom field called `Flag`! 🎯

Got the flag! 🎯

---

## Why This Works

### What are PNG Chunks?

A PNG file is not just an image — it's made up of **chunks** of data stacked on top of each other:

```
PNG File Structure:
┌──────────────────┐
│  PNG Header      │  ← magic bytes that identify the file as PNG
├──────────────────┤
│  IHDR chunk      │  ← image info (width, height, bit depth)
├──────────────────┤
│  tEXt chunk      │  ← custom text! key=value pairs (THIS is where the flag was)
├──────────────────┤
│  IDAT chunk      │  ← the actual compressed pixel data
├──────────────────┤
│  IEND chunk      │  ← marks the end of the PNG file
└──────────────────┘
```

The `tEXt` chunk is a special chunk that lets anyone store **custom key-value pairs** inside a PNG file. Image viewers ignore it — they only care about IHDR and IDAT. But forensics tools like `exiftool` and `pngcheck` can read it!

In this challenge the creator added a custom field:
```
Key:   Flag
Value: ctf{png_chunk_s3cret}
```

Completely invisible when you open the image normally, but readable with the right tools.

---

## Alternative Methods

### Method 1: pngcheck (Kali — shows all chunks)
```bash
pngcheck -c -v png_chunks.png
```

Output shows all chunks including the tEXt chunk with the flag.

### Method 2: Nayuki PNG Chunk Inspector (Online — Easiest)
1. Go to 👉 https://www.nayuki.io/page/png-file-chunk-inspector
2. Upload `png_chunks.png`
3. It lists every chunk in the file
4. Find the `tEXt` chunk → value is `ctf{png_chunk_s3cret}`

### Method 3: strings (Quick check)
```bash
strings png_chunks.png | grep "ctf{"
```

Output:
```
ctf{png_chunk_s3cret}
```

### Method 4: Python (Read chunks directly)
```python
import struct

with open("png_chunks.png", "rb") as f:
    data = f.read()

# Skip 8-byte PNG header
pos = 8
while pos < len(data):
    length = struct.unpack(">I", data[pos:pos+4])[0]
    chunk_type = data[pos+4:pos+8].decode()
    chunk_data = data[pos+8:pos+8+length]
    if chunk_type == "tEXt":
        print(chunk_data.decode(errors="ignore"))
    pos += 12 + length
```

Output:
```
Flag ctf{png_chunk_s3cret}
```

---

## Flag

```
ctf{png_chunk_s3cret}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `exiftool` | Read all metadata including tEXt chunks — flag found instantly! |
| `pngcheck` | Show all PNG chunks verbosely |
| Nayuki PNG Inspector | Online tool to inspect PNG chunks visually |
| `strings` | Quick grep for the flag in raw file |

---

## Key Takeaways

- PNG files have hidden **tEXt chunks** that can store any key-value data — invisible to normal viewers
- `exiftool` reads tEXt chunks and is already on Kali — **always run exiftool first** on any image in a forensics challenge
- `strings | grep "ctf{"` is always the fastest fallback for any file type
- The image was only **1x1 pixel** — a hint that the image content isn't what matters, the metadata is!
- pngcheck and Nayuki's online tool are the dedicated tools for deep PNG chunk analysis
