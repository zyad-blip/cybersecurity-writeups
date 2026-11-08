# Linux CLI Fundamentals

**Type:** Practical learning notes  
**Status:** Practiced

These are command-line skills practiced during beginner Linux exercises and CTF tasks. This page documents the tools and techniques, not a claim that each command represents a separate completed room.

## File inspection

### file

Identify a file's type from its contents rather than relying only on its extension.

```bash
file <filename>
```

### strings

Display readable text sequences found inside a file. This can be useful when inspecting unknown or binary files.

```bash
strings <filename>
```

## Searching text

### grep

Search for matching text in a file:

```bash
grep -n "pattern" <filename>
```

Search recursively through a directory:

```bash
grep -Rni "pattern" .
```

Useful options:

- `-R`: search recursively
- `-n`: show line numbers
- `-i`: ignore case differences

The Big Zip challenge is a practical example of using recursive `grep`: [Big Zip write-up](../../General-Skills/Big-Zip/README.md).

## Network connections

### Netcat (`nc`)

Netcat is a networking utility, not a file-inspection command. In an authorized lab, it can connect to a known host and port to inspect a service response.

```bash
nc -nv <authorized-target> <port>
```

## A repeatable first-look workflow

When a lab provides an unfamiliar file:

1. Run `file` to identify its type.
2. Use `strings` to look for readable embedded text.
3. Use `grep` when searching known text patterns across files.
4. Use Netcat only when the task involves an authorized network service.

## Key lesson

The tools are most useful when chosen for a specific question. Identify the file first, inspect its contents, then search or connect only when the evidence calls for it.
