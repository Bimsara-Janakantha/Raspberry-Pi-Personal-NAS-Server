# Solution Comparison

There are several ways to expose a home NAS to the internet. Each has different trade-offs in terms of cost, security, and user experience.

---

> ⚠️ **Golden rule of home NAS networking:** Never open Samba (SMB) ports directly to the internet. SMB ports are constantly scanned by attackers and will be compromised. Always route access through a secure tunnel or VPN.

---

## Comparison Table

| Method | Cost | Security | User Experience | Port Forwarding Required |
|--------|------|----------|-----------------|--------------------------|
| **Tailscale** | Free | High — WireGuard mesh VPN | VPN app required on each device | No |
| **OpenVPN** | Free | High | Complex to set up, client app required | Yes (or hosted server) |
| **Cloudflare Zero Trust Tunnel** | Free + ~$10/yr domain | Elite — home IP fully hidden | Standard web browser, no app needed | No |
| **Dynamic DNS + Reverse Proxy** | Free | Medium | Browser access, home IP partially exposed | Yes |

---

## Why Cloudflare Zero Trust Was Chosen

### How It Works

Instead of opening a hole in the router, a tiny Cloudflare connector app runs on the Pi. It creates a secure, **outbound-only** tunnel to Cloudflare's global network. Cloudflare then attaches a custom domain to the tunnel and handles the SSL certificate automatically.

### The User Experience

Users type `https://cloud.mynas.dev` into any web browser. Cloudflare routes the request down the tunnel to the Pi. The Pi responds. The user never needs to install a VPN app.

### Why It Is Secure

- The home IP address is completely hidden from the internet
- All router ports remain closed — no port forwarding needed
- Cloudflare handles SSL termination with a valid certificate
- Cloudflare's network absorbs DDoS attacks before they reach the Pi

### The Cost

The Cloudflare tunnel service itself is **100% free**. The only cost is a domain name, which runs approximately **$4–$10 per year** depending on the extension (`.dev`, `.net`, `.io`, etc.).

---

## What About Tailscale?

Tailscale was a strong second choice. It is completely free, uses WireGuard (excellent security), and would work seamlessly with the Samba shares already set up.

The deciding factor was user experience: Tailscale requires users to install the Tailscale app on every device they want to access the NAS from. If Alice is at a friend's house and wants to grab a file, she cannot do it from her friend's computer.

With Cloudflare, any browser anywhere in the world works.

---

[← Internet Access Overview](README.md) | [Next: Domain Name Setup →](domain-setup.md)
