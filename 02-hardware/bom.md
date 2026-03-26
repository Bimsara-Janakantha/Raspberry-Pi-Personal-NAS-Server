# Bill of Materials

A full list of every component used in this build.

---

## Component List

| Component | Qty | Notes |
|-----------|-----|-------|
| Raspberry Pi 5 (8 GB) | 1 | Main compute board |
| MicroSD Card (64 GB) | 1 | OS boot drive |
| Geekworm Penta SATA Hat | 1 | Supports up to 5 SATA drives via PCIe |
| PCIe FPC Ribbon Cable | 1 | Connects Pi 5 to Penta SATA Hat |
| ATX Desktop Power Supply (650W) | 1 | Powers drives and the Pi |
| Old Desktop PC Case | 1 | Enclosure with existing fans and LEDs |
| Industrial SAS HDDs (4 TB each) | 3 | Enterprise-grade drives |
| SATA HDD (1 TB) | 1 | Standard desktop drive for web server storage |
| SAS-to-SATA Adapters | 3 | One per SAS drive |
| SATA Data Cables | 4 | One per drive |
| SATA Power Cables / Splitters | As needed | From ATX PSU |
| Latching Push Button | 1 | Replaces the momentary power switch |
| Male DC Barrel Jack (5.5mm/2.1mm) | 1 | For Pi power input via SATA Hat |
| Terminal Block Connectors | Several | For safe wire terminations |
| Active 5V Buzzer | 1 | Temperature warning alert |
| Green LED (Power Indicator) | 1 | Already built into the PC case |
| Red LED (Disk Activity) | 1 | Already built into the PC case |
| PC Case Cooling Fans (12V) | 2 | Additional ventilation |
| Shrink Tubing | Assorted | For insulating solder joints |
| Zip Ties | 1 pack | Cable management |
| Soldering Iron and Solder | 1 set | For all wiring connections |
| Multimeter | 1 | For continuity and voltage checks |

---

## Where to Source Components

- **Raspberry Pi 5** — Official Raspberry Pi resellers (e.g. PiHut, Adafruit, RS Components)
- **Geekworm Penta SATA Hat** — [Geekworm official store](https://geekworm.com) or Amazon
- **SAS HDDs** — eBay (used enterprise drives are a great value option)
- **ATX PSU** — Any PC parts store or reuse one from an old desktop
- **PC Case** — Reuse an old mid-tower or full-tower desktop case
- **Everything else** — Any electronics supplier (Jaycar, Mouser, DigiKey, or Amazon)

---

> 💡 **Tip:** The SAS drives in this build were sourced second-hand from a decommissioned server. Enterprise SAS drives are extremely reliable and usually have very low total hours when pulled from data centre rotations.

---

[← Hardware Overview](README.md) | [Next: Component Verification →](component-verification.md)
