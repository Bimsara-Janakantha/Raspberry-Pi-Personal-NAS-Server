# 🖥️ RasPi NAS — Personal Network Attached Storage (DIY Build) - Hardware Configuration

> A fully functional, low-power NAS built on a Raspberry Pi 5, featuring a Geekworm Penta SATA Hat, industrial SAS drives, and housed in a repurposed desktop tower — complete with GPIO-controlled status LEDs, buzzer alerts, and ATX power integration.

---

## 🗺️ Project Overview

This project transforms a Raspberry Pi 5 into a capable home NAS server using the Geekworm Penta SATA Hat for multi-drive support. Three industrial 4TB SAS drives (via SAS-to-SATA adapters) and one 1TB SATA HDD are housed inside a repurposed desktop PC tower powered by an ATX PSU. The system integrates directly with the Pi's GPIO for visual status indicators, thermal alerts, and fan control.

**Total raw storage capacity:** ~13 TB (3 × 4TB SAS + 1 × 1TB SATA)
**Total usable capacity (RAID 5):** ~9 TB (RAID 5 Array + 1 × 1TB SATA)
**Key Features:** - Multi-drive support via PCIe-connected Penta SATA Hat - ATX power supply integration with a custom latching power button - GPIO-controlled power and activity LEDs - GPIO-driven buzzer for temperature warnings - Additional cooling fans for optimal airflow - Neat cable management with shrink tubing and zip ties

---

## 🔧 Part 1 — Hardware Configuration

### Bill of Materials

| #   | Component                     | Quantity  | Notes                                 |
| --- | ----------------------------- | --------- | ------------------------------------- |
| 1   | Raspberry Pi 5                | 1         | Main compute board                    |
| 2   | MicroSD Card (64GB)           | 1         | OS boot drive                         |
| 3   | Geekworm Penta SATA Hat       | 1         | Supports up to 5 SATA drives via PCIe |
| 4   | PCIe FPC Ribbon Cable         | 1         | Connects Pi 5 to Penta SATA Hat       |
| 5   | ATX Desktop Power Supply      | 1         | Powers drives and the Pi              |
| 6   | Old Desktop PC Case           | 1         | Enclosure with existing fans & LEDs   |
| 7   | Industrial SAS HDDs (4TB)     | 3         | Enterprise-grade drives               |
| 8   | SATA HDD (1TB)                | 1         | Standard desktop drive                |
| 9   | SAS-to-SATA Adapters          | 3         | One per SAS drive                     |
| 10  | SATA Data Cables              | 4         | One per drive                         |
| 11  | SATA Power Cables / Splitters | As needed | From ATX PSU                          |
| 12  | Latching Push Button          | 1         | Replaces momentary power switch       |
| 13  | Male DC Barrel Jack           | 1         | For Pi power input via SATA Hat       |
| 14  | Terminal Block Connectors     | Several   | For safe wire terminations            |
| 15  | Active 5V Buzzer              | 1         | Temperature warning alert             |
| 16  | Green LED (Power Indicator)   | 1         | Already in PC case                    |
| 17  | Red LED (Disk Activity)       | 1         | Already in PC case                    |
| 18  | PC Case Cooling Fans (12V)    | 2         | Additional ventilation                |
| 19  | Shrink Tubing                 | Assorted  | For connection insulation             |
| 20  | Zip Ties                      | Pack      | Cable management                      |
| 21  | Soldering Iron + Solder       | 1 set     | For wiring connections                |
| 22  | Multimeter                    | 1         | For continuity and voltage checks     |

<img src="../../asserts/img1.png" alt="Bill of Materials Table" width="100%" height="auto">

---

## ⚠️ Safety Warnings

- **Mains voltage is lethal.** The ATX PSU contains capacitors that retain charge even after being unplugged. Never open the PSU enclosure.
- **Always switch off and unplug the PSU** before changing any wiring inside the case.
- **Verify polarity** before powering on — reversed 12V will instantly destroy the Penta SATA Hat and Raspberry Pi.
- **Do not exceed GPIO current limits** — each GPIO pin is rated at ~16mA maximum. Always use current-limiting resistors with LEDs.
- **The buzzer requires 5V** — never connect it directly to a GPIO pin without a transistor switching circuit.
- **Ensure adequate ventilation** — hard drives produce heat, and a closed case without airflow will shorten drive lifespan significantly.
- This build involves mains-connected equipment. If you are not experienced with electrical work, consult someone who is.

---

## 📄 License

This project documentation is released under the [MIT License](LICENSE).

---

_Built with ☕ and too many zip ties._
