# picoCTF — General Skills

This section contains solution notes for the picoCTF General Skills challenges completed during my learning.

## Completed challenges

| Challenge | Edition | Difficulty | Main concept | Write-up |
|---|---|---|---|---|
| Wave a flag | 2021 | Easy | Linux permissions and command-line arguments | [Read](Wave-a-flag/README.md) |
| Tab, Tab, Attack | 2021 | Easy | ZIP extraction, tab completion, strings, grep | [Read](Tab-Tab-Attack/README.md) |
| Nice netcat... | 2021 | Easy | Netcat and decimal ASCII conversion | [Read](Nice-netcat/README.md) |
| Obedient Cat | 2021 | Easy | Downloading and reading a plain-text file | [Read](Obedient-Cat/README.md) |
| First Grep | 2019 | Easy | Searching file contents with grep | [Read](First-Grep/README.md) |
| First Find | picoCTF | Easy | Recursive file search and reading files by full path | [Read](First-Find/README.md) |
| Bases | 2019 | Easy | Base64 decoding | [Read](Bases/README.md) |
| strings it | 2019 | Easy | Extracting printable strings and filtering output | [Read](strings-it/README.md) |
| what's a net cat? | 2019 | Easy | Connecting to a TCP service with Netcat | [Read](Whats-a-net-cat/README.md) |
| Plumbing | 2019 | Medium | Pipes, stdin/stdout, Netcat, grep | [Read](Plumbing/README.md) |
| Warmed Up | 2019 | Easy | Hexadecimal to decimal conversion | [Read](Warmed-Up/README.md) |
| 2warm | 2019 | Easy | Decimal to binary conversion with bc | [Read](2warm/README.md) |
| Printer Shares | 2026 | Easy | SMB share discovery and file retrieval | [Read](Printer-Shares/README.md) |
| Static ain't always noise | 2021 | Easy | Binary identification, printable strings, file offsets, and disassembly | [Read](Static-Aint-Always-Noise/README.md) |
| Magikarp Ground Mission | 2021 | Easy | SSH, shell navigation, and reading files across directories | [Read](Magikarp-Ground-Mission/README.md) |

## Documentation note

Some earlier challenge notes were reconstructed after completion by reviewing publicly available walkthroughs. The Plumbing, Printer Shares, Warmed Up, 2warm, Static ain't always noise, Magikarp Ground Mission, and First Find write-ups are based on the commands and output recorded during my own solves.

## Skills covered

- Linux file handling and executable permissions.
- Shell navigation and tab completion.
- Recursive file searching with `find`.
- Text searching with `grep`.
- Printable string extraction with `strings`.
- Base64 decoding.
- Netcat and TCP connections.
- Decimal ASCII conversion.
- Command pipelines and standard input/output.
- SMB share enumeration and remote file retrieval.
- Conversion between hexadecimal and decimal number bases.
- Conversion between decimal and binary number bases using `bc`.
- Identifying Linux ELF executables with `file`.
- Extracting strings and hexadecimal file offsets from binaries.
- Basic disassembly with `objdump`.
- Connecting to remote Linux challenge environments using SSH.
- Navigating between the filesystem root (`/`) and a user's home directory (`~`).
- Reading files and combining information from multiple locations.

## Responsible practice

All network connections described here were made only to the authorized picoCTF / CyLab challenge services.
