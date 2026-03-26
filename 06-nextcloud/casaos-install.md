# Installing Nextcloud via CasaOS

Nextcloud is installed through the CasaOS App Store, but with custom volume paths set before the container starts. This is the "split-brain" approach — the app lives on the 1 TB web server drive, while the user data stays on the 8 TB RAID array.

---

## Why Custom Paths Matter

By default, CasaOS would install Nextcloud's data into `/DATA/AppData/nextcloud/` on the SD card. This is a problem because:

- The SD card has limited space and is not suitable for application data
- Nextcloud would store thumbnails, logs, and database files on the SD card, which will wear it out quickly
- It separates the app brain from the user storage, which is harder to back up

The fix is to redirect everything to the 1 TB drive before the container ever starts.

---

## Steps

### 1. Open the CasaOS App Store

Navigate to the CasaOS web dashboard and open the **App Store**.

### 2. Find Nextcloud

Search for Nextcloud and click it. **Do not click the default Install button yet.**

Click **Custom Install** (or the settings icon) to open the volume configuration screen.

### 3. Redirect the Core Volumes

In the **Volumes** section, you will see several tab entries (nextCloud, cron, Pg_data, etc.). For each tab:

- Find the **Host Path** that points to `/DATA/AppData/nextcloud...`
- Change it to `/mnt/Web_Server/Nextcloud_Brain`

### 4. Add the NAS Storage Volume

In the **nextCloud** tab and the **cron** tab, click **+ Add** to create a new volume mapping:

| Field | Value |
|-------|-------|
| Host path | `/mnt/NAS_Storage` |
| Container path | `/NAS_Storage` |

This gives the Nextcloud container a window into the RAID array at the path `/NAS_Storage` inside the container.

### 5. Install

Click **Save** and then install. Let the container pull and start — this usually takes 1 to 3 minutes.

---

## What Was Stored Where

| Data | Location |
|------|----------|
| Nextcloud application files | `/mnt/Web_Server/Nextcloud_Brain` |
| Database (PostgreSQL) | `/mnt/Web_Server/Nextcloud_Brain` |
| Thumbnails and cache | `/mnt/Web_Server/Nextcloud_Brain` |
| Alice's actual files | `/mnt/NAS_Storage/alice` (via volume mount) |
| Bob's actual files | `/mnt/NAS_Storage/bob` (via volume mount) |

---

[← Permissions Bridge](permissions-bridge.md) | [Next: Initial Setup →](initial-setup.md)
