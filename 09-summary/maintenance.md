# Maintenance Reference

A quick-reference guide for keeping the NAS healthy day to day.

---

## RAID Health

### Check array status

```bash
cat /proc/mdstat
```

Healthy output shows `active raid5` and `[3/3] [UUU]`.

### Deep array detail

```bash
sudo mdadm --detail /dev/md0
```

State should be `clean`. During a sync it will show `clean, degraded, recovering` — this is normal.

### Check disk usage and mount point

```bash
df -h | grep md0
```

---

## RAID Recovery (After Reboot Issues)

If the array shows as `inactive` or is mounted as `md127`:

```bash
# Stop the paused array
sudo mdadm --stop /dev/md0

# Force reassemble
sudo mdadm --assemble --force /dev/md0 /dev/sda /dev/sdb /dev/sdc

# Remove read-only lock
sudo mdadm --readwrite /dev/md0

# Reload fstab mounts
sudo mount -a
```

If a drive shows as a spare (`[UU_]`), add it back:

```bash
sudo mdadm --manage /dev/md0 --add /dev/sdc
```

---

## Storage Quotas

### View current usage per user

```bash
sudo repquota -us /mnt/NAS_Storage
```

### Change a user's quota

```bash
# Example: increase Alice to 6 TB
sudo quotatool -u alice -bq 6000G -l 6000G /mnt/NAS_Storage
```

---

## User Management

### Add a new user

```bash
sudo adduser newuser
sudo smbpasswd -a newuser
sudo mkdir /mnt/NAS_Storage/newuser
sudo chown newuser:33 /mnt/NAS_Storage/newuser
sudo chmod 770 /mnt/NAS_Storage/newuser
sudo quotatool -u newuser -bq 1000G -l 1000G /mnt/NAS_Storage
```

Then add a Samba share block and a Nextcloud external storage entry for the new user.

### Change a Samba password

```bash
sudo smbpasswd alice
```

---

## Health Monitor

### Check service status

```bash
sudo systemctl status nashealth.service
```

### View real-time temperature and disk log

```bash
tail -f /usr/local/bin/heath/nas_status.log
```

### Restart the monitor

```bash
sudo systemctl restart nashealth.service
```

### Check current CPU temperature manually

```bash
cat /sys/class/thermal/thermal_zone0/temp
```

Divide by 1000 for degrees Celsius. Values above 80°C under sustained load are worth investigating.

---

## Nextcloud

### Restart the Nextcloud container

From the CasaOS dashboard: three dots on Nextcloud icon → **Restart**.

Or via command line:

```bash
sudo docker ps                          # find the container name
sudo docker restart <container_name>
```

### Force Nextcloud to rescan files

If files added via Samba are not appearing in Nextcloud:

```bash
sudo docker exec -u www-data <nextcloud_container> php occ files:scan --all
```

### View Nextcloud logs

```bash
tail -f /mnt/Web_Server/Nextcloud_Brain/html/data/nextcloud.log
```

---

## Cloudflare Tunnel

### Check tunnel container is running

```bash
sudo docker ps | grep cloudflared
```

### Restart the tunnel

```bash
sudo docker restart <cloudflared_container_name>
```

The tunnel reconnects automatically. Status changes back to **Healthy** in the Cloudflare dashboard within seconds.

---

## System Updates

### Update Ubuntu packages

```bash
sudo apt update && sudo apt upgrade -y
```

### Update CasaOS

Updates are available through the CasaOS web dashboard. A notification appears when updates are ready.

### Update Docker containers

In CasaOS, click the three dots on any container → **Update** (if available).

---

## Useful One-Liners

| Task | Command |
|------|---------|
| See all running containers | `sudo docker ps` |
| Check disk usage (all mounts) | `df -h` |
| Check hardware drives | `lsblk` |
| View system memory usage | `free -h` |
| View CPU and process load | `htop` |
| Check uptime | `uptime` |
| View recent system logs | `sudo journalctl -n 50 --no-pager` |

---

[← What Was Built](what-was-built.md) | [Back to Summary Overview →](README.md)
