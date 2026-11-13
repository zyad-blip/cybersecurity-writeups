# Red Nexus — WebDAV Upload and Remote Command Execution

**Platform:** Red Nexus Windows Lab  
**Category:** Web / WebDAV / File Upload  
**Difficulty:** Not specified  
**Status:** Completed

## Objective

Identify the exposed web service, determine whether its WebDAV endpoint accepts file uploads, and verify the impact in the authorized lab.

## Initial Information

- Target: `10.13.41.22`
- Nmap hostname: `win_winweb.red-nexus-windows_labnet`
- HTTP service: TCP/80

## Approach

### 1. Service enumeration

```bash
nmap -Pn -sCV 10.13.41.22
```

Relevant output:

- TCP/80 was open.
- Service identified as Apache httpd 2.4.69 on Debian.
- HTTP title: `Home · Red Nexus`.

**Important discrepancy:** The lab hostname and recovered flag refer to Windows/IIS, but service detection identified Apache on Debian. A hostname or flag label is not reliable proof of the actual web-server implementation.

### 2. Check WebDAV with Nmap

```bash
nmap -Pn -p80 --script http-webdav-scan 10.13.41.22
```

The script did not report WebDAV details. This is not conclusive evidence that WebDAV is disabled; the endpoint should be checked directly.

### 3. Inspect allowed HTTP methods

```bash
curl -i -X OPTIONS http://10.13.41.22/webdav/
```

The response included:

```text
Allow: OPTIONS,GET,HEAD,POST,DELETE,TRACE,PROPFIND,PROPPATCH,COPY,MOVE,LOCK,UNLOCK
```

The advertised methods include WebDAV operations such as `PROPFIND`, `PROPPATCH`, `COPY`, and `MOVE`. The response suggested that a WebDAV resource was available at `/webdav/`.

### 4. Verify upload behavior in the lab

A harmless test PHP file was created locally and uploaded to the lab's WebDAV path using an HTTP PUT request via curl's `-T` option.

```bash
curl -T <test-file> http://10.13.41.22/webdav/<test-file>
```

The server returned:

```text
201 Created
Resource /webdav/<test-file> has been created.
```

This confirmed that the endpoint accepted the uploaded resource.

### 5. Confirm impact

The uploaded test resource was then accessed through the lab web server. In the lab, it was possible to execute a command and read the designated flag file:

```text
redNexus{iis_webdav_webshell_rce}
```

This confirmed server-side command execution through the uploaded resource in this lab environment.

## Findings

- TCP/80 exposed Apache httpd 2.4.69 (Debian).
- The `/webdav/` endpoint advertised WebDAV-related HTTP methods.
- The Nmap WebDAV script did not identify the configuration, but a direct OPTIONS request exposed the allowed-method list.
- An HTTP PUT upload was accepted with `201 Created`.
- Accessing the uploaded lab resource demonstrated command execution and allowed recovery of the lab flag.
- The flag says `iis_webdav`, while Nmap identified Apache on Debian. This discrepancy remains unexplained and should be documented rather than silently assuming IIS.

## Result

```text
redNexus{iis_webdav_webshell_rce}
```

## What I Learned

- `-Pn` skips host discovery; it does not mean the host is actually offline or online.
- `-sC` runs Nmap's default scripts, while `-sV` attempts service/version detection.
- An empty or unhelpful Nmap script result does not replace direct protocol checks.
- HTTP `OPTIONS` can reveal the methods advertised by a path.
- WebDAV methods and upload permissions should be verified with the server's actual response.
- HTTP `201 Created` confirms that the requested resource was created; it does not by itself prove code execution.
- Keep the service fingerprint, hostname, and flag wording separate when they appear inconsistent.

## Takeaway

The key weakness in this lab was an upload-enabled WebDAV location combined with server-side execution of the uploaded resource. In a real deployment, WebDAV should be disabled where unnecessary, write access should be restricted, and uploaded files should be stored outside executable web paths or served with execution disabled.
