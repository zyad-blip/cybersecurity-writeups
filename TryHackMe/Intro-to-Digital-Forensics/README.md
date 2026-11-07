# TryHackMe - Intro to Digital Forensics

## 1. Introduction

In this room, I learned the basics of Digital Forensics and how to extract useful information from digital files.

The scenario was about a kidnapped cat named Gado. The kidnapper sent a ransom letter with an attached image.

My goal was to investigate the files and find useful information from their metadata.

## 2. Tools Used

- `pdfinfo`
- `exiftool`
- Google Maps
- Linux Terminal

## 3. Investigation

### Step 1: PDF Metadata Analysis

First, I used `pdfinfo` to check the metadata of the ransom letter.

```bash
pdfinfo ransom-letter.pdf
```

I found some interesting information:

- Title: Pay NOW
- Subject: We Have Gato
- Author: Ann Gree Shepherd
- Creator: Microsoft Word 2016
- Creation Date: February 23, 2022

The Author field gave me a possible lead, but it doesn't prove who actually created the document.

### Step 2: Image Metadata Analysis

Next, I checked the attached image using ExifTool.

```bash
exiftool letter-image.jpg
```

I found several useful details:

- Camera Make: Canon
- Camera Model: Canon EOS R6
- Editing Software: GIMP 2.10.28
- Original Filename: 5L0A2971.CR3

I also found GPS coordinates inside the image metadata.

### Step 3: Extracting GPS Coordinates

Instead of reading all the metadata again, I used:

```bash
exiftool -GPSPosition letter-image.jpg
```

The result was:

```text
51 deg 30' 51.90" N
0 deg 5' 38.73" W
```

I converted the coordinates to decimal format and opened them in Google Maps.

The location pointed to **Milk Street**.

### Step 4: Finding the Camera Model

I checked the camera information in the metadata.

The camera model was:

**Canon EOS R6**

## 4. Final Findings

| Question | Finding |
|---|---|
| PDF Author | Ann Gree Shepherd |
| Camera Model | Canon EOS R6 |
| Street Name | Milk Street |
| Editing Software | GIMP 2.10.28 |

## 5. What I Learned

During this room, I learned:

- How to extract metadata from PDF files using `pdfinfo`.
- How to analyze image metadata using `exiftool`.
- How to extract GPS coordinates from an image.
- How to use Google Maps to identify a location.
- How metadata can provide useful clues during a digital investigation.

## 6. Conclusion

This room helped me understand that files can contain more information than what we see when we open them.

Metadata can give us useful clues about files, cameras, dates, software, and locations.

**Room completed: Intro to Digital Forensics.**
