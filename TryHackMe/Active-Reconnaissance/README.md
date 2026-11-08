# Active Reconnaissance

**Category:** Reconnaissance  
**Status:** Completed study notes

## Objective

Learn how direct, controlled interaction with an authorized target can reveal information about reachable hosts, network paths, and exposed services.

## Tools covered

### Browser and Developer Tools

A browser can reveal more than the rendered page. Developer Tools can help inspect:

- HTTP response headers
- Loaded JavaScript and other page resources
- Network requests
- TLS certificate information
- Clues about the technologies used by a website

### ping

`ping` tests basic reachability and reports round-trip time. The TTL value may provide a rough clue about the remote system, but it is not reliable proof of an operating system.

```bash
ping -c 4 <authorized-target>
```

### traceroute

`traceroute` shows the network hops between the source and destination. It can help explain the path traffic takes, although routers may filter or limit responses.

```bash
traceroute <authorized-target>
```

### Telnet and Netcat

These tools can connect to a specific service port and help inspect the initial service response or banner.

```bash
nc -nv <authorized-target> <port>
```

Only connect to hosts and ports included in the training scope.

## A simple workflow

1. Confirm the target and the permitted scope.
2. Check whether the host is reachable.
3. Observe the network path where relevant.
4. Connect only to the specific authorized service port.
5. Record the banner or response and verify any version information separately.

## Key takeaway

Active reconnaissance produces useful evidence, but a banner or TTL value is only a clue. It should not be treated as definitive identification without further validation.
