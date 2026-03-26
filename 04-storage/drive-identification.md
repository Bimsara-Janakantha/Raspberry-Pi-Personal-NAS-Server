# Drive Identification

Before running any formatting or RAID commands, you must confirm the exact names that Ubuntu has assigned to your drives. Guessing is not an option — selecting the wrong disk will destroy data, including the OS boot drive.

---

## Run lsblk

```bash
lsblk
```

Example output:

```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
mmcblk0     179:0    0  59.5G  0 disk
└─mmcblk0p1 179:1    0  59.5G  0 part /boot/firmware
mmcblk0p2   179:2    0  58.5G  0 part /
sda           8:0    0   3.6T  0 disk
sdb           8:16   0   3.6T  0 disk
sdc           8:32   0   3.6T  0 disk
sdd           8:48   0 931.5G  0 disk
```

---

## How to Read the Output

| Name | What It Is | Action |
|------|-----------|--------|
| `mmcblk0` | Your MicroSD boot card | **Do not touch** |
| `sda`, `sdb`, `sdc` | Your three 4 TB SAS drives | These go into the RAID array |
| `sdd` | Your 1 TB SATA drive | This becomes the web server drive |

> 💡 The exact letters (`sda`, `sdb`, etc.) may be different on your system. Always use the size to identify each drive — a 4 TB drive shows as approximately 3.6 T in lsblk.

---

## Write These Down

Before continuing, write down or copy the names of:

- Your **three 4 TB drives** (e.g. `sda`, `sdb`, `sdc`)
- Your **1 TB drive** (e.g. `sdd`)

You will need these exact names for every command in the RAID setup steps.

---

[← Storage Overview](README.md) | [Next: RAID 5 Setup →](raid-setup.md)
