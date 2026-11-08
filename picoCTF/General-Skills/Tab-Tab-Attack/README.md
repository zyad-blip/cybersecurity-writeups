# Tab, Tab, Attack

**Platform:** picoCTF 2021  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge provides an archive named `Addadshashanammu.zip`. Its nested directories have long names, making terminal tab completion the intended navigation skill.

## Solution approach

Extract the archive:

```bash
unzip Addadshashanammu.zip
```

Move into the extracted directory:

```bash
cd Addadshashanammu
```

Continue navigating through the nested directories. Type `cd ` and press **Tab** to complete the next directory name. Repeat until reaching the file named:

```text
fang-of-haynekhtnamet
```

The file is not meant to be read as ordinary text. Identify it and inspect its printable strings:

```bash
file fang-of-haynekhtnamet
strings fang-of-haynekhtnamet | grep 'picoCTF{'
```

The matching line reveals the flag.

## Why this works

- `unzip` extracts the directory tree from the archive.
- Shell tab completion fills in a unique matching path component, reducing typing mistakes in long directory names.
- `file` identifies the type of the final artifact.
- `strings` extracts readable character sequences from the binary without executing it.
- `grep` filters that output to the picoCTF flag pattern.

## What this challenge teaches

- Extracting ZIP archives.
- Navigating deeply nested directory structures.
- Using shell tab completion efficiently.
- Recognizing a binary file and extracting embedded text without running it.
- Combining commands with a pipe.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [Community write-up — Tab, Tab, Attack](https://ahmedheltaher.github.io/ctf-writeups/sites/picoCTF/General-Skills/Tab-Tab-Attack.html)
- [DMFR Security — picoCTF 2021 Tab, Tab, Attack](https://dmfrsecurity.com/2021/04/19/picoctf-2021-tab-tab-attack-writeup/)
