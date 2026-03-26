# Storage Quotas

Disk quotas enforce a hard limit on how much space each user can consume on the RAID array. Even if Nextcloud or any other software malfunctions, the Linux kernel itself will physically refuse to let a user write past their limit.

---

## Step 1 — Enable Quota Tracking in fstab

Open the fstab file:

```bash
sudo nano /etc/fstab
```

Find the line for the RAID array (`/mnt/NAS_Storage`) and update the mount options from `defaults,nofail` to `defaults,nofail,usrquota,grpquota`:

```
UUID=your-uuid-here  /mnt/NAS_Storage  ext4  defaults,nofail,usrquota,grpquota  0  0
```

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

## Step 2 — Upgrade to Modern Journaled Quotas

The modern approach bakes quota tracking invisibly into the ext4 filesystem itself, rather than using legacy external tracking files. This is more reliable and requires no maintenance.

```bash
sudo umount /mnt/NAS_Storage
sudo tune2fs -O quota /dev/md0
sudo mount /mnt/NAS_Storage
```

> 💡 This only needs to be done once. After this, the kernel tracks quota usage automatically in the background forever — no manual `quotacheck` needed.

---

## Step 3 — Install Quota Tools

```bash
sudo apt update && sudo apt install quota quotatool -y
```

---

## Step 4 — Apply the Hard Limits

Set limits for each user. Adjust the sizes to match your storage plan:

```bash
# Alice — 5 TB limit
sudo quotatool -u alice -bq 5000G -l 5000G /mnt/NAS_Storage

# Bob — 1.5 TB limit
sudo quotatool -u bob -bq 1500G -l 1500G /mnt/NAS_Storage
```

The `-bq` flag sets the soft warning threshold and `-l` sets the hard brick-wall limit. Both are set to the same value here, meaning the user hits the wall immediately with no grace period.

---

## Step 5 — Verify the Limits

```bash
sudo repquota -us /mnt/NAS_Storage
```

Example output:

```
*** Report for user quotas on device /dev/md0
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
alice     --       0 5120000000 5120000000              0     0     0
bob       --       0 1536000000 1536000000              0     0     0
```

Both users show their respective limits and 0 bytes used — ready for data.

---

[← Private Vaults](private-vaults.md) | [Next: Samba Sharing →](samba.md)
