# Nice netcat...

**Platform:** picoCTF 2021  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge runs a program on a remote server. Instead of sending a normal sentence, the server returns a sequence of numbers that must be interpreted.

## Solution approach

Read the host and port shown by the active challenge instance, then connect with Netcat:

```bash
nc <HOST_FROM_CHALLENGE> <PORT_FROM_CHALLENGE>
```

The response is a whitespace-separated sequence of decimal integers. These values represent ASCII character codes.

One way to convert the response is to copy the numbers into Python:

```python
numbers = "112 105 99 111 67 84 70"
decoded = "".join(chr(int(value)) for value in numbers.split())
print(decoded)
```

For a live connection, the same conversion can be performed as a pipeline:

```bash
timeout 5 nc <HOST_FROM_CHALLENGE> <PORT_FROM_CHALLENGE> | \
  python3 -c 'import sys; print("".join(chr(int(x)) for x in sys.stdin.read().split()))'
```

If the challenge service takes longer to respond, adjust the timeout. Use the exact host and port displayed by the challenge instance.

## Why this works

- Netcat (`nc`) opens a raw network connection and prints the server's response.
- Each decimal number is an ASCII code for one character.
- Python's `int()` converts each number from text to an integer.
- Python's `chr()` converts each integer to its corresponding character.
- `split()` separates the whitespace-delimited values, and `join()` combines the decoded characters into one string.

For example, decimal `112` maps to the character `p`.

## What this challenge teaches

- Connecting to a remote TCP service with Netcat.
- Recognizing decimal ASCII encoding.
- Converting numeric data to text.
- Using a pipe to pass one command's output into another command.

## Endpoint note

Public walkthroughs may show a historical port or a different challenge instance. Do not rely on a copied port number; use the endpoint shown in your own picoCTF challenge session.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [picoCTF 2021 — Nice netcat... walkthrough](https://picoctfsolutions.com/picoctf-2021-nice-netcat)
