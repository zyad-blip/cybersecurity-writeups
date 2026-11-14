# Glory of the Garden

**Platform:** picoCTF-style challenge  
**Category:** Forensics  
**Difficulty:** Easy

## Challenge

We are given a JPEG image named `garden.jpg`. The goal is to find information hidden in the file.

## Tools Used

- `file`
- `xxd`
- `tail`
- `strings`
- `grep`

## Solution

### 1. Identify the file type

I checked the file:

```bash
file garden.jpg
```

The output identified it as a JPEG image.

### 2. Inspect the file contents

I created a hexadecimal dump:

```bash
xxd garden.jpg > hex.txt
```

Then inspected the end of the dump:

```bash
tail hex.txt
```

At the end, I noticed the JPEG end marker `FFD9`, followed by additional readable bytes.

### 3. Read the hidden text

The ASCII column showed:

```text
Here is a flag: academy{more_than_m33ts_the_3y3ee53c6bc}
```

A quicker way to search for readable text containing the word "flag" is:

```bash
strings garden.jpg | grep -i flag
```

## Flag

```text
academy{more_than_m33ts_the_3y3ee53c6bc}
```

## What I Learned

- `file` identifies a file's type, but does not guarantee that it contains no extra data.
- `xxd` displays binary data in hexadecimal and ASCII.
- `strings` extracts readable text from binary files.
- The JPEG end marker is `FFD9`; additional data can still be appended after it.
