# Static ain't always noise

- **Platform:** picoCTF 2021
- **Category:** General Skills
- **Difficulty:** Easy
- **Status:** Completed

## Challenge objective

Inspect the provided Linux binary named `static`. The challenge also provides a Bash script, `ltdis.sh`, that helps produce a disassembly and extract printable strings with their file offsets.

## Investigation

### 1. Identify the file type

The `file` command was initially unavailable, so it was installed:

```bash
sudo apt install file
file static
```

The output identified `static` as an ELF 64-bit LSB PIE executable for x86-64 Linux. It was dynamically linked and not stripped.

This established that the supplied file was a Linux executable rather than an ordinary text file.

### 2. Extract printable strings

```bash
strings static
```

This displayed readable character sequences found in the binary. Filtering the output made the challenge flag easy to locate:

```bash
strings static | grep "academy"
```

The command pipeline sends the output of `strings` to `grep`, which prints matching lines.

### 3. Inspect the helper script

Reading `ltdis.sh` showed that it performs two main tasks:

- Runs `objdump` to disassemble the executable's `.text` section.
- Runs `strings` with options that scan the file and print each string's hexadecimal file offset.

The key commands in the script are:

```bash
objdump -Dj .text "$1" > "$1.ltdis.x86_64.txt"
strings -a -t x "$1" > "$1.ltdis.strings.txt"
```

Here, `$1` is the first argument passed to the script (the input filename). The `>` operator redirects command output into a file.

The script checks whether the disassembly output file exists and is non-empty before reporting success.

### 4. Run the helper script

```bash
chmod +x ltdis.sh
./ltdis.sh static
ls
```

The script created two output files:

- `static.ltdis.x86_64.txt` — disassembled instructions from the `.text` section.
- `static.ltdis.strings.txt` — printable strings with hexadecimal file offsets.

To inspect a small sample of each file:

```bash
head -n 15 static.ltdis.x86_64.txt
head -n 15 static.ltdis.strings.txt
```

The disassembly output contains addresses, machine-code bytes, and corresponding Assembly instructions. The strings output contains a file offset followed by the readable string. For example, the flag appeared at offset `3020` (hexadecimal) in this file.

## Result

The flag was found using printable string extraction:

```text
academy{d15a5m_t34s3r_fac3431a}
```

## What I learned

- Use `file` to identify an unfamiliar file before deciding how to inspect it.
- Use `strings` to extract readable text from a binary.
- Use `grep` to filter command output for a keyword.
- Use `objdump` to disassemble executable code into Assembly.
- Use `strings -a -t x` to scan a file and display the hexadecimal file offset of each extracted string.
- A Bash script can automate several analysis commands and save their output for later inspection.

## Key distinction

- **Disassembly:** presents machine instructions in a human-readable Assembly form.
- **String extraction:** finds readable text embedded in the binary; it does not explain the program's full logic.

This was a basic introduction to static binary analysis. The executable was inspected without needing to run it.
