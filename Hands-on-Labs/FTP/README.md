# FTP Service Lab

**Type:** Personal hands-on lab  
**Status:** Completed

## Objective

Understand the FTP client/server model and the difference between active and passive FTP data connections.

## FTP client/server model

FTP uses a client and a server:

- The **client** initiates the session and sends commands.
- The **server** listens for the control connection and responds to client requests.
- File transfers use a separate data connection.

## Active vs. passive mode

| Mode | How the data connection is initiated |
|---|---|
| Active FTP | The server initiates the data connection back to the client. |
| Passive FTP | The client initiates the data connection to the server. |

The key difference is which side initiates the data connection. This matters when firewalls or NAT are involved.

## What I learned

- FTP is a client/server protocol.
- The control connection and data connection serve different purposes.
- Active and passive modes differ in who initiates the data connection.
- Network configuration can affect whether a transfer succeeds.

## Security note

Traditional FTP does not encrypt credentials or file contents by default. Do not use it to transfer sensitive data over untrusted networks. FTPS and SFTP are different approaches and should not be confused with one another.

## Takeaway

Understanding the connection model makes FTP behavior easier to troubleshoot and helps when identifying and assessing network services in an authorized lab.
