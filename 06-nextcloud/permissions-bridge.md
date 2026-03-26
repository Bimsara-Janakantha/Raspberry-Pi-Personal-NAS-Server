# Permissions Bridge

The user vaults were locked with `chmod 700`, meaning only the folder owner can access them. Before installing Nextcloud, these permissions need to be updated so the Nextcloud web server process can also access the folders — without giving users access to each other's data.

---

## Why This Is Needed

Nextcloud runs inside a Docker container. The web server process inside that container runs as internal user ID **33** (the `www-data` user). When Nextcloud tries to read Alice's folder, it is operating as user ID 33 — which currently has zero access.

The fix is to change the group ownership of each folder to group ID 33, and set permissions to `770` (owner and group have full access, everyone else has none).

---

## Run These Commands

```bash
sudo chown -R alice:33 /mnt/NAS_Storage/alice
sudo chmod -R 770      /mnt/NAS_Storage/alice

sudo chown -R bob:33   /mnt/NAS_Storage/bob
sudo chmod -R 770      /mnt/NAS_Storage/bob
```

---

## What This Achieves

| Who | Access |
|-----|--------|
| Alice (owner) | Full read/write to her folder |
| Nextcloud process (group 33) | Full read/write to her folder |
| Bob | No access to Alice's folder |
| Everyone else | No access |

The storage quotas are completely unaffected by this change — quotas are tied to the user ID, not the permissions.

---

## Also Prepare the Web Server Directory

Create the directory where Nextcloud's core application files will live:

```bash
sudo mkdir -p /mnt/Web_Server/Nextcloud_Brain
```

---

[← Why Nextcloud](why-nextcloud.md) | [Next: CasaOS Installation →](casaos-install.md)
