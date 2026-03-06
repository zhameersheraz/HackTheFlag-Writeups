# Geolocation Challenge - Hack the Flag Writeup

**Challenge:** Geolocation Challenge  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{eiffel_tower}`

---

## Description

A photo was taken at a famous landmark. Download the image and check its EXIF metadata for GPS coordinates. Identify the landmark and submit as `ctf{landmark_in_lowercase_with_underscores}`.

---

## Hints

1. Search the GPS coordinates on a map service to find the exact location.

---

## Solution

### Step 1: Download the Challenge File

Download the provided image `geolocation.jpg` from the challenge page.

### Step 2: Extract EXIF Metadata (My Method)

EXIF metadata is hidden data embedded inside image files — it can contain camera info, timestamps, and even GPS coordinates.

I used **ExifTool** on Kali Linux to extract everything:

```bash
exiftool geolocation.jpg
```

Output (key parts):

```
Image Description   : A famous landmark in Paris
GPS Latitude        : 48 deg 51' 30.24" N
GPS Longitude       : 2 deg 17' 40.20" E
GPS Position        : 48 deg 51' 30.24" N, 2 deg 17' 40.20" E
```

The image gave it away in **two ways**:
- The `Image Description` field literally says **"A famous landmark in Paris"**
- The GPS coordinates point to a very specific location in Paris

### Step 3: Look Up the GPS Coordinates

I searched the coordinates on Google Maps:

```
48°51'30.24"N, 2°17'40.20"E
```

Or in decimal format:
```
48.8584°N, 2.2945°E
```

👉 This is the exact location of the **Eiffel Tower** in Paris, France!

### Step 4: Format the Flag

The challenge says to submit as `ctf{landmark_in_lowercase_with_underscores}`:

```
Eiffel Tower → eiffel_tower
```

Got the flag! 🎯

---

## Why This Works

### What is EXIF Metadata?

EXIF (Exchangeable Image File Format) is invisible data automatically saved inside photos. It records things like:

| EXIF Field | What it stores |
|------------|---------------|
| GPS Latitude / Longitude | Exact location where the photo was taken |
| Image Description | A text description added by the creator |
| Camera Model | What camera was used |
| Artist | Who took the photo |
| Software | What software processed the image |

In this challenge, the creator left GPS coordinates **and** a description inside the image — both pointing to Paris.

### Converting GPS Coordinates

The EXIF data stores GPS in degrees/minutes/seconds (DMS) format:
```
48 deg 51' 30.24" N  →  48 + (51/60) + (30.24/3600)  =  48.8584°N
 2 deg 17' 40.20" E  →   2 + (17/60) + (40.20/3600)  =   2.2945°E
```

The known coordinates of the Eiffel Tower are **48.8582°N, 2.2945°E** — a perfect match! ✅

---

## Alternative Methods

### Method 1: Jeffrey's Exif Viewer (No install needed)
1. Go to 👉 https://exifdata.com
2. Upload `geolocation.jpg`
3. Look for **GPS Latitude** and **GPS Longitude**
4. Copy the coordinates and paste into Google Maps

### Method 2: ExifMeta Online
1. Go to 👉 https://www.exifmeta.com
2. Upload the image
3. Scroll to the GPS section
4. Coordinates are shown — click to open in maps

### Method 3: CyberChef
1. Go to 👉 https://gchq.github.io/CyberChef/
2. Upload the image as input
3. Use the **"Extract EXIF"** recipe
4. Look for GPS fields in the output

### Method 4: Windows (No tools needed!)
1. Right-click `geolocation.jpg`
2. Click **Properties → Details**
3. Scroll down to the **GPS** section
4. Copy the coordinates into Google Maps

---

## Flag

```
ctf{eiffel_tower}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| ExifTool (Kali Linux) | Extract EXIF metadata from the image |
| Google Maps | Look up GPS coordinates to identify the landmark |
| exifdata.com | Online alternative to ExifTool |
| ExifMeta | Another online EXIF viewer |

---

## Key Takeaways

- **EXIF metadata** is hidden data embedded inside images — always check it in OSINT challenges
- GPS coordinates inside a photo can reveal exactly where it was taken
- `exiftool` is the fastest way to extract EXIF data on Linux
- Always check fields like `Image Description`, `Artist`, and `Comment` — CTF makers sometimes leave extra hints there
- Windows users can check EXIF data without any tools via right-click → Properties → Details
