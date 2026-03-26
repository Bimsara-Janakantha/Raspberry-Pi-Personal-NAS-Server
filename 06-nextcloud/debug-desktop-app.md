# Debug — Desktop App Issues

The web browser and mobile apps connected to Nextcloud without problems, but the Windows desktop app showed a pink security warning and refused to connect. This page explains what caused it and exactly how it was fixed.

---

## The Problem

The Nextcloud desktop app is stricter than the browser about HTTPS. Because Cloudflare was wrapping the connection in HTTPS on the outside while CasaOS was running plain HTTP on the inside, the desktop app detected the mismatch and blocked the connection.

Additionally, the Docker container's automatic startup script was silently overwriting the `config.php` file on every restart, undoing any manual fixes.

---

## Fix 1 — Update config.php

Open the Nextcloud config file:

```bash
sudo nano /mnt/Web_Server/Nextcloud_Brain/html/config/config.php
```

Add or update these three lines near the bottom of the file (above the closing `);`):

```php
'overwriteprotocol' => 'https',
'overwritehost'     => 'cloud.mynas.dev',
'overwrite.cli.url' => 'https://cloud.mynas.dev',
```

Also add the trusted proxies array to allow the Cloudflare Docker container to forward requests:

```php
'trusted_proxies' =>
  array (
    0 => '127.0.0.1',
    1 => '10.0.0.0/8',
    2 => '172.16.0.0/12',
    3 => '192.168.0.0/16',
  ),
```

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

Restart the Nextcloud container via CasaOS (three dots → Restart) and wait 30–60 seconds.

---

## Fix 2 — Set Environment Variables (Permanent Fix)

The real root cause was that the Docker startup script was regenerating `config.php` and overwriting the settings above on every container restart. The permanent fix is to set the values as environment variables in CasaOS so they are applied automatically at boot.

1. Go to the CasaOS dashboard.
2. Click the **three dots** on the Nextcloud icon → **Settings**.
3. Scroll to the **Environment Variables** section.
4. Add or update these three variables:

| Key | Value |
|-----|-------|
| `OVERWRITEPROTOCOL` | `https` |
| `OVERWRITEHOST` | `cloud.mynas.dev` |
| `OVERWRITECLIURL` | `https://cloud.mynas.dev` |

5. Click **Save**.

The container will rebuild with these values locked in permanently.

---

## Finding the Exact Docker Proxy IP (Optional — For Tighter Security)

If you want to restrict the `trusted_proxies` to only the exact Cloudflare container IP instead of the broad subnet:

```bash
# Step 1 — Get the Cloudflare container ID
sudo docker ps

# Step 2 — Inspect its IP address
sudo docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' CONTAINER_ID
```

Then update `trusted_proxies` in `config.php` to only include that specific IP.

Alternatively, search the Nextcloud log for blocked proxy messages:

```bash
sudo grep "Trusted proxy" /mnt/Web_Server/Nextcloud_Brain/html/data/nextcloud.log | tail -n 5
```

---

[← Mount Options](mount-options.md) | [Back to Nextcloud Overview →](README.md)
