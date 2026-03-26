# Public Hostname and Routing

With the tunnel healthy, the final networking step is telling Cloudflare which local service to forward requests to when someone visits your domain.

---

## Step 1 — Open the Tunnel Settings

1. In the Cloudflare Zero Trust dashboard, click **Networks** → **Tunnels**.
2. Click on the **Pi-NAS** tunnel to open its settings.
3. Click the **Public Hostname** tab at the top.
4. Click **Add a public hostname**.

---

## Step 2 — Configure the Hostname

Fill in the form as follows:

| Field | Value |
|-------|-------|
| Subdomain | `cloud` |
| Domain | `mynas.dev` (your domain from the dropdown) |
| Service Type | `HTTP` |
| URL | `192.168.1.100:7580` |

> 💡 Use `HTTP` (not HTTPS) for the service type. Cloudflare handles the HTTPS layer on the outside — it communicates with CasaOS internally over plain HTTP.

> 💡 The port number (`7580` in this example) is the local port that the Nextcloud container is exposed on. Find this in CasaOS by clicking the three dots on the Nextcloud icon → Settings → looking at the port mapping.

Click **Save hostname**.

---

## Result

Your NAS is now accessible at:

```
https://cloud.mynas.dev
```

Cloudflare receives the HTTPS request, routes it down the secure tunnel, and forwards it to the Nextcloud container on the Pi. The Pi responds. The user sees Nextcloud.

---

## Subdomain Suggestions

You can create multiple public hostnames on the same tunnel — each pointing to a different local service:

| Subdomain | Points To | Purpose |
|-----------|-----------|---------|
| `cloud.mynas.dev` | Nextcloud port | File sync and cloud storage |
| `home.mynas.dev` | CasaOS port (port 80) | Dashboard management |

---

[← Install Tunnel on Pi](install-tunnel.md) | [Next: Nextcloud HTTPS Config →](nextcloud-https.md)
