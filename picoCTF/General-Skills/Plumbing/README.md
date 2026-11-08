# Plumbing

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Medium  
**Status:** Completed

## Challenge objective

The challenge provides a network service that prints many lines of text, most of which are decoys. The goal is to keep the program output and search it for the flag without saving it to a file.

## Solution

I connected to the challenge service with Netcat and piped its output directly into `grep`:

```bash
nc chatelaine.cylabacademy.net 47051 | grep "aca"
```

The command returned the matching line:

```text
academy{digital_plumb3r_ef32328E}
```

## How the command works

- `nc chatelaine.cylabacademy.net 47051` opens a connection to the challenge's TCP service and writes the received text to standard output (`stdout`).
- `|` is a pipe. It connects the first command's standard output to the second command's standard input (`stdin`).
- `grep "aca"` reads the incoming text line by line and prints only lines containing the string `aca`.

The decoy messages did not contain the selected substring, while the flag begins with `academy{`, so the matching line was displayed.

## What I learned

- How standard output from one process can become standard input for another process.
- How to use a pipe to filter live network output.
- How `grep` can reduce a large stream of text to the lines relevant to a search pattern.
- Why intermediate files are not always necessary when processing command output.

## Key takeaway

A shell pipeline can connect small tools into a more useful workflow. Here, Netcat handled the network connection while `grep` handled filtering. Each tool had one clear responsibility.

## Flag

```text
academy{digital_plumb3r_ef32328E}
```
