# Mount Options

After linking the vaults, the mount options for each storage entry control how Nextcloud interacts with the physical folders. These settings are found in the External storages admin page next to each vault entry.

---

## Recommended Settings

| Option | Setting | Reason |
|--------|---------|--------|
| Check filesystem changes | Once every direct access | Detects files added via Samba/Windows drives without delay |
| Read only | **OFF** | Users need full upload and edit access |
| Enable previews | **ON** | Generates photo and file thumbnails in the web interface |
| Enable sharing | **ON** | Users can create public sharing links for individual files |
| Compatibility with Mac NFD encoding | **OFF** | Unnecessary overhead unless users regularly upload files with special characters in filenames |

---

## Why "Once Every Direct Access" Matters

This project uses two access methods for the same files:

1. **Nextcloud** (web, mobile, desktop app) — Nextcloud tracks every file change internally
2. **Samba** (Windows mapped drives) — Files are written directly to the filesystem without going through Nextcloud

Without the "check filesystem changes" option, files dropped via the Z: drive in Windows would be invisible in Nextcloud until a manual rescan. Setting it to "once every direct access" means Nextcloud scans the physical folder every time a user opens their dashboard — so both access methods stay perfectly in sync.

---

## A Note on Read Only

The read-only switch defaults to **OFF**, but it is worth double-checking before saving. If it is accidentally left ON, users can see their files in Nextcloud but cannot upload, edit, or delete anything. This is a common gotcha.

---

[← Connect User Vaults](connect-vaults.md) | [Next: Debug — Desktop App Issues →](debug-desktop-app.md)
