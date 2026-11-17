# Hidden in Plain Sight — Steganography Write-up

## Challenge Information

- **Platform:** CyLab Academy
- **Category:** Forensics
- **Difficulty:** Easy
- **Author:** Yahaya Meddy
- **Description:** A seemingly ordinary JPG contains a hidden payload. The goal is to discover and extract the flag.

## Objective

Inspect the supplied JPEG, identify the clue that points to the steganography tool, and extract the hidden flag.

## 1. Inspect the image metadata

Run ExifTool:

```bash
exiftool img.jpg
```

The image metadata contains an unusual `Comment` value:

```text
c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

## 2. Decode the Base64 clue

Decode the comment:

```bash
echo "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9" | base64 -d
```

Output:

```text
steghide:cEF6endvcmQ=
```

The text identifies **steghide** as the tool. The value after the colon is another Base64 string, so decode that part separately:

```bash
echo "cEF6endvcmQ=" | base64 -d
```

Output:

```text
pAzzword
```

This gives the passphrase for the hidden payload.

## 3. Check for embedded data

Run:

```bash
steghide info img.jpg
```

The tool reports an embedded file named `flag.txt`, with a size of 34 bytes. It is encrypted and compressed.

## 4. Extract the hidden file

Use the recovered passphrase:

```bash
steghide extract -sf img.jpg -p 'pAzzword'
```

Steghide confirms that it wrote the extracted data to `flag.txt`.

Read the file:

```bash
cat flag.txt
```

## Flag

```text
academy{h1dd3n_1n_1m4g3_a2158aa5}
```

## Tools Used

- `exiftool` — inspect image metadata.
- `base64` — decode the embedded clue and passphrase.
- `steghide` — inspect and extract hidden data from the JPEG.
- `cat` — read the extracted text file.

## Lessons Learned

- Metadata can contain clues that are not visible when viewing an image normally.
- A Base64 value may contain another encoded value; inspect the decoded output before assuming the decoding is finished.
- `steghide info` can reveal whether a supported image contains embedded data.
- Use the exact filename that exists in the current directory. In this solve, the image was named `img.jpg`, not `image.jpg`.
- A clean result from `binwalk` does not rule out steganography. Data embedded with a tool such as Steghide may not appear as a separate appended file.

## Takeaway

The key was to inspect the JPEG comment, decode the tool and passphrase clue, then use Steghide to extract the hidden payload.
