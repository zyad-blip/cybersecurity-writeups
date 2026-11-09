# 2warm

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

Convert the decimal number `42` (base 10) into binary (base 2).

## My approach

I first tried the suggested `bc` command:

```bash
echo "obase=2; 42" | bc
```

The command was not available on my system, so I installed it:

```bash
sudo apt install bc -y
```

After installation, I ran the conversion again:

```bash
echo "obase=2; 42" | bc
```

Output:

```text
101010
```

## How it works

- `echo` sends the expression to standard output.
- The pipe (`|`) passes that expression to `bc`.
- `obase=2` sets the output base to binary.
- `42` is the input value, interpreted in decimal by default.

## Manual conversion

Binary uses powers of 2. The decimal number 42 can be written as:

```text
42 = 32 + 8 + 2
   = 2^5 + 2^3 + 2^1
```

Writing a 1 in those positions and 0 in the remaining positions gives:

```text
101010
```

## Result

```text
42 (base 10) = 101010 (base 2)
```

## What I learned

- How to convert a decimal number to binary using `bc`.
- How `obase` controls the output number base in `bc`.
- How a shell pipe sends an expression from one command to another.
- How to install a missing command-line utility with APT.
- How to verify a conversion manually using powers of 2.

## Key takeaway

In `bc`, `obase` sets the output base. For decimal-to-binary conversion, use `obase=2`.
