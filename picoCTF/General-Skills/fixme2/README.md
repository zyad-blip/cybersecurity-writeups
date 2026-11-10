# fixme2.py — picoCTF

## Challenge information

- **Platform:** picoCTF
- **Category:** General Skills
- **Difficulty:** Easy
- **Event:** picoMini 2022
- **Author:** LT 'syreal' Jones
- **Status:** Completed

## Objective

Fix the syntax error in the supplied Python script so it runs and prints the decrypted flag.

## Approach

I first ran the script:

```bash
python fixme2.py
```

Python reported a `SyntaxError` on this line:

```python
if flag = "":
```

The issue is that `=` is the assignment operator. An `if` condition needs a comparison expression. The intended condition is:

```python
if flag == "":
```

- `=` assigns a value to a variable.
- `==` compares two values.

I edited the file with:

```bash
nano fixme2.py
```

During my actual run, I changed the condition to `if flag == "2222":`. This is syntactically valid and the script printed the flag because the decrypted flag is not equal to `"2222"`, so the `else` branch ran. However, this is not the intended logical fix: the correct condition for checking whether the flag is empty is `if flag == "":`.

## Commands and tools

```bash
python fixme2.py
nano fixme2.py
python fixme2.py
```

The final run printed:

```text
That is correct! Here's your flag: academy{3qu4l1ty_n0t_4551gnm3nt_ad29eab9}
```

## Result

**Flag:**

```text
academy{3qu4l1ty_n0t_4551gnm3nt_ad29eab9}
```

## Lessons learned

1. Python uses `=` for assignment and `==` for equality comparison.
2. A syntax error prevents Python from parsing and running the script.
3. A change can make code syntactically valid without preserving its intended logic.
4. Read the condition's purpose before changing it; here, the goal was to test whether `flag` is an empty string.

## Takeaway

The challenge was about distinguishing assignment from comparison in Python. The proper repair is `if flag == "":`, even though my recorded successful run used a different comparison value.
