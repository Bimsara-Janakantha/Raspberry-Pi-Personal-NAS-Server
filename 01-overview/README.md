# 01. Project Overview

> A personal NAS built on a Raspberry Pi 5 — self-hosted, private, and fully under your control.

---

## What Is This Project?

This project is a fully custom-built home Network Attached Storage (NAS) server, assembled inside a recycled desktop PC case and powered by a Raspberry Pi 5. The goal was to create a private, reliable storage system that replaces paid cloud services like OneDrive or Google Drive — at no recurring cost.

The system stores approximately 8 TB of data across three enterprise SAS drives in a RAID 5 array. It supports two regular users, each with their own isolated storage space and hard size limits. All files are accessible from anywhere in the world through a secure Cloudflare tunnel.

---

## Why Build a NAS Instead of Using Cloud Storage?

| Concern | Commercial Cloud | This NAS |
|---------|-----------------|----------|
| Monthly cost | $5–$15/month ongoing | ~$0 after hardware |
| Privacy | Files stored on third-party servers | Files stay on your own hardware |
| Storage limit | Fixed tier, costs more to expand | Add drives as needed |
| Internet dependency | Always required | Works on local network without internet |
| Customisation | Limited | Full control |

---

## What the Finished System Can Do

- Serve as a shared network drive on your home Wi-Fi
- Sync files automatically to Windows, Mac, iOS, and Android — just like OneDrive
- Let users access their personal files securely from anywhere via a web browser or desktop app
- Alert you with an LED and buzzer if the system gets too hot
- Protect data with RAID 5 parity — one drive can fail without losing anything
- Enforce strict per-user storage limits so no single user fills the disk

---

## Project Scope

This documentation covers the complete build from start to finish:

1. [Hardware Configuration](../02-hardware/README.md)
2. [Operating System Setup](../03-os-setup/README.md)
3. [Storage Configuration](../04-storage/README.md)
4. [User Management](../05-user-management/README.md)
5. [Nextcloud Setup](../06-nextcloud/README.md)
6. [Internet Access](../07-internet-access/README.md)
7. [Client Setup](../08-client-setup/README.md)
8. [Project Summary](../09-summary/README.md)

---

[Next: Hardware Configuration →](../02-hardware/README.md)
