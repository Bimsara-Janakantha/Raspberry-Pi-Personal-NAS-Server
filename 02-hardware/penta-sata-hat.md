# Penta SATA Hat Setup

The Geekworm Penta SATA Hat connects to the Raspberry Pi 5 via the PCIe FPC interface. This is the primary data bus that all four drives communicate through.

<img src="../asserts/img8.webp" alt="Geekworm Penta SATA Hat" width="400">

---

## Steps

### 1. Attach the Cooling Fan

Attach the cooling fan included with the Penta SATA Hat to the Hat's fan connector. Orient the fan so it draws air across the Hat's bridge chip — the large integrated circuit (IC) in the centre of the board.

### 2. Connect the PCIe FPC Ribbon Cable — Pi End

1. Locate the **PCIe FPC connector on the Raspberry Pi 5**, labelled **J2** on the board.
2. Gently open the FPC latch by pushing it upward.
3. Insert the ribbon cable with the **metal contacts facing down** (toward the Pi board).
4. Push the latch down firmly until it clicks and holds the cable in place.

### 3. Connect the FPC Cable — Hat End

Repeat the same process on the Penta SATA Hat's PCIe FPC connector.

### 4. Position the Hat

Mount the Penta SATA Hat on standoffs **beside or above** the Pi — **not stacked directly on top**. The Hat connects via the PCIe ribbon cable, not the 40-pin GPIO header (which remains free for the LED and buzzer wiring).

<img src="../asserts/img9.webp" alt="Penta SATA Hat with fan attached and PCIe cable connected" width="400">

### 5. Check the Cable

Inspect the ribbon cable after routing. Make sure:

- There are **no sharp bends or kinks** in the cable
- The **bend radius is gentle** — a tight fold can damage the cable conductors
- The cable is not under tension at either end

<img src="../asserts/img10.webp" alt="Close-up of PCIe FPC cable connection with proper orientation" width="400">

---

## Why PCIe?

The Raspberry Pi 5 is the first Pi model to expose a PCIe interface. The Penta SATA Hat uses this to connect up to 5 SATA drives through an ASM1166 bridge chip, giving significantly higher bandwidth than USB-attached storage solutions.

---

[← GPIO Wiring](gpio-wiring.md) | [Next: Pi Power Supply →](pi-power.md)
