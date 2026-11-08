# Big Zip — CTF Write-up

**Category:** General Skills  
**Technique:** Recursive file search with `grep`

## Challenge Overview

The challenge provided a ZIP archive containing a large number of files inside nested directories. Inspecting every file manually would be inefficient, so I used recursive text search to look for the flag pattern.

## Solution

After extracting the archive and moving into the extracted directory, I ran:

```bash
grep -Rni "academy{" .
```

### What the options mean

- `-R`: Search recursively through subdirectories.
- `-n`: Show the matching line number.
- `-i`: Ignore uppercase/lowercase differences.
- `.`: Start searching from the current directory.

The command returned a matching file and showed the flag in its first line.

## Flag

```text
academy{gr3p_15_m4g1c_ef8790dc}
```

## What I Learned

- How to search through nested directories without opening files one by one.
- How to combine `grep` options for recursive searching.
- Why understanding command-line tools is useful when investigating large collections of files.

## Takeaway

A focused recursive search can turn a tedious manual inspection task into a quick, repeatable command.
