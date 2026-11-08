# Printer Shares

**Platform:** picoCTF 2026  
**Author:** Janice He  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

Retrieve a file that was accidentally sent to a network printer. The challenge provides a hostname and a non-standard port for the printer's SMB service.

## 1. Check the service

I first checked whether the provided TCP port was reachable:

```bash
nc -vz xebec.cylabacademy.net 34751
```

The connection check reported that port `34751` was open. The DNS forward/reverse mismatch message was informational; it did not mean that the TCP connection had failed.

## 2. Enumerate available SMB shares

I used `smbclient` to list the shares:

```bash
smbclient -N -L xebec.cylabacademy.net -p 34751
```

The server returned:

```text
Sharename       Type
---------       ----
shares          Disk
IPC$            IPC
```

The important result was the disk share named `shares`.

The client also printed a warning while trying to reconnect with SMB1 for workgroup browsing. The share list had already been returned, so this warning did not prevent me from accessing the discovered share.

## 3. Connect to the share

My first connection command omitted the share path:

```bash
smbclient -N xebec.cylabacademy.net -p 34751
```

That produced a usage error because `smbclient` expects a service name in the form `//server/share`.

I then connected to the discovered share:

```bash
smbclient -N //xebec.cylabacademy.net/shares -p 34751
```

The connection opened an interactive `smb:> ` prompt.

## 4. List and retrieve the file

Inside the SMB prompt, I listed the directory:

```text
smb: \> ls
```

The share contained:

```text
dummy.txt
flag.txt
```

I downloaded the flag file to my local working directory:

```text
smb: \> get flag.txt
smb: \> exit
```

Back in the local Kali terminal, I read the downloaded file:

```bash
cat flag.txt
```

## Flag

```text
academy{5mb_pr1nter_5h4re5_3ef3d817}
```

## Command breakdown

| Option / command | Meaning |
|---|---|
| `nc -vz host port` | Check whether the TCP port accepts a connection. |
| `-L` | List the SMB services/shares offered by the server. |
| `-N` | Suppress the password prompt; useful when testing a share that allows guest access. |
| `-p 34751` | Connect to the SMB service on the challenge's non-standard port. |
| `//host/shares` | SMB service path: server name followed by the share name. |
| `ls` | List files in the current remote share directory. |
| `get flag.txt` | Download a remote file to the local machine. |

## Troubleshooting notes

- One attempt to connect to `10.113.139.118` timed out. Using the challenge-provided hostname succeeded, so I kept the hostname from the task instead of assuming the earlier IP was the correct endpoint.
- The reverse-DNS mismatch notice did not stop the TCP connection.
- The SMB1 workgroup warning appeared after the share list had been displayed; it was not the same as a failure to access the `shares` disk share.
- The command must include the share path, for example `//xebec.cylabacademy.net/shares`. A hostname by itself is not a complete SMB service name.

## What I learned

- How to check a service port before connecting to it.
- How to enumerate SMB shares with `smbclient -L`.
- How guest/no-password access can be tested with `-N`.
- How SMB service paths use the `//server/share` format.
- How to browse a remote share and retrieve a file with `get`.
- How to distinguish connection errors and workgroup-browsing warnings from a successful share enumeration.

## References

- [Samba smbclient manual](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)
