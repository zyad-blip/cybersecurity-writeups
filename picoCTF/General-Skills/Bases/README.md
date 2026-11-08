# Bases

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge presents an encoded string and asks what it represents. The task is to recognize the encoding and decode it.

## Solution approach

The supplied string is:

```text
bDNhcm5fdGgzX3IwcDM1
```

Its character set and length are consistent with Base64. Decode it in the terminal:

```bash
echo 'bDNhcm5fdGgzX3IwcDM1' | base64 -d
```

The decoded text gives the challenge flag.

## Why this works

Base64 represents binary data using a printable alphabet containing uppercase and lowercase letters, digits, `+`, `/`, and optional `=` padding.

The `base64 -d` option reverses the encoding and returns the original bytes. Base64 is an encoding scheme, not encryption: it does not require a secret key.

## What this challenge teaches

- Recognizing a Base64-looking string.
- Decoding Base64 from the Linux command line.
- Understanding the difference between encoding and encryption.
- Using a pipe to pass text directly into a decoder.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [picoCTF 2019 — Bases walkthrough](https://picoctfsolutions.com/picoctf-2019-bases)
