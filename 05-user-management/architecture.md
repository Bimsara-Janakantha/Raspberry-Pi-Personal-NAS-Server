# Architecture Overview

Before creating any accounts or folders, it helps to understand how the different layers of access work together.

---

## The Two-Door Model

Think of the NAS as a building with two separate entrances:

```
[ Internet / Wi-Fi ]
        │
        ├── Front Door: Nextcloud (web, mobile, desktop app)
        │       └── Checks Nextcloud username + password
        │
        └── Back Door: Samba (Windows mapped drives, local network only)
                └── Checks Linux system username + password
```

Both doors lead to the same physical folders on the RAID array. The underlying Ubuntu user accounts and disk quotas apply regardless of which door a user enters through.

---

## User Design

| User | Role | Storage Limit | Access |
|------|------|--------------|--------|
| `admin` | System administrator | Unlimited | All areas including Web_Server |
| `alice` | Regular user | 5 TB | Alice's vault only |
| `bob` | Regular user | 1.5 TB | Bob's vault only |

---

## Folder Layout on the RAID Array

```
/mnt/NAS_Storage/
├── alice/          ← owned by alice, locked to alice (+ Nextcloud process)
└── bob/            ← owned by bob, locked to bob (+ Nextcloud process)

/mnt/Web_Server/
└── Nextcloud_Brain/   ← owned by admin only (chmod 700)
```

---

## Why Keep the Ubuntu Accounts Even After Setting Up Nextcloud?

This is a common question. The short answer: the Ubuntu accounts are the invisible glue holding everything together.

- **Disk quotas** are set at the Linux kernel level, tied to the Ubuntu user ID. Delete the account and the quota disappears.
- **Folder ownership** — if you delete a user, their folder becomes "orphaned" and Nextcloud throws permission errors.
- **Samba sharing** — the local network (mapped drive) bouncer only reads Ubuntu accounts. Delete the account and the Z: drive breaks.

Nextcloud manages the web experience. Ubuntu manages the hardware permissions. Both layers work together.

---

[← User Management Overview](README.md) | [Next: User Accounts →](user-accounts.md)
