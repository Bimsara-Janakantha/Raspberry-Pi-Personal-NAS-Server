# Domain Name Setup

A domain name is the public address users will type to access the NAS from the internet (for example: `cloud.mynas.dev`). This needs to be purchased and then handed over to Cloudflare's nameservers for management.

---

## Step 1 — Purchase a Domain

Domain names can be purchased from any registrar. Some popular options:

| Registrar | Notes |
|-----------|-------|
| [Namecheap](https://www.namecheap.com) | Affordable, clear pricing |
| [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/) | At-cost pricing, tightest integration |
| [Name.com](https://www.name.com) | Easy to use |
| [Porkbun](https://porkbun.com) | Frequently discounted |

**Cost:** Expect to pay $4–$10 per year for common extensions like `.dev`, `.net`, or `.io`.

> 💡 You can use Cloudflare as your registrar directly, which simplifies the nameserver step — your domain is already managed by Cloudflare from day one.

---

## Step 2 — Add Your Domain to Cloudflare

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) and create a free account (or log in).
2. Click **Add a Site**.
3. Enter your domain name (e.g. `mynas.dev`) and click **Continue**.
4. Select the **Free ($0/month)** plan and click **Continue**.
5. Cloudflare scans your existing DNS records. Click **Continue** past this.
6. Cloudflare displays two **nameserver addresses** — they look something like:
   ```
   dana.ns.cloudflare.com
   eric.ns.cloudflare.com
   ```
   Copy both of these.

---

## Step 3 — Update Nameservers at Your Registrar

Log into whichever registrar you bought the domain from and navigate to the domain's **Nameserver** or **DNS** settings.

Replace the default nameservers with the two Cloudflare nameservers you copied in Step 2.

Save the changes.

> ⏱️ Nameserver propagation usually takes **5 to 30 minutes**. Back on the Cloudflare dashboard, click **Check nameservers** until you see a green success message.

---

## Step 4 — Confirm Cloudflare Is Active

Once propagation is complete, your Cloudflare dashboard will show the domain as **Active**. You are now ready to create the tunnel.

---

[← Solution Comparison](solution-comparison.md) | [Next: Cloudflare Tunnel Setup →](cloudflare-tunnel.md)
