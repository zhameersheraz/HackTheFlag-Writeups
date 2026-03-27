# Steganography Intro — Hack the Flag Writeup

**Challenge:** Steganography Intro
**Category:** Forensics
**Difficulty:** Easy
**Points:** 100 pts
**Flag:** `ctf{st3g0_b3ginn3r}`

---

## Description

There is a message hidden in the image file. The text is written in white on a white background. Try selecting all the text or adjusting the colors to reveal it!

---

## Hints

> Try selecting all text on the page (Ctrl+A), or open the image in an editor and adjust brightness/contrast.

---

## Solution

### Step 1: Download the file

```bash
wget <challenge-file-url> -O steganography.png
```

### Step 2: Run exiftool to inspect metadata

```bash
exiftool steganography.png
```

### Output:

```
ExifTool Version Number         : 13.36
File Name                       : steganography.png
File Type                       : PNG
Image Width                     : 500
Image Height                    : 200
Warning                         : [minor] Text/EXIF chunk(s) found after PNG IDAT
Flag                            : ctf{st3g0_b3ginn3r}
```

The flag is stored directly in the PNG's custom text chunk — visible through `exiftool` as the `Flag` field.

### Step 3 (Alternative): Use strings

```bash
strings steganography.png | grep ctf
```

---

## What is Steganography?

Steganography is the practice of **hiding data within other data**. Here, the flag was embedded as metadata inside the PNG file structure — in a tEXt or iTXt chunk after the image data. Most viewers ignore these chunks, making the image look blank.

---

## Flag

```
ctf{st3g0_b3ginn3r}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `exiftool` | Reads all metadata from image files |
| `strings` | Extracts printable text from binary files |

---

## Key Takeaways

- PNG files contain hidden **text chunks** (`tEXt`, `iTXt`) invisible when viewing the image
- `exiftool` is your first tool for any forensics challenge involving images
- Always run `strings` and `exiftool` before going deeper
