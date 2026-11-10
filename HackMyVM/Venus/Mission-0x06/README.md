# HackMyVM Venus — Mission 0x06: Locate the Password File

**Platform:** HackMyVM  
**Machine:** Venus  
**Category:** Linux Host Enumeration / Recursive Text Search / File Discovery  
**Status:** Mission objective completed

## Objective

The mission states that the user `luna` left a password in a file somewhere inside the `muack` directory.

## Initial Information

I read the mission instructions:

```bash
cat mission.txt
```

The task identified the target directory as `muack/`, but did not provide the filename.

## Initial Search Attempt

I first tried searching recursively for the literal string `8===`:

```bash
grep -Rni "8===" muack/
```

The command returned no matches. This only establishes that the searched text was not found in the files that grep could read; it does not mean grep failed to enter numeric subdirectories. Directory names being numbers do not prevent recursive searching.

I also tried:

```bash
grep -rni -- "8===" muack/
```

This also returned no matches. The `--` marker ends option parsing; it does not change the search depth or the search pattern.

## Enumerating the Directory Structure

To inspect the files under the target directory, I used:

```bash
find muack/ -maxdepth 3 -type f
```

It returned:

```text
muack/111/111/muack
```

This revealed a file named `muack` nested inside two directories named `111`.

## Reading the Discovered File

I navigated to the containing directory:

```bash
cd muack/111/111/
ls
```

The listing showed:

```text
muack
```

I then tried `cd muack`, but it failed because `muack` was a regular file, not a directory:

```text
-bash: cd: muack: Not a directory
```

I read the file with:

```bash
cat muack
```

The file contained the requested password. Its value is intentionally redacted in this public write-up.

## Why the First grep Search Returned Nothing

- `grep -r` recursively searches files under the specified directory; numeric directory names are not a problem.
- The pattern `8===` is a literal text search. If that exact sequence is not present, grep prints no matches.
- `-n` prints matching line numbers, and `-i` ignores letter case (it has no practical effect for this numeric/symbol-only pattern).
- `find` is useful when the filename is unknown and you need to map the directory tree.
- A path that appears as the final component of a pathname can still be a regular file. Use `find ... -type f` to identify files and `find ... -type d` to identify directories.

## Findings

| Item | Result |
|---|---|
| Target account mentioned in the mission | `luna` |
| Target directory | `muack/` |
| Discovered file | `muack/111/111/muack` |
| File type | Regular file |
| Password | Recovered; value redacted |
| Account login | Not tested or claimed |

## Result

The file containing the requested password was located and read. This completes the file-discovery objective for Mission 0x06.

## What I Learned

- How to use `grep -r` for recursive content searching.
- Numeric subdirectory names do not prevent recursive traversal.
- A no-match result is different from a traversal or permission error.
- How to use `find` with `-type f` and `-maxdepth` to inspect a directory tree.
- The difference between a regular file and a directory.
- How to read a file whose name is known from the current directory.

## Takeaway

When a recursive text search returns no matches, do not assume that nested directories were skipped. Verify the directory structure with `find`, identify candidate files, and inspect the relevant file. Keep confirmed file discovery separate from any account login that has not been tested.
