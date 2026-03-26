# Connect User Vaults

This is the step where everything comes together. The physical quota-locked folders on the 8 TB RAID array are linked into Nextcloud so each user sees their storage as their personal cloud drive.

---

## Step 1 — Enable External Storage Support

By default, Nextcloud only reads files from inside its own database directory. The External Storage app needs to be enabled first.

1. Click the **profile icon** → **Apps**.
2. In the left sidebar, click **Disabled apps** (or search for "External storage").
3. Find **External storage support** and click **Enable**.

---

## Step 2 — Open External Storage Settings

1. Click the **profile icon** → **Administration settings**.
2. In the left sidebar, scroll down to the **Administration** section and click **External storages**.

> ⚠️ Make sure you are in the **Administration** section, not the **Personal** section.

---

## Step 3 — Link Alice's Vault

Fill in the form as follows:

| Field | Value |
|-------|-------|
| Folder name | `/` |
| External storage | Local |
| Authentication | None |
| Configuration | `/NAS_Storage/alice` |
| Restricted to | `alice` |

Click the **Save** (tick) button on the right.

> 💡 Setting the folder name to `/` means Alice sees the root of her vault immediately when she logs in — not a subfolder. She cannot navigate above this point.

> ⚠️ The path is `/NAS_Storage/alice` — **not** `/mnt/NAS_Storage/alice`. Inside the Docker container, the RAID array was mounted at `/NAS_Storage` (not `/mnt/NAS_Storage`).

---

## Step 4 — Link Bob's Vault

Add a second entry:

| Field | Value |
|-------|-------|
| Folder name | `/` |
| External storage | Local |
| Authentication | None |
| Configuration | `/NAS_Storage/bob` |
| Restricted to | `bob` |

Click **Save**.

---

## Verify

A **green circle** next to each entry confirms Nextcloud can access the folder successfully.

Log out of the admin account and log back in as Alice. Her personal vault should appear immediately as the root of her Nextcloud drive.

---

[← Initial Setup](initial-setup.md) | [Next: Mount Options →](mount-options.md)
