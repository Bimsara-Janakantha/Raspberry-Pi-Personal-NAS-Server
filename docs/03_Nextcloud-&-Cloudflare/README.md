# ☁️ Part 3 — Nextcloud Deployment & Cloudflare Zero Trust

This section covers deploying the Nextcloud application stack, connecting it to your storage drives, exposing it securely to the internet without port forwarding, and fixing the reverse-proxy issues that break the Windows Desktop client.

---

## 📂 Documents in This Section

| # | File | Description |
|---|------|-------------|
| 3.1 | [nextcloud-docker-setup.md](./3.1-nextcloud-docker-setup.md) | CasaOS deployment, container stack, and volume mappings |
| 3.2 | [cloudflare-tunnel.md](./3.2-cloudflare-tunnel.md) | Zero Trust tunnel setup — secure remote access without port forwarding |
| 3.3 | [reverse-proxy-fix.md](./3.3-reverse-proxy-fix.md) | Fixing the Windows Desktop client "Pink Screen" error |

---

## 🗺️ How the Pieces Connect

```
  [ Your Phone / Laptop ]
         │
         │  HTTPS request to cloud.justinnas.dev
         ▼
  [ Cloudflare Edge ]
         │
         │  Encrypted Zero Trust Tunnel
         ▼
  [ cloudflared container on Pi ]
         │
         │  Internal forward → http://192.168.2.245:7580
         ▼
  ┌──────────────────────────────────────────────┐
  │           Raspberry Pi 5 NAS                 │
  │                                              │
  │  ┌─────────────┐   ┌──────┐   ┌──────────┐  │
  │  │  Nextcloud  │   │  DB  │   │  Redis   │  │
  │  │  App :7580  │◄──│ Psql │   │  Cache   │  │
  │  └──────┬──────┘   └──────┘   └──────────┘  │
  │         │                                    │
  │         │  Volume mapped                     │
  │         ▼                                    │
  │  /mnt/NAS_Storage  (3×4TB Array)             │
  └──────────────────────────────────────────────┘
```

**Key principle:** Cloudflare acts as a secure middleman. Your router never needs an open port. Your NAS is never directly exposed to the internet.

---

## ⚡ Setup Order

```
3.1 → Deploy Nextcloud stack via CasaOS + map storage volumes
3.2 → Create Cloudflare tunnel + configure host header
3.3 → Fix reverse proxy settings + trust Docker subnets
```

Complete all three in order — the Windows Desktop client fix in 3.3 depends on the Cloudflare tunnel being live from 3.2.

---

*Previous section: [Part 2 — Architecture & OS Setup](../02-architecture-and-os/README.md)*  
*Next section: [Part 4 — Client Setup](../04-client-setup/README.md)* *(Coming Soon)*
