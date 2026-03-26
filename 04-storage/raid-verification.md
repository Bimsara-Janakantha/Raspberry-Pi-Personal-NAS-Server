# RAID Verification

Before handing the system over to users, four diagnostic commands were run to confirm the array is correctly configured, fully synced, and properly mounted.

---

## Check 1 — Quick Health Check

```bash
cat /proc/mdstat
```

**What to look for:**

- `active raid5` — the array is running
- All three drive names listed (e.g. `sda`, `sdb`, `sdc`)
- `[3/3] [UUU]` — all three drives are up and the sync is complete

```
Personalities : [raid6] [raid5] [raid4]
md0 : active raid5 sda[0] sdb[1] sdc[2]
      7813772288 blocks super 1.2 level 5, 512k chunk, ...
      [3/3] [UUU]
```

✅ `[3/3] [UUU]` is the goal. It means the RAID 5 array is fully healthy.

---

## Check 2 — Deep Configuration Check

```bash
sudo mdadm --detail /dev/md0
```

**What to look for:**

| Field | Expected Value |
|-------|---------------|
| Raid Level | `raid5` |
| Array Size | ~7813772288 (7.28 TiB / 8 TB) |
| State | `clean` |
| Active Devices | 3 |
| All drives listed | As `active sync` devices |

---

## Check 3 — Capacity and Mount Check

```bash
df -h | grep md0
```

**What to look for:**

- **Size:** approximately `7.3T`
- **Mounted on:** `/mnt/NAS_Storage`

Example output:

```
/dev/md0        7.3T   28K  6.9T   1% /mnt/NAS_Storage
```

If `/mnt/NAS_Storage` does not appear, CasaOS cannot use the drive.

---

## Check 4 — Visual Hardware Tree

```bash
lsblk
```

**What to look for:**

- Each of the three 4 TB drives (`sda`, `sdb`, `sdc`) branches down to `md0`
- `md0` shows `/mnt/NAS_Storage` in the MOUNTPOINTS column
- `sdd` shows `/mnt/Web_Server`

Example output:

```
NAME        SIZE TYPE  MOUNTPOINTS
sda         3.6T disk
└─md0       7.3T raid5 /mnt/NAS_Storage
sdb         3.6T disk
└─md0
sdc         3.6T disk
└─md0
sdd       931.5G disk  /mnt/Web_Server
mmcblk0    59.5G disk
└─mmcblk0p1       part /
```

---

## All Four Checks Passed?

If every check above looks correct, the storage foundation is solid and ready for user configuration.

- ✅ `[3/3] [UUU]` in mdstat
- ✅ `clean` state in mdadm detail
- ✅ ~7.3T visible and mounted at `/mnt/NAS_Storage`
- ✅ All three drives branching to `md0` in lsblk

---

[← Debug — RAID Reboot Issues](debug-raid-reboot.md) | [Back to Storage Overview →](README.md)
