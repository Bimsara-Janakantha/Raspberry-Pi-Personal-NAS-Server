# Architecture and Operating System Setup

## 1. Storage Strategy (Split-Brain)

To maximize the lifespan of the drives and the speed of the Nextcloud web interface, the storage is split into two physical domains:

- **`/dev/Web_Server` (1TB NVMe):** Hosts the Ubuntu OS, Docker engine, CasaOS, PostgreSQL database, and Redis cache.
- **`/mnt/NAS_Storage` (8TB Array):** Strictly dedicated to raw file storage.

## 2. Hardware-Level User Quotas

To prevent users (e.g., `user1`, `user2`) from hoarding space, quotas are enforced at the Linux kernel level (ext4/xfs), not just in the Nextcloud UI.

**Implementation:**

1. Created dedicated Ubuntu user accounts for each user (These also act as Samba/local network accounts).
2. Applied physical disk quotas using `setquota` (e.g., xTB hard limit for `user1`, yTB for `user2`).
3. Nextcloud is granted read/write access to these specific directories `/mnt/NAS_Storage/username`.

_Note: The physical Ubuntu accounts must never be deleted, as they manage the underlying folder ownership and quota enforcement._
