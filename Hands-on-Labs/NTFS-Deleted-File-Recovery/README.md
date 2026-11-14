# NTFS Deleted File Recovery — Forensic Writeup

## Challenge Summary

We are given a disk image (`evidence.img`) containing a DOS/MBR partition table and a single NTFS partition. The goal is to recover a deleted file and extract the flag.

The deleted file was:

```text
Users/j.morris/Documents/recovery_code.gz
```

After recovering and decompressing it, the content was ROT13-encoded. Decoding it revealed the flag.

## Tools Used

- `file`
- `exiftool`
- `binwalk`
- `mmls`
- `fdisk`
- `fsstat`
- `fls`
- `icat`
- `gzip`
- `tr`
- `xxd`
- `base64` (for decoy analysis)

Install Sleuth Kit and gzip on Debian/Ubuntu/WSL:

```bash
sudo apt update
sudo apt install -y sleuthkit gzip
```

## Evidence Overview

Identify the disk image:

```bash
file evidence.img
```

Output:

```text
evidence.img: DOS/MBR boot sector; partition 1 : ID=0x7, start-CHS (0x0,32,33), end-CHS (0x1,135,6), startsector 2048, 22528 sectors, extended partition table (last)
```

The image is approximately 12 MiB and contains one partition starting at sector `2048`.

Inspect the partition table:

```bash
mmls evidence.img
```

Relevant output:

```text
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000024575   0000022528   NTFS / exFAT (0x07)
```

The NTFS partition starts at sector `2048`. Sleuth Kit commands therefore use the partition offset:

```bash
-o 2048
```

Inspect filesystem metadata:

```bash
fsstat -o 2048 evidence.img
```

Key details:

```text
File System Type: NTFS
Volume Name: RECOVERY
Cluster Size: 4096
Sector Size: 512
Root Directory: 5
```

## Listing Files, Including Deleted Files

List the filesystem recursively:

```bash
fls -r -o 2048 evidence.img
```

Relevant output:

```text
d/d 64-144-2:   Users
+ d/d 65-144-2: j.morris
++ d/d 66-144-2: Documents
+++ r/r 70-128-2: report_q3.txt
+++ r/r 73-128-2: wifi_backup.txt
+++ -/r * 75-128-2: recovery_code.gz
```

The `*` marker indicates a deleted entry. The interesting file is:

```text
-/r * 75-128-2: Users/j.morris/Documents/recovery_code.gz
```

Confirm deleted entries only:

```bash
fls -r -d -o 2048 evidence.img
```

Relevant output:

```text
-/r * 75-128-2: Users/j.morris/Documents/recovery_code.gz
```

The file's metadata address is:

```text
75-128-2
```

## Recovering the Deleted File

Create an extraction directory:

```bash
mkdir -p extract
```

Recover the deleted file with `icat`:

```bash
icat -o 2048 evidence.img 75-128-2 > extract/recovery_code.gz
```

Verify the recovered file:

```bash
file extract/recovery_code.gz
```

Output:

```text
extract/recovery_code.gz: gzip compressed data, from Unix, original size modulo 2^32 26
```

Check its gzip metadata:

```bash
gzip -l extract/recovery_code.gz
```

Output:

```text
         compressed        uncompressed  ratio uncompressed_name
                 46                  26  -7.7% recovery_code
```

Decompress it to a different file:

```bash
gzip -dc extract/recovery_code.gz > extract/recovery_code
```

View the decompressed content:

```bash
cat extract/recovery_code
```

Output:

```text
VaVg{q3y3g3q_ohg_a0g_t0a3}
```

Hex dump:

```bash
xxd extract/recovery_code
```

Output:

```text
00000000: 5661 5667 7b71 3379 3367 3371 5f6f 6867  VaVg{q3y3g3q_ohg
00000010: 5f61 3067 5f74 3061 337d                 _a0g_t0a3}
```

## Decoding the Recovered Text

The recovered string is encoded with ROT13. Decode it with `tr`:

```bash
echo 'VaVg{q3y3g3q_ohg_a0g_t0a3}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Output:

```text
InIt{d3l3t3d_but_n0t_g0n3}
```

## One-Liner Recovery

The recovery, decompression, and ROT13 decoding can be chained together:

```bash
icat -o 2048 evidence.img 75-128-2 | gzip -dc | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Output:

```text
InIt{d3l3t3d_but_n0t_g0n3}
```

## Flag

```text
InIt{d3l3t3d_but_n0t_g0n3}
```

## Decoys and Pitfalls

Several files in the image were decoys or were extracted with the wrong metadata address during manual exploration.

### `wifi_backup_recovered.txt`

Recovered content:

```text
bXktd2lmaS1wYXNzd29yZA==
```

Base64 decoding:

```bash
echo 'bXktd2lmaS1wYXNzd29yZA==' | base64 -d
```

Output:

```text
my-wifi-password
```

This was not the flag.

### `logo_recovered.jpg`

The recovered 25-byte file contained the text:

```text
JFIF decoy image data
```

It was a fake/decoy JPEG, not a valid image.

### Directory Metadata Addresses

Using `icat` on a directory metadata address, such as `66-144-2`, returns directory index data rather than the contents of a regular file. Always map paths to their metadata addresses with `fls` before using `icat`.

### Avoid Overwriting a Compressed Input

Do not redirect decompressed output to the same file being read:

```bash
gzip -dc file.gz > file.gz
```

The shell can truncate the input before `gzip` reads it. Use a different output filename.

## Lessons Learned

- Identify the partition offset before running Sleuth Kit commands.
- Use `fls -r` to list filesystem entries and `fls -r -d` to focus on deleted entries.
- The `*` marker in `fls` output indicates a deleted entry.
- Recover deleted files with `icat` and the correct metadata address.
- Check recovered file types; deleted files may still be compressed.
- ROT13 is a simple substitution cipher, not encryption.
- Verify whether an entry is a regular file or a directory before extracting it.

## Final Answer

```text
InIt{d3l3t3d_but_n0t_g0n3}
```
