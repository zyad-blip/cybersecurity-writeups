# Warmed Up

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

Convert the hexadecimal value `0x3D` (base 16) into decimal (base 10).

## My attempts

I first tried:

```bash
echo "0x3D" | base64 -d
```

This was the wrong decoder: Base64 and hexadecimal are different encodings, so a Base64 decoder cannot be used to interpret a hexadecimal number.

I then tried:

```bash
echo "0x3D" | base16 -d
```

My system did not have a `base16` command installed. It is not a standard command available in every Linux installation.

I also tried using `bc`, but the utility was not installed:

```bash
echo "ibase=16;0x3D" | bc
```

For `bc`, the hexadecimal digits should be supplied without the `0x` prefix, for example:

```bash
echo 'ibase=16; 3D' | bc
```

## Working solution

The command that worked on my system was:

```bash
printf "%d\\n" 0x3D
```

Output:

```text
61
```

The shell's `printf` recognizes the `0x` prefix as hexadecimal input, and `%d` prints the value in decimal notation.

## Manual conversion

Hexadecimal is base 16. In `0x3D`:

- `3` is in the 16¹ position.
- `D` represents decimal 13 and is in the 16⁰ position.

So:

```text
(3 × 16) + (13 × 1)
= 48 + 13
= 61
```

## Result

```text
0x3D (base 16) = 61 (base 10)
```

## What I learned

- The difference between hexadecimal and Base64.
- The `0x` prefix indicates a hexadecimal number in common programming and shell contexts.
- `printf "%d"` can format a hexadecimal integer as decimal.
- Some utilities, such as `bc`, may not be installed by default.
- When a command is missing, check whether a simpler built-in tool already solves the problem.

## Key takeaway

Choose a conversion tool based on the representation being converted. Hexadecimal-to-decimal conversion is a number-base conversion, not a Base64 decoding task.
