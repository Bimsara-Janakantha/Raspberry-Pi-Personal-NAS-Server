# Cloudflare Tunnel Setup

With the domain active on Cloudflare, the next step is creating a Zero Trust tunnel — this is the secure channel between Cloudflare's network and your Raspberry Pi.

---

## Step 1 — Open Zero Trust

In the Cloudflare dashboard left sidebar, click **Zero Trust**.

If this is your first time, you will be asked to choose a plan. Select the **Free plan**. A credit card is required on file to prevent abuse, but you will not be charged for the tunnel.

---

## Step 2 — Create the Tunnel

1. In the Zero Trust dashboard, click **Networks** → **Tunnels** in the left sidebar.
2. Click **Create a tunnel**.
3. Select **Cloudflared** as the connector type and click **Next**.
4. Give your tunnel a name — something like `Pi-NAS` — and click **Save tunnel**.

---

## Step 3 — Copy the Docker Token

On the next screen, Cloudflare asks how you want to install the connector.

1. Click the **Docker** tab.
2. You will see a command that looks like:
   ```
   docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token YOUR_LONG_TOKEN_HERE
   ```
3. **Copy this entire command.** The long string at the end is your unique tunnel token — keep it safe.

---

## Step 4 — Verify the Tunnel Is Waiting

Back on the Tunnels list, your `Pi-NAS` tunnel will show a status of **Inactive**. This is expected — it is waiting for the Pi to connect. That happens in the next step.

---

[← Domain Name Setup](domain-setup.md) | [Next: Install Tunnel on Pi →](install-tunnel.md)
