# strings it

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge provides a file named `strings` and asks for the flag without executing that file.

## Solution approach

First, identify the downloaded file:

```bash
file strings
```

Use the `strings` utility to extract printable text, then filter the result for the flag prefix:

```bash
strings strings | grep 'picoCTF{'
```

The matching line contains the flag.

## Why this works

Compiled binaries can contain readable text such as messages, labels, and hard-coded strings. The `strings` utility scans the file's bytes and prints sequences of printable characters.

The pipe (`|`) sends that output to `grep`, which keeps only lines matching the requested pattern.

This approach inspects the file as data; it does not execute the binary.

## What this challenge teaches

- The purpose of the `strings` utility.
- The difference between inspecting a binary and running it.
- Filtering command output with `grep`.
- Combining utilities into a simple shell pipeline.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [picoCTF 2019 — strings it walkthrough](https://picoctfsolutions.com/picoctf-2019-strings-it)
- [Community write-up — strings it](https://ahmedheltaher.github.io/ctf-writeups/sites/picoCTF/General-Skills/strings-it.html)
