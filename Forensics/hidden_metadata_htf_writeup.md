# Hidden Metadata — Hack the Flag Writeup

**Challenge:** Hidden Metadata  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{m3tadata_tells_secrets}`  

---

## Description

Download the image file. The flag is hidden in the EXIF metadata. Sometimes photographers leave more than just photos in their images.

---

## Hints

> Use an EXIF viewer or the `exiftool` command to inspect image metadata.

---

## Solution

### Step 1: Download the file

```bash
wget <challenge-file-url> -O hidden_metadata.jpg
```

### Step 2: Run exiftool

```bash
exiftool hidden_metadata.jpg
```

### Output:

```
ExifTool Version Number         : 13.36
File Name                       : hidden_metadata.jpg
File Size                       : 47 kB
File Type                       : JPEG
Image Description               : ctf{m3tadata_tells_secrets}
Make                            : CTF Camera Corp
Camera Model Name               : SecretCam X1
Software                        : Adobe Photoshop CS6
Artist                          : ctf{m3tadata_tells_secrets}
Copyright                       : Flag: ctf{m3tadata_tells_secrets}
Image Width                     : 600
Image Height                    : 400
```

The flag appears in three EXIF fields: `Image Description`, `Artist`, and `Copyright`.

### Step 3: Target a specific field (optional)

```bash
exiftool -Artist hidden_metadata.jpg
exiftool -Copyright hidden_metadata.jpg
```

---

## What is EXIF Metadata?

**EXIF** (Exchangeable Image File Format) embeds metadata into image files. It was designed to store camera settings, but also allows custom text fields like `Artist`, `Copyright`, and `ImageDescription` — which can store anything, including flags.

---

## Flag

```
ctf{m3tadata_tells_secrets}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `exiftool` | Gold standard for reading/writing image metadata |
| `exif.tools` | Web-based EXIF viewer (no install needed) |

---

## Key Takeaways

- EXIF metadata is invisible to the eye but readable with tools
- `exiftool` works on JPEG, PNG, TIFF, PDF, and many other formats
- In real digital forensics, EXIF can reveal GPS location, device serial numbers, and editing history
