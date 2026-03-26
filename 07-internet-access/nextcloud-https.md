# Nextcloud HTTPS Configuration

Even though Cloudflare serves the site over HTTPS externally, Nextcloud itself does not automatically know it is running behind a secure proxy. Without extra config, the Nextcloud desktop app will show a security warning and refuse to connect. This step fixes that permanently.

---

## Step 1 — Add the Public Domain to Trusted Domains

Open the config file:

```bash
sudo nano /mnt/Web_Server/Nextcloud_Brain/html/config/config.php
```

Update the `trusted_domains` array to include your public domain:

```php
'trusted_domains' =>
  array (
    0 => 'localhost',
    1 => '192.168.1.100',
    2 => 'pinas.local',
    3 => 'cloud.mynas.dev',
  ),
```

---

## Step 2 — Add the HTTPS Override Settings

Still in `config.php`, add the following lines near the bottom of the file (above the closing `);`):

```php
'overwriteprotocol' => 'https',
'overwritehost'     => 'cloud.mynas.dev',
'overwrite.cli.url' => 'https://cloud.mynas.dev',
```

**What each line does:**

| Setting | Purpose |
|---------|---------|
| `overwriteprotocol` | Tells Nextcloud it is being served over HTTPS, even though CasaOS uses HTTP internally |
| `overwritehost` | Tells Nextcloud its true public hostname so it does not redirect users to a local IP |
| `overwrite.cli.url` | Ensures background sync tasks and app connections use the correct public URL |

---

## Step 3 — Add Trusted Proxies

Add the trusted proxies array to allow the Cloudflare Docker container to forward requests without being blocked:

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

---

## Step 4 — Restart the Nextcloud Container

Nextcloud caches its config aggressively. Just saving the file is not always enough — you need to force the container to re-read it.

1. Go to the CasaOS dashboard.
2. Click the **three dots** on the Nextcloud icon → **Restart**.
3. Wait 30–60 seconds for all services to come back up.

---

## Step 5 — Set Environment Variables (Prevents Regression)

The Docker startup script can overwrite `config.php` on container rebuilds. Lock the HTTPS settings permanently as environment variables:

1. Click the **three dots** on Nextcloud → **Settings** → **Environment Variables**.
2. Add or update:

| Key | Value |
|-----|-------|
| `OVERWRITEPROTOCOL` | `https` |
| `OVERWRITEHOST` | `cloud.mynas.dev` |
| `OVERWRITECLIURL` | `https://cloud.mynas.dev` |

3. Click **Save**.

---

## Final Test

Disconnect from your home Wi-Fi (use a mobile hotspot to simulate being outside the home network) and visit:

```
https://cloud.mynas.dev
```

You should see the Nextcloud login page served over HTTPS with a valid certificate.

---

[← Public Hostname and Routing](public-hostname.md) | [Back to Internet Access Overview →](README.md)
