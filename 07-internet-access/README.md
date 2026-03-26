# 07 Internet Access via Cloudflare Tunnel

This section covers making the NAS accessible securely from anywhere on the internet — without opening any ports on the home router or exposing the home IP address.

---

## Sub-Sections

| File | Content |
|------|---------|
| [Solution Comparison](solution-comparison.md) | Comparing Tailscale, OpenVPN, Cloudflare, and Dynamic DNS |
| [Domain Name Setup](domain-setup.md) | Purchasing a domain and pointing it to Cloudflare |
| [Cloudflare Tunnel Setup](cloudflare-tunnel.md) | Creating the Zero Trust tunnel |
| [Install Tunnel on Pi](install-tunnel.md) | Running the Cloudflare connector as a Docker container |
| [Public Hostname and Routing](public-hostname.md) | Mapping your domain to the Nextcloud container |
| [Nextcloud HTTPS Config](nextcloud-https.md) | Updating config.php for the public domain |

---

[← Back to Nextcloud](../06-nextcloud/README.md) | [Next: Client Setup →](../08-client-setup/README.md)
