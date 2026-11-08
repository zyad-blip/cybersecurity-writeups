# Passive Reconnaissance

**Category:** Reconnaissance  
**Status:** Completed study notes

## Objective

Understand how to collect publicly available information about a target without directly interacting with its infrastructure.

## Key concepts

### WHOIS

WHOIS can provide domain-registration details such as:

- Registrar
- Registration and expiration dates
- Name servers
- Organization details, when publicly available

Personal registrant information is often redacted for privacy, so WHOIS results may be incomplete.

### DNS records

DNS queries can reveal useful information about a domain:

| Record | Purpose |
|---|---|
| `A` | Maps a hostname to an IPv4 address |
| `AAAA` | Maps a hostname to an IPv6 address |
| `MX` | Lists mail servers for a domain |
| `TXT` | Stores text data, often including SPF or verification records |
| `NS` | Identifies authoritative name servers |

A public resolver such as `1.1.1.1` can be used when performing DNS lookups.

Example query format:

```bash
dig @1.1.1.1 example.com A
dig @1.1.1.1 example.com MX
dig @1.1.1.1 example.com TXT
```

Replace `example.com` with a domain that is within the exercise scope.

## Why passive reconnaissance matters

- It builds an initial picture of the target.
- It can reveal infrastructure and email-related information.
- It avoids sending probes directly to the target's systems.
- It helps decide what should be investigated in later, authorized phases.

## Important lesson

Passive reconnaissance does not mean that every result is accurate or complete. DNS data, WHOIS records, and public sources should be cross-checked before drawing conclusions.
