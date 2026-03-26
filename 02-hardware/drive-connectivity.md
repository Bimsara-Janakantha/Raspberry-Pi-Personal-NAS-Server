# Drive Connectivity

Connecting the SAS drives via adapters and wiring all four drives to the Penta SATA Hat.

---

## Steps

### 1. Attach SAS-to-SATA Adapters

Press each SAS-to-SATA adapter firmly onto the back of a SAS drive until fully seated. These can be a tight fit — apply even pressure and confirm the adapter is not sitting at an angle.

> ⚠️ A misaligned adapter can damage pins and prevent the drive from being detected.

### 2. Connect SATA Data Cables

Run a SATA data cable from each drive (including the 1 TB SATA drive) to a port on the Penta SATA Hat.

The Hat supports up to 5 drives. Label or note which Hat port connects to which drive — this makes it easier to match drives to their device names (e.g. `sda`, `sdb`) later during software setup.

### 3. Connect SATA Power Cables

Run a SATA power cable from the ATX PSU to each drive.

The SAS drives with adapters accept standard SATA power connectors. If the PSU does not have enough SATA power connectors, use a SATA power splitter.

### 4. Check All Connections

Give each data cable and power cable a firm tug to confirm it is locked in place. A loose connection is one of the most common causes of drives not being detected.

---

## Summary Table

| Drive | Type | Data Cable To | Power Source |
|-------|------|---------------|--------------|
| SAS 1 (4 TB) | SAS → SATA adapter | Penta SATA Hat port 1 | ATX PSU (SATA power) |
| SAS 2 (4 TB) | SAS → SATA adapter | Penta SATA Hat port 2 | ATX PSU (SATA power) |
| SAS 3 (4 TB) | SAS → SATA adapter | Penta SATA Hat port 3 | ATX PSU (SATA power) |
| SATA (1 TB) | Standard SATA | Penta SATA Hat port 4 | ATX PSU (SATA power) |

---

[← Mechanical Assembly](mechanical-assembly.md) | [Next: ATX Power Integration →](atx-power.md)
