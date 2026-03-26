# OS Selection

Choosing the right operating system for a home NAS comes down to stability, community support, and how much you want to manage via a web interface versus the command line.

---

## What Was Chosen: Ubuntu Server 24.04 LTS + CasaOS

### Ubuntu Server 24.04 LTS

Ubuntu Server is a well-supported, stable Linux distribution with a long-term support (LTS) lifecycle of 5 years. It is officially supported on the Raspberry Pi 5 and receives regular security updates.

For a system that will run 24/7, an LTS release is always preferable over a rolling release — fewer surprise updates and breaking changes.

### CasaOS

CasaOS is not a separate operating system — it is a lightweight web dashboard that installs on top of Ubuntu (or Debian) with a single command. It provides:

- A clean, browser-based management interface
- A built-in App Store powered by Docker (one-click installs for Nextcloud, Samba, databases, etc.)
- A visual storage manager and file browser
- An easy way to monitor containers and services

CasaOS handles the visual layer, while Ubuntu handles everything underneath — RAID management, user accounts, disk quotas, and system services.

---

## Why Not Raspberry Pi OS?

Raspberry Pi OS is great for desktop or hobbyist use, but Ubuntu Server was chosen for this project because:

- Better suited for server workloads
- More familiar to Linux system administrators
- Broader software compatibility with server-grade tools like `mdadm` and `quota`

---

## Why Not TrueNAS or OMV?

Dedicated NAS operating systems like TrueNAS Scale or OpenMediaVault are valid options, but they were not chosen here because:

- This project required fine-grained control over RAID setup, user quotas, and custom GPIO scripting
- The split-brain Nextcloud architecture required manual Docker volume configuration
- Ubuntu + CasaOS gives all the visual convenience without locking you into a pre-built NAS framework

---

[← OS Overview](README.md) | [Next: OS Installation →](os-installation.md)
