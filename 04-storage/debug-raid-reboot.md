# Debug — RAID Reboot Issues

Real-world NAS builds rarely go perfectly on the first reboot. This page documents the specific issues encountered during this build and exactly how they were fixed.

---

## Issue 1 — Array Renamed to md127 After Reboot

### What Happened

After the first reboot, the RAID array appeared as `/dev/md127` instead of `/dev/md0`.

### Why It Happens

The Pi boots very fast. During early boot, Ubuntu's startup process scanned the drives and found RAID signatures on them, but could not find a configuration file saying what the array should be named. To avoid conflicts, Linux automatically assigns unidentified arrays a high number starting at 127.

### The Fix

**Step 1 — Stop the temporary array:**

```bash
sudo mdadm --stop /dev/md127
```

**Step 2 — Reassemble it with the correct name:**

```bash
sudo mdadm --assemble /dev/md0 /dev/sda /dev/sdb /dev/sdc
```

**Step 3 — Save the configuration:**

```bash
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
```

**Step 4 — Update the boot image (crucial — most guides skip this):**

```bash
sudo update-initramfs -u
```

**Step 5 — Reboot and verify:**

```bash
sudo reboot now
```

After logging back in, run `lsblk`. The array should now permanently show as `md0`.

---

## Issue 2 — Array Shows as "inactive" After Reboot

### What Happened

After a subsequent reboot, `cat /proc/mdstat` showed the array as `inactive`.

### Why It Happens

The Pi booted faster than the hard drives could spin up and respond. When Ubuntu tried to assemble the RAID array and the drives were not yet ready, it marked the array as inactive to protect your data rather than proceeding with missing drives.

### The Fix

**Step 1 — Stop the paused array:**

```bash
sudo mdadm --stop /dev/md0
```

**Step 2 — Force reassembly:**

```bash
sudo mdadm --assemble --force /dev/md0 /dev/sda /dev/sdb /dev/sdc
```

**Step 3 — Verify it resumed:**

```bash
cat /proc/mdstat
```

You should see `active raid5` and (if a sync was still in progress) the rebuild progress bar.

**Step 4 — Reload fstab mounts:**

```bash
sudo mount -a
```

---

## Issue 3 — Array Shows as "auto-read-only"

### What Happened

The array assembled but appeared in `auto-read-only` mode, meaning no data could be written to it.

### Why It Happens

When Ubuntu assembles a RAID array during boot and no process has actively requested write access yet, it places it in a protective read-only state. This also pauses any ongoing background sync.

### The Fix

Wake up the array and switch it to read/write mode:

```bash
sudo mdadm --readwrite /dev/md0
```

Verify the sync resumed:

```bash
cat /proc/mdstat
```

Then reload the mounts:

```bash
sudo mount -a
```

---

## Issue 4 — One Drive Shows as a Spare ([UU_])

### What Happened

`cat /proc/mdstat` showed `[UU_]` — two drives active and one listed as a spare.

### Why It Happens

After a forced reassembly following a read-only lock, the third drive was sitting on the bench waiting for permission to rejoin rather than automatically reintegrating.

### The Fix

Manually add the drive back to the array:

```bash
sudo mdadm --manage /dev/md0 --add /dev/sdc
```

Check `cat /proc/mdstat` again — the rebuild progress bar should now appear and the status will change from `[UU_]` to `[UUU]` once complete.

---

> 💡 **Long-term fix for spin-up delays:** If the inactive issue recurs on reboots, a boot delay can be added to give drives more time to spin up before Ubuntu tries to assemble the array. This is covered in the maintenance notes in the [Project Summary](../09-summary/README.md).

---

[← Permanent Mounting](permanent-mount.md) | [Next: RAID Verification →](raid-verification.md)
