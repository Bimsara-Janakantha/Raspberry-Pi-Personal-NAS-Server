# Map Network Drives on Windows

Instead of typing `\\pinas.local` every time, users can pin their vault as a permanent drive letter in Windows. It will appear alongside their C: drive and reconnect automatically every time they log in.

---

## Steps (Per User, on Their Windows PC)

1. Open **File Explorer** and click **This PC** in the left sidebar.
2. Click the **three dots (...)** at the top of the window and select **Map network drive**.
3. Choose a drive letter (e.g. `Z:` for Alice, `V:` for Bob).
4. In the **Folder** field, enter the path to the personal vault:
   - Alice: `\\pinas.local\Alice_Vault`
   - Bob: `\\pinas.local\Bob_Vault`
5. Check the box for **Reconnect at sign-in**.
6. Click **Finish**.
7. If prompted for credentials, enter the Samba username and password, and check **Remember my credentials**.

---

## Result

The vault now appears in File Explorer as a drive letter — just like a USB hard drive, but stored on the NAS over the network.

```
This PC
├── Local Disk (C:)
├── Alice_Vault (Z:)   ← 8TB NAS vault
└── ...
```

Users can drag and drop files, open documents directly, and save files to their vault from any application — exactly like a local drive.

---

## Troubleshooting

**Cannot see the share:**
- Make sure the Pi is on the same network.
- Try connecting by IP address instead of hostname: `\\192.168.1.100\Alice_Vault`
- Confirm Samba is running: `sudo systemctl status smbd`

**Access denied:**
- Double-check the Samba password was set with `sudo smbpasswd -a alice`
- Confirm the folder ownership: `ls -la /mnt/NAS_Storage/`

**Drive disappears after reboot:**
- Ensure **Reconnect at sign-in** was checked when mapping the drive.

---

[← Samba Sharing](samba.md) | [Back to User Management Overview →](README.md)
