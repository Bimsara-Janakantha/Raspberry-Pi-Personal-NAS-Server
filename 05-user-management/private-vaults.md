# Private Vaults

Each user gets a dedicated folder on the 8 TB RAID array. These folders are locked so users cannot access each other's data — not through the network drive, not through Nextcloud, not through any other method.

---

## Create the Folders

```bash
sudo mkdir /mnt/NAS_Storage/alice
sudo mkdir /mnt/NAS_Storage/bob
```

---

## Assign Ownership

Give each folder to its respective user:

```bash
sudo chown alice:alice /mnt/NAS_Storage/alice
sudo chown bob:bob     /mnt/NAS_Storage/bob
```

---

## Lock the Doors

Apply strict permissions so only the folder owner can access it:

```bash
sudo chmod 700 /mnt/NAS_Storage/alice
sudo chmod 700 /mnt/NAS_Storage/bob
```

`chmod 700` means:

| Who | Permission |
|-----|-----------|
| Owner (alice / bob) | Read, write, execute |
| Group | No access |
| Everyone else | No access |

---

## Verify the Setup

```bash
ls -la /mnt/NAS_Storage/
```

Expected output:

```
drwx------ 2 alice alice 4096 Jul 10 10:00 alice
drwx------ 2 bob   bob   4096 Jul 10 10:01 bob
```

The `drwx------` confirms each folder is strictly private to its owner.

---

> 💡 **Note:** When Nextcloud is set up later, the folder ownership will be slightly adjusted to allow the Nextcloud web server process to access the folders. This is covered in the [Nextcloud — Permissions Bridge](../06-nextcloud/permissions-bridge.md) step. The strict user isolation remains intact.

---

[← User Accounts](user-accounts.md) | [Next: Storage Quotas →](quotas.md)
