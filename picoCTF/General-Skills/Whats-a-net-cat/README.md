# what's a net cat?

**Platform:** picoCTF 2019  
**Category:** General Skills  
**Difficulty:** Easy  
**Status:** Completed

## Challenge objective

The challenge gives a hostname and port and asks for a connection using Netcat. The service sends the flag when a client connects.

## Solution approach

Use the host and port provided by the active challenge page:

```bash
nc <HOST_FROM_CHALLENGE> <PORT_FROM_CHALLENGE>
```

The server sends its response to the terminal. For this challenge, the response contains the flag; no additional command needs to be sent.

## Why this works

Netcat creates a raw TCP connection to the specified host and port. It forwards data received from the remote service to standard output.

This is different from using an HTTP client such as `curl`: the challenge service is not necessarily speaking HTTP. The task only requires a TCP connection.

## What this challenge teaches

- The basic syntax of Netcat.
- The relationship between a hostname, a port, and a TCP connection.
- How a server can send data immediately after a client connects.
- The difference between a raw TCP service and an HTTP service.

## Endpoint note

Public write-ups for older copies of this challenge list different historical ports. Use the host and port given by your own challenge page rather than relying on an old endpoint.

## Flag

The accepted flag is not reproduced in this reconstructed note because the exact value from the completed account was not provided.

## References

- [picoCTF 2019 — what's a net cat? walkthrough](https://picoctfsolutions.com/picoctf-2019-what-s-a-net-cat)
- [Community write-up — what's a net cat?](https://github.com/kevinjycui/picoCTF-2019-writeup/blob/master/General%20Skills/what's%20a%20net%20cat/README.md)
