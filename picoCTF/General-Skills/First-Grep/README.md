# First Grep

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge provides a large text file and hints that reading or printing the entire file is unnecessary. The goal is to locate the flag efficiently.

## Solution approach

Download the challenge attachment named `file` and move into its directory.

Search for the picoCTF flag prefix:

```bash
grep 'picoCTF{' file
```

The matching line is printed without displaying the unrelated contents of the file.

## Why this works

`grep` reads the file and prints lines that match the supplied pattern. Since the flag uses a recognizable prefix, searching for that prefix is faster and more reliable than manually scrolling through a large file.

An equivalent command can use a shorter pattern:

```bash
grep 'picoCTF' file
```

## What this challenge teaches

- Searching text files with `grep`.
- Using a known string as a search pattern.
- Avoiding unnecessary terminal output.
- Recognizing when a focused search is more useful than reading an entire file.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [picoCTF 2019 — First Grep walkthrough](https://picoctfsolutions.com/picoctf-2019-first-grep)
- [Community write-up — First Grep](https://yorzaren.github.io/ctf/picoctf-2019/first-grep/)
