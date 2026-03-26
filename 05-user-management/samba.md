# Samba Sharing

Samba broadcasts the private vaults over the local Wi-Fi so users can access them from Windows File Explorer — without needing any special software installed.

---

## Step 1 — Back Up the Samba Config

Always make a backup before editing the main Samba configuration file:

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

---

## Step 2 — Edit the Samba Configuration

```bash
sudo nano /etc/samba/smb.conf
```

Scroll to the very bottom of the file and add the following two share blocks:

```ini
[Alice_Vault]
   path = /mnt/NAS_Storage/alice
   valid users = alice
   read only = no
   browseable = yes
   create mask = 0700
   directory mask = 0700

[Bob_Vault]
   path = /mnt/NAS_Storage/bob
   valid users = bob
   read only = no
   browseable = yes
   create mask = 0700
   directory mask = 0700
```

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

**What these settings mean:**

| Setting | Meaning |
|---------|---------|
| `valid users` | Only this specific user can connect to this share |
| `browseable = yes` | The share appears in the network browser |
| `create mask = 0700` | Any file created inside is private to the owner only |
| `directory mask = 0700` | Any folder created inside is private to the owner only |

---

## Step 3 — Set Folder Permissions for Samba

Samba needs the folder to be writable by the user. Confirm ownership is correct:

```bash
sudo chown -R alice:alice /mnt/NAS_Storage/alice
sudo chown -R bob:bob     /mnt/NAS_Storage/bob
sudo chmod -R 700 /mnt/NAS_Storage/alice
sudo chmod -R 700 /mnt/NAS_Storage/bob
```

---

## Step 4 — Restart Samba

```bash
sudo systemctl restart smbd
```

---

## Test the Connection

From a Windows PC on the same network, open File Explorer and type in the address bar:

```
\\pinas.local
```

Or use the IP address directly:

```
\\192.168.1.100
```

When prompted for credentials, enter the user's Samba username and password. You should see their vault folder and be able to read and write files.

---

[← Storage Quotas](quotas.md) | [Next: Map Network Drives →](map-drives.md)
