# Raspberry Pi Power Supply (12V via ATX PSU)

The Penta SATA Hat includes a DC barrel jack input that accepts 12V and provides regulated power to both the Hat and the Raspberry Pi 5. This means a separate USB-C power supply for the Pi is not needed — everything runs from the single ATX PSU.

---

## Steps

### 1. Identify the 12V Rail

On the ATX PSU, locate a spare **Molex** or **SATA power connector**:

- **Yellow wires = +12V**
- **Black wires = GND**

Verify with a multimeter: yellow (+) to black (−) should read approximately **12V DC**.

> ⚠️ Do **not** cut into the main 24-pin ATX bundle. Use a spare drive power connector instead.

### 2. Prepare the DC Barrel Jack

The Penta SATA Hat uses a standard **5.5mm/2.1mm DC barrel jack** with **centre-positive** polarity.

1. Strip the ends of a yellow (+12V) wire and a black (GND) wire from the spare connector.
2. Solder the **yellow wire to the centre pin** (positive).
3. Solder the **black wire to the outer barrel** (negative).
4. Apply shrink tubing to each solder joint individually, then add an outer layer over the full connector.

### 3. Verify Polarity

Before plugging anything in, use a multimeter to confirm:

- Centre pin reads **+12V**
- Outer barrel reads **0V (GND)**

> ⚠️ **Reversed polarity will damage both the Penta SATA Hat and the Raspberry Pi.** Always confirm with a multimeter before powering on.

### 4. Connect to the Hat

Plug the DC barrel jack into the power input on the Penta SATA Hat. The silkscreen on the board shows a centre-positive diagram — double-check that this matches.

---

## Why 12V Instead of USB-C?

| Method | Pros | Cons |
|--------|------|------|
| USB-C (5V/5A) | Simple | Requires a separate power supply and cable |
| 12V via SATA Hat | One PSU powers everything | Requires soldering a DC barrel jack |

Using the 12V route keeps the build clean — one PSU, one power switch, everything tied together.

---

[← Penta SATA Hat Setup](penta-sata-hat.md) | [Next: Cooling and Cable Management →](cooling-cable-management.md)
