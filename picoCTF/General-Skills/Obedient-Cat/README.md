# Obedient Cat

**Platform:** picoCTF 2021  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge provides a file named `flag`. Its contents are already readable text; there is no special encoding or file format to decode.

## Solution approach

Download the attachment named `flag` from the challenge page and move into the directory where it was saved.

Confirm that the file exists:

```bash
ls -l flag
file flag
```

Print its contents:

```bash
cat flag
```

The terminal displays the flag directly.

## Why this works

The file contains plain text. The `cat` command reads the file and writes its contents to standard output. A file does not need a familiar extension for `cat` to display readable text.

## What this challenge teaches

- Downloading and locating a challenge attachment.
- Understanding that a filename extension is not required to read a text file.
- Using `cat` to display file contents.
- Starting with the simplest inspection method before trying more complicated tools.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [picoCTF 2021 — Obedient Cat walkthrough](https://picoctfsolutions.com/picoctf-2021-obedient-cat)
