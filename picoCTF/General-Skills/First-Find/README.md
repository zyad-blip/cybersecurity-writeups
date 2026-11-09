# First Find

**Platform:** picoCTF  
**Category:** General Skills  
**Difficulty:** Easy

## Objective

Locate the file named `uber-secret.txt` inside the provided archive contents and read it to retrieve the flag.

## Approach

### 1. Inspect the working directory

I checked the current directory:

```bash
ls
```

The output showed `files.zip` and a `files/` directory, along with other files from my previous work. The extracted `files/` directory was already present when I started the recorded search.

### 2. Search recursively for the target filename

I used `find` to search inside the extracted directory:

```bash
find /mnt/c/Users/zyadr/Downloads/files/ -name 'uber-secret.txt'
```

It returned this path:

```text
/mnt/c/Users/zyadr/Downloads/files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```

This showed that the file was nested several directories deep, including hidden directories such as `.secret`.

### 3. Read the file using its full path

I first tried to pipe the output of `find` into `cat uber-secret.txt`, but that failed because `cat` interpreted `uber-secret.txt` as a filename in the current directory. It did not automatically use the path printed by `find`.

After using the full path returned by `find`, I read the file successfully:

```bash
cat /mnt/c/Users/zyadr/Downloads/files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```

## Result

The file contained the flag:

```text
academy{f1nd_15_f457_ab443fd1}
```

## What I learned

- `find <directory> -name '<filename>'` searches recursively below the given directory.
- A leading dot in a directory name, such as `.secret`, means it is hidden in normal directory listings; it does not prevent `find` from searching it.
- `find` prints the path it discovers; it does not print the file's contents.
- `cat filename` opens the named file. If a filename is supplied, `cat` does not use standard input as the file instead.
- A pipe (`|`) sends one command's standard output to the next command's standard input. It does not turn that text into a command-line filename argument.

## Key takeaway

Separate the two tasks: first locate the file with `find`, then read it using the path returned by the search. Understanding what a pipe passes between commands prevents confusing standard input with filenames.
