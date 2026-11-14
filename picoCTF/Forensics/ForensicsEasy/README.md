# ForensicsEasy

**Category:** Forensics  
**Difficulty:** Easy  
**Source:** picoCTF 2021 (Cylab Academy adaptation)

## Challenge

Files can always be changed in a secret way. Can you find the flag?

The challenge provides `cat.jpg` and hints us to inspect the file details.

## Solution

### 1. Identify the file

```bash
file cat.jpg
```

The output identifies it as a JPEG image. This confirms the file type, but it does not reveal all metadata.

### 2. Inspect the metadata

```bash
exiftool cat.jpg
```

Most fields look ordinary, but the `License` field contains an unusual Base64-encoded string:

```text
YWNhZGVteXt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
```

### 3. Decode the value

```bash
echo 'YWNhZGVteXt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9' | base64 -d
```

Decoded output:

```text
academy{the_m3tadata_1s_modified}
```

## Flag

```text
academy{the_m3tadata_1s_modified}
```

## What I learned

- `file` identifies a file's format; it does not inspect every metadata field for hidden content.
- `exiftool` displays image metadata such as license, copyright, and comments.
- Metadata values can contain encoded data.
- Base64 is an encoding, not encryption; decode it with `base64 -d`.

## Key takeaway

For image-forensics challenges, inspect metadata with `exiftool` before assuming the hidden data must be embedded in the image pixels.
