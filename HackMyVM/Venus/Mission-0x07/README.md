# HackMyVM Venus — Mission 0x07: Find a File by Exact Size

**Platform:** HackMyVM  
**Machine:** Venus  
**Category:** Linux Host Enumeration / File Discovery / `find`  
**Status:** File located and read

## Objective

The mission states that the user `eleanor` left her password in a file whose size is exactly **6969 bytes**.

## Initial Information

I read the mission instructions:

```bash
cat mission.txt
```

The clue gives the file size, but not its name or location.

## Search Attempts

My first command was:

```bash
find / -type f -size 6969 2>dev//null/
```

This contained a redirection typo: `2>dev//null/` is treated as a relative path named `dev//null/`, which does not exist. The correct special device path is `/dev/null`.

I then tried:

```bash
find / -type f -size 6969 2>/dev//null
```

This still had the path written incorrectly. The correct path is `/dev/null`, with a slash between `dev` and `null`.

## Correct Enumeration

I first ran:

```bash
find / -type f -size 6969 2>/dev/null
```

This returned no results because, without a unit suffix, `find -size` measures file size in 512-byte blocks (rounded up), not exact bytes.

I then specified the byte unit with `c`:

```bash
find / -type f -size 6969c 2>/dev/null
```

The command returned:

```text
/usr/share/moon.txt
```

## Reading the File

I read the discovered file:

```bash
cat /usr/share/moon.txt
```

The file contained the password requested by the mission. Its value is intentionally redacted in this public write-up.

## Command Breakdown

- `find /`: search starting from the filesystem root.
- `-type f`: match regular files only.
- `-size 6969c`: match files whose size is exactly 6969 bytes; `c` means bytes.
- `2>/dev/null`: redirect standard error (file descriptor 2), such as permission-denied messages, to `/dev/null`.

**Important distinction:** `2>/dev/null` suppresses error output; it does not grant additional permissions or make inaccessible files readable.

## Findings

| Item | Result |
|---|---|
| Target account mentioned in the mission | `eleanor` |
| Required file size | 6969 bytes |
| Discovered file | `/usr/share/moon.txt` |
| File contents | Password recovered; value redacted |
| Account login | Not tested or claimed |

## Result

The file matching the exact byte size was found and read. This completes the file-discovery objective for Mission 0x07.

## What I Learned

- How to use `find` to search the filesystem from `/`.
- How `-type f` limits results to regular files.
- Why the `c` suffix matters when searching for an exact byte count.
- How to redirect standard error to `/dev/null`.
- How to check command syntax carefully when a search returns no results.

## Takeaway

When a task gives an exact file size in bytes, use `find -size <number>c`. Do not omit the `c` suffix, because the default size unit is 512-byte blocks rather than bytes.
