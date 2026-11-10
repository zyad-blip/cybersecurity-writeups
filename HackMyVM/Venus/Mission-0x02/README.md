# HackMyVM Venus — Mission 0x02: Find the Saved Password

**Platform:** HackMyVM  
**Machine:** Venus  
**Category:** Linux Host Enumeration / File Discovery  
**Status:** Mission objective completed

## Objective

The mission states that the user `angela` saved a password in a file but no longer remembers its location. The only known detail is the filename:

```text
whereismypazz.txt
```

The task is to locate the file and read its contents from the current shell session as `sophia`.

## Initial Information

The mission instructions were read from:

```bash
cat mission.txt
```

The shell prompt showed that the current user was `sophia` on the `venus` machine.

## Approach

I used `find` to search from the filesystem root for a regular file with the exact filename. Permission errors were redirected to `/dev/null` so that the matching path would be easier to see.

```bash
find / -type f -name "whereismypazz.txt" 2>/dev/null
```

The command returned:

```text
/usr/share/whereismypazz.txt
```

I then read the file:

```bash
cat /usr/share/whereismypazz.txt
```

The file contained a password. Its value is intentionally redacted in this public write-up.

## Understanding the Commands

- `find /` starts a recursive search from the filesystem root.
- `-type f` limits the search to regular files.
- `-name "whereismypazz.txt"` matches the requested filename.
- `2>/dev/null` redirects standard error (file descriptor 2) to `/dev/null`, suppressing error messages without changing the search permissions or results.
- `cat` prints the contents of the located file.

### What happened when stderr was not redirected?

Running the search without `2>/dev/null` displayed messages such as:

```text
find: '/proc/...': Permission denied
```

These messages indicate that the current user could not inspect some protected entries under `/proc`. They do not mean that the entire search failed. The matching file was still found.

## Findings

| Item | Result |
|---|---|
| Current user | `sophia` |
| Target account mentioned in the mission | `angela` |
| File located | `/usr/share/whereismypazz.txt` |
| File contents | Password recovered; value redacted |
| Privilege escalation | Not performed or demonstrated |

## Result

The requested file was successfully located and read. This completes the file-discovery objective of Mission 0x02.

**Scope note:** The mission output provided here confirms that the password was recovered from the file. It does not show a subsequent login as `angela`, so account access is not claimed as verified.

## What I Learned

- How to search the filesystem recursively with `find`.
- How to combine `-type` and `-name` to narrow a search.
- The difference between standard output (`stdout`) and standard error (`stderr`).
- Why `2>/dev/null` hides errors but does not bypass permissions.
- How to inspect a discovered file with `cat`.
- Why enumeration findings should be recorded separately from actions that have not been tested.

## Takeaway

A useful enumeration workflow is to search for a known filename, record the exact path, inspect the file only when permitted, and distinguish confirmed findings from assumptions. Suppressing noisy errors can make output easier to read, but it does not grant additional access.
