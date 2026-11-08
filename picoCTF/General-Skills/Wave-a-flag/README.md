# Wave a flag

**Platform:** picoCTF 2021  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge provides a Linux executable named `warm` and hints that the program has useful information available through a help option.

## Solution approach

First, identify the downloaded file and inspect its permissions:

```bash
file warm
ls -l warm
```

If the execute permission is missing, add it:

```bash
chmod +x warm
```

Run the program once to observe its message:

```bash
./warm
```

The program points to the short help option. Run:

```bash
./warm -h
```

The help output contains the challenge flag.

## Why this works

- `file` identifies the file format; in this challenge, the attachment is a compiled Linux executable.
- `chmod +x` adds execute permission so Linux can run the program.
- `./warm -h` passes `-h` as the first command-line argument. The program handles that option by printing its helpful output, including the flag.

Use the exact option requested by the program. A short `-h` option and a long `--help` option are not guaranteed to behave the same way.

## What this challenge teaches

- Linux execute permissions.
- The difference between a binary executable and a shell script.
- How command-line arguments are passed to a program.
- Why challenge descriptions often contain useful hints.

## Safety note

Only execute files supplied by a trusted, authorized CTF environment. For an unknown executable from an untrusted source, inspect it first and use an isolated lab.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided. Public copies can show different flag instances.

## References

- [picoCTF 2021 — Wave a flag walkthrough](https://picoctfsolutions.com/picoctf-2021-wave-a-flag)
- [Community write-up](https://ctftime.org/writeup/28150)
