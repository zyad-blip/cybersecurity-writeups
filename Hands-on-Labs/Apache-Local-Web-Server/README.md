# Apache Local Web Server Lab

**Type:** Personal local lab  
**Status:** Built and tested

## Objective

Build a basic Apache web server in a Kali Linux virtual machine and understand how it handles HTTP requests and records them in access logs.

## Setup

Apache was enabled and started with:

```bash
sudo systemctl enable --now apache2
```

I verified that the service was listening on port 80 and opened the default Apache page in a browser.

## Virtual host

I configured a local virtual host named:

```text
site1.test
```

This helped me understand how Apache can serve a site using a hostname rather than only the default page.

## Request and log observation

I sent requests using the browser and `curl`, then watched the virtual host's access log:

```bash
sudo tail -f /var/log/apache2/site1_access.log
```

The log showed successful requests such as:

- `GET /` with status `200`
- A request for `favicon.ico` returning `404`

The `200` response confirmed that the page request succeeded. The favicon `404` indicated that the browser requested an icon that was not present at that path; it did not mean the main page had failed.

## What I learned

- How to start and enable the Apache service with systemd.
- How to verify that a service is listening on a port.
- What a virtual host does.
- How HTTP requests appear in Apache access logs.
- How to interpret common HTTP status codes in log entries.

## Why this matters for web security

A local web server is a safe place to learn how requests reach an application, how hostnames map to virtual hosts, and where server-side evidence is recorded. These fundamentals are useful before studying web testing in authorized environments.

## Takeaway

Watching the access log while making requests made the relationship between the browser, HTTP, and Apache much clearer.
