# Enterprise-Grade Raspberry Pi NAS

A highly optimized, production-ready Network Attached Storage (NAS) system built on a Raspberry Pi 5. This project provides a "OneDrive-like" experience for users while bypassing strict corporate firewalls using Zero Trust tunneling.

## Core Features

- **Split-Brain Storage Architecture:** The OS and Nextcloud databases run on a 1TB HDD to prevent burnout, while mass file storage is offloaded to an 8TB storage array.
- **Hardware-Level Quotas:** Strict Linux filesystem quotas prevent users from over-consuming storage, acting as a fail-safe beneath the software layer.
- **Corporate Firewall Bypass:** Utilizing Cloudflare Zero Trust Tunnels to disguise NAS traffic as standard, encrypted web traffic (`HTTPS/443`).
- **Virtual File Sync:** Desktop clients utilize "Files On-Demand" (Virtual Files) to browse terabytes of data without consuming local `C:\` drive space.

## Technology Stack

- **Hardware:** Raspberry Pi 5, 64GB SD Card (OS), 1TB HDD (DB), 3 x 4TB Storage (Data)
- **OS & Management:** Ubuntu Server, CasaOS
- **Application:** Nextcloud ("Big Bear" optimized Docker stack with PostgreSQL & Redis)
- **Networking:** Cloudflare Tunnels (Zero Trust)
- **Monitoring:** Custom Python script for real-time disk activity and temperature feedback via GPIO
