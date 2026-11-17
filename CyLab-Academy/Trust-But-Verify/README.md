# Trust But Verify

**Platform:** CyLab Academy  
**Category:** General Skills / File Analysis  
**Status:** Completed

## Objective

Find the valid file in the `files/` directory and recover the challenge flag using the provided decryption script.

## Initial Information

The challenge environment contained:

- `decrypt.sh` — a script that attempts to decrypt a supplied file.
- `files/` — a directory containing many files with randomly generated names.
- `checksum.txt` — a checksum value supplied with the challenge.

## Approach

Running the script against the directory itself did not work because it expects a file path, not a directory. I needed to pass each file to the script individually.

I used `find` to enumerate all regular files and execute the decryption script on each one. Then I used `grep` to filter the output for the flag prefix.

## Commands and Tools

```bash
find files/ -type f -exec ./decrypt.sh {} \; | grep "academy"
```

- `find files/ -type f` lists regular files under `files/`.
- `-exec ./decrypt.sh {} \;` runs the script once for each discovered file. `{}` is replaced by the current file path.
- `|` sends the command output to `grep`.
- `grep "academy"` displays lines containing the flag prefix.

## Findings

Most candidate files produced a `bad magic number` error because they were not accepted by the decryption script. One file produced a matching flag line.

## Result

```text
academy{trust_but_verify_a906870d}
```

## What I Learned

- A script that expects a file cannot necessarily process a whole directory.
- `find -exec` is useful when the same command must be run against every file.
- Error messages from unsuccessful candidates can appear alongside useful output.
- Piping output to `grep` makes it easier to spot a known flag format.

## Takeaway

When a challenge provides many candidate files and a helper script, first understand what input the script expects. Then automate the repeated file-by-file check and filter for the expected result.
