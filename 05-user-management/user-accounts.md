# User Accounts

Two sets of accounts need to be created for each user: a Linux system account (for file ownership and quotas) and a Samba account (for local network access).

---

## Create Linux System Accounts

```bash
sudo adduser alice
sudo adduser bob
```

The command will prompt you to set a password for each user and optionally fill in their full name and other details. You can press Enter to skip the optional fields.

> 💡 Use real first names or consistent usernames. These are used for folder names, Samba shares, and Nextcloud accounts — keeping them consistent avoids confusion later.

---

## Add Users to Samba

Linux network sharing (Samba) maintains its own separate password registry. Even though you just set a password for each user in the step above, you need to register them with Samba separately:

```bash
sudo smbpasswd -a alice
sudo smbpasswd -a bob
```

You can use the same passwords as their Linux accounts, or set different ones.

---

## Lock Down the Web Server Drive

Before setting up user folders, lock the 1 TB web server drive so only the admin account can access it:

```bash
sudo chown -R admin:admin /mnt/Web_Server
sudo chmod -R 700 /mnt/Web_Server
```

`chmod 700` means: the owner has full read, write, and execute access — and everyone else has zero access. The drive is completely invisible to other users.

---

[← Architecture Overview](architecture.md) | [Next: Private Vaults →](private-vaults.md)
