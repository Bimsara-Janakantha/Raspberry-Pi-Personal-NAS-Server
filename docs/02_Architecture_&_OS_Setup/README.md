# 🏗️ Part 2 — Architecture & Operating System Setup

This section covers everything from the high-level storage architecture down to user accounts, disk quotas, and the local high-speed file transfer method.

---

## 📂 Documents in This Section

| #   | File                                                     | Description                                                 |
| --- | -------------------------------------------------------- | ----------------------------------------------------------- |
| 2.1 | [storage-architecture.md](./2.1-storage-architecture.md) | Three-tier storage design, disk formatting & fstab mounting |
| 2.2 | [user-accounts.md](./2.2-user-accounts.md)               | Creating OS-level Linux user accounts                       |
| 2.3 | [disk-quotas.md](./2.3-disk-quotas.md)                   | Enforcing hardware-level storage quotas                     |
| 2.4 | [samba-local-access.md](./2.4-samba-local-access.md)     | Samba SMB shares for LAN file access                        |
| 2.5 | [bulk-file-transfer.md](./2.5-bulk-file-transfer.md)     | Bulk import files & resync to Nextcloud                     |

---

## 🗺️ Architecture at a Glance

<img src="../../asserts/img_os1.png" alt="Storage Architecture Diagram" width="80%"/>

**Why three tiers?**

- The SD card only reads on boot — write cycles stay near zero, extending its life significantly
- The 1TB HDD absorbs all constant database read/writes from Nextcloud — standard HDDs handle this well
- The 4TB array is a clean, dedicated file store with no OS or database contention
- Quotas are enforced at the kernel/filesystem level on Tier 3 — impossible to bypass from any app

---

## ⚡ Quick-Start Order

If you are setting this up from scratch, follow the documents in order:

```
2.1 → Format & mount the storage array
2.2 → Create Linux user accounts
2.3 → Apply disk quotas
2.4 → Set up Samba for local access
2.5 → Bulk-import existing files
```

---

_Next section: [Part 3 — Nextcloud & Cloudflare Setup](../03-nextcloud-and-cloudflare/README.md)_
