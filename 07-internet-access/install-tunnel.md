# Install Tunnel on the Pi

The Cloudflare connector runs as a Docker container on the Pi. It is configured to restart automatically so the tunnel comes back up after every reboot.

---

## Run the Tunnel Container

In your SSH terminal, take the command copied from Cloudflare and add `-d --restart unless-stopped` right after `docker run`:

```bash
sudo docker run -d --restart unless-stopped \
  cloudflare/cloudflared:latest \
  tunnel --no-autoupdate run \
  --token YOUR_TUNNEL_TOKEN_HERE
```

Replace `YOUR_TUNNEL_TOKEN_HERE` with the actual token from the Cloudflare dashboard.

**What the flags do:**

| Flag | Meaning |
|------|---------|
| `-d` | Run in the background (detached mode) |
| `--restart unless-stopped` | Automatically restart the container after a Pi reboot |

---

## Verify the Connection

1. Go back to the Cloudflare Zero Trust dashboard.
2. Click **Networks** → **Tunnels**.
3. Within a few seconds, the `Pi-NAS` tunnel status should change from **Inactive** to a green **Healthy**.

A healthy status confirms the Pi and Cloudflare are connected and the tunnel is active.

---

## Confirm the Container Is Running

On the Pi, you can confirm the container is running at any time:

```bash
sudo docker ps
```

Look for a container running `cloudflare/cloudflared`.

---

[← Cloudflare Tunnel Setup](cloudflare-tunnel.md) | [Next: Public Hostname and Routing →](public-hostname.md)
