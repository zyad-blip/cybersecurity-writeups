# PW Crack 1

**Platform:** picoCTF picoMini 2022  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** In Progress — password identified; flag not yet verified

## Objective

The challenge provides a Python password checker and an encrypted flag file. The goal is to identify the accepted password, run the checker, and retrieve the flag.

## Initial Information

The downloaded files are:

- `level1.py` — the password checker and XOR decryption function.
- `level1.flag.txt.enc` — the encrypted flag.

Both files must stay in the same directory because the script opens the encrypted file using a relative path.

## Approach

I started by reading the Python source:

```bash
cat level1.py
```

The important part is the password comparison:

```python
if(user_pw == "8713"):
    print("Welcome back... your flag, user:")
    decryption = str_xor(flag_enc.decode(), user_pw)
    print(decryption)
    return
```

The accepted password is hard-coded in the source, so there is no need to brute-force it.

## Commands and Tools

Run the script with the Python interpreter:

```bash
python3 level1.py
```

When prompted, enter:

```text
8713
```

Do not run `str_xor` as a standalone terminal command. It is a Python function defined inside `level1.py`.

Also, running `./level1.py` made the shell try to interpret Python syntax as shell commands. Calling `python3 level1.py` explicitly uses the Python interpreter and avoids that problem.

## How the Decryption Works

The script uses XOR with the password as a repeating key. The `str_xor` function extends the key until it matches the encrypted text length, then XORs each character with the corresponding key character.

XOR is reversible:

```text
encrypted_character XOR key_character = plaintext_character
plaintext_character XOR key_character = encrypted_character
```

This is why the same function can decrypt the flag.

## Findings

- The password is visible in the source code: `8713`.
- The encrypted flag is read from `level1.flag.txt.enc`.
- The flag has not yet been recorded here because the decrypted output has not been verified.

## Result

**Password:** `8713`  
**Flag:** Pending verification — run the script and record the exact output.

## What I Learned

- Read the source code before trying to reverse the encryption.
- A Python function is not automatically a terminal command.
- Use `python3 filename.py` to run a Python script.
- A shell error can come from using the wrong interpreter, not from a bug in the Python code.
- XOR with the same key reverses the operation.

## Takeaway

For simple password-checking challenges, inspect the condition that validates the input first. Understand how the script is meant to run before trying to analyze the encrypted file.
