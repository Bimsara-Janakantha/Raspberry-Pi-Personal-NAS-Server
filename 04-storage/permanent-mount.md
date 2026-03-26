# Permanent Mounting

By default, the drive mounts you created will not survive a reboot. This step writes the mount instructions permanently into Ubuntu's master boot file (`/etc/fstab`) so the drives are always available after every restart.

---

## Step 1 — Get the UUID of the RAID Array

UUIDs are permanent identifiers for filesystems — unlike device names like `sda` which can change, a UUID always refers to the same filesystem.

```bash
sudo blkid /dev/md0
```

Example output:

```
/dev/md0: UUID="a1b2c3d4-e5f6-7890-abcd-ef1234567890" TYPE="ext4"
```

Copy the UUID string (everything inside the quotes after `UUID=`).

Do the same for the 1 TB drive:

```bash
sudo blkid /dev/sdd
```

---

## Step 2 — Edit fstab

```bash
sudo nano /etc/fstab
```

Use the arrow keys to go to the very bottom of the file and add these two lines (replace the UUIDs with your actual values):

```
UUID=a1b2c3d4-e5f6-7890-abcd-ef1234567890  /mnt/NAS_Storage  ext4  defaults,nofail  0  0
UUID=b2c3d4e5-f6a7-8901-bcde-f12345678901  /mnt/Web_Server   ext4  defaults,nofail  0  0
```

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

> 💡 The `nofail` option is important — it tells Ubuntu that if a drive is not yet ready during boot (e.g. the drives are still spinning up), keep booting rather than stopping and waiting indefinitely.

---

## Step 3 — Test the fstab Entry

Before rebooting, test that the fstab entries work correctly:

```bash
sudo mount -a
```

If no errors appear, the entries are valid.

---

## Step 4 — Save the RAID Configuration

Tell `mdadm` to write the current array configuration to its config file so it remembers the array name on every boot:

```bash
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
```

Then update the boot image so Ubuntu reads this configuration during early boot:

```bash
sudo update-initramfs -u
```

---

## Step 5 — Restart CasaOS

Restart CasaOS so its dashboard picks up the newly mounted drives:

```bash
sudo systemctl restart casaos.service
```

The drives will now appear correctly in the CasaOS Files app every time the Pi boots.

---

[← CasaOS Installation](casaos-install.md) | [Next: Debug — RAID Reboot Issues →](debug-raid-reboot.md)
