# Why Nextcloud

Two options were seriously considered for providing a OneDrive-style sync experience: Nextcloud and Syncthing. Here is a direct comparison.

---

## Performance

### Nextcloud

Nextcloud is a full web application. It requires a web server (Nginx), a database (MariaDB), and a PHP scripting engine. Every file sync involves a database write to update its records.

On a Raspberry Pi 5, this is manageable but noticeable. Syncing a large batch of small files (thousands of photos, for example) will put real load on the Pi's RAM and the database.

### Syncthing

Syncthing is a lightweight compiled program written in Go. It does not use a database or a web server. It also uses **block-level syncing** — if you change one layer of a large Photoshop file, Syncthing only transfers the changed blocks, not the whole file.

On a Raspberry Pi 5, Syncthing is almost invisible in terms of CPU and RAM usage. Files sync within seconds of being saved.

---

## Security

### Nextcloud

- Traditional enterprise security model
- Username and password authentication
- Two-factor authentication (2FA) support
- Detailed audit logs
- Larger attack surface due to complex PHP codebase — requires regular updates

### Syncthing

- Zero-trust cryptographic model
- No passwords — uses 56-character cryptographic device IDs
- A device must be explicitly whitelisted before it can sync
- Very small attack surface — all traffic is TLS 1.2+ encrypted
- Security relies on physical device control

---

## Features

| Feature | Nextcloud | Syncthing |
|---------|-----------|-----------|
| Web browser access | ✅ Yes | ❌ No |
| Mobile apps (iOS/Android) | ✅ Yes | ✅ Basic |
| Photo auto-backup | ✅ Yes | ⚠️ Manual setup |
| Public sharing links | ✅ Yes | ❌ No |
| User accounts with separate storage | ✅ Yes | ⚠️ Manual setup |
| Admin dashboard | ✅ Yes | ✅ Basic |
| Lightweight on Pi | ❌ No | ✅ Yes |

---

## The Decision

**Nextcloud was chosen** because this project needed:

- Internet access for multiple users via a web browser (no app install required for basic access)
- Per-user accounts with separate storage spaces
- Mobile apps for automatic photo backup
- Public sharing links

The performance trade-off on the Pi 5 is acceptable given its 8 GB of RAM.

---

[← Nextcloud Overview](README.md) | [Next: Permissions Bridge →](permissions-bridge.md)
