# Cybersecurity Learning & CTF Write-ups

<p align="center">
  <img src="https://img.shields.io/badge/Focus-Cybersecurity-informational?style=flat-square" alt="Focus: Cybersecurity" />
  <img src="https://img.shields.io/badge/Approach-Hands--on-blue?style=flat-square" alt="Hands-on learning" />
  <img src="https://img.shields.io/badge/Status-Always%20learning-success?style=flat-square" alt="Always learning" />
</p>

A personal knowledge base documenting my cybersecurity learning through CTF challenges, guided rooms, and hands-on labs.

The goal is not only to collect flags. I want to understand **why a technique works**, keep track of what I have practiced, and build a repeatable problem-solving methodology.

## Learning focus

- **Linux & Command Line:** file inspection, text searching, shell basics, and system fundamentals.
- **Networking:** client/server communication, common services, SMB, and reconnaissance.
- **Web & Infrastructure:** Apache, HTTP requests, virtual hosts, and access logs.
- **Digital Forensics:** metadata analysis and extracting useful evidence.
- **Binary Analysis:** identifying executables, extracting strings, and basic disassembly.
- **CTFs & General Skills:** practical problem-solving in authorized training environments.

## Write-ups and labs

| Topic | Challenge / Lab | Type | Status |
|---|---|---|---|
| picoCTF General Skills | [13 completed challenges](picoCTF/General-Skills/README.md) | CTF challenges | Completed |
| Digital Forensics | [Intro to Digital Forensics](TryHackMe/Intro-to-Digital-Forensics/README.md) | TryHackMe room | Completed |
| Reconnaissance | [Passive Reconnaissance](TryHackMe/Passive-Reconnaissance/README.md) | Training room | Completed |
| Reconnaissance | [Active Reconnaissance](TryHackMe/Active-Reconnaissance/README.md) | Training room | Completed |
| General Skills | [Big Zip](General-Skills/Big-Zip/README.md) | CTF challenge | Completed |
| Networking | [FTP Service Lab](Hands-on-Labs/FTP/README.md) | Hands-on lab | Completed |
| Web / Infrastructure | [Apache Local Web Server](Hands-on-Labs/Apache-Local-Web-Server/README.md) | Hands-on lab | Built and tested |
| Linux | [CLI Fundamentals](Linux/CLI-Fundamentals/README.md) | Practical notes | Practiced |

For a more detailed view of what is complete and what is still in progress, see **[PROGRESS.md](PROGRESS.md)**.

## Repository structure

```text
.
├── General-Skills/
│   └── Big-Zip/
├── Hands-on-Labs/
│   ├── Apache-Local-Web-Server/
│   └── FTP/
├── In-Progress/
├── Linux/
│   └── CLI-Fundamentals/
├── picoCTF/
│   └── General-Skills/
│       ├── 2warm/
│       ├── Bases/
│       ├── First-Grep/
│       ├── Nice-netcat/
│       ├── Obedient-Cat/
│       ├── Plumbing/
│       ├── Printer-Shares/
│       ├── Static-Aint-Always-Noise/
│       ├── Tab-Tab-Attack/
│       ├── Warmed-Up/
│       ├── Wave-a-flag/
│       ├── Whats-a-net-cat/
│       └── strings-it/
├── Templates/
│   └── WRITEUP-TEMPLATE.md
├── TryHackMe/
│   ├── Active-Reconnaissance/
│   ├── Intro-to-Digital-Forensics/
│   └── Passive-Reconnaissance/
├── PROGRESS.md
└── README.md
```

## How I document a challenge

Each write-up aims to include:

1. **Objective** — what the challenge asks me to find or understand.
2. **Approach** — how I investigated the problem.
3. **Commands and tools** — what I actually used, with relevant output or evidence.
4. **Result** — the confirmed finding or flag, when appropriate.
5. **Lessons learned** — what I understood and what I would improve next time.

Some earlier challenge notes are reconstructed from public solution references after completion. Notes based on my recorded solve commands are labelled accordingly.

## Responsible practice

All practical testing and security exercises documented here are performed in local labs or authorized training environments. Never use these techniques against systems without permission.

---

*This repository is a work in progress and will grow as I complete more rooms and labs.*
