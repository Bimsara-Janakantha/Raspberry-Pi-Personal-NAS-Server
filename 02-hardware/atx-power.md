# ATX Power Integration

The ATX PSU needs a permanent on/off mechanism since it is no longer controlled by a motherboard. The PC case's existing power button was re-wired to control the PSU directly.

---

## Part A — Replace the Momentary Switch with a Latching Push Button

A standard PC power button is **momentary** — it sends a brief pulse when pressed and released. An ATX PSU needs the PS_ON pin held LOW (connected to GND) continuously to stay on. A **latching button** does exactly this: press once to latch on, press again to release and shut off.

### Steps

1. Locate the power switch connector in the front panel cable bundle of the PC case.
2. Desolder (or cut and strip) the existing momentary push button wires from the power switch header.
3. Solder the two wires to the terminals of the new latching push button.
4. Apply shrink tubing over each solder joint and use a heat gun to seal them.
5. Connect the wires to a small terminal block for a cleaner connection point.

---

## Part B — Wire PS_ON to the Latching Button

### How ATX Power-On Works

An ATX PSU stays in standby mode until its **PS_ON pin is pulled to GND** (below ~0.8V). Shorting PS_ON to GND via the latching button mimics exactly what a motherboard does when you press the power button.

### Identifying the Correct Pins

On the 24-pin ATX connector:

| Pin | Wire Colour | Signal |
|-----|-------------|--------|
| Pin 16 | Green | PS_ON |
| Pin 15 or 17 | Black | GND |

> 💡 Use a multimeter in continuity mode to double-check the wire colours on your specific PSU, as some brands vary.

### Steps

1. Identify the **PS_ON wire (green, Pin 16)** and a **GND wire (black)** near the connector end (not near the PSU body).
2. Tap or splice into both wires.
3. Connect the **PS_ON wire** and the **GND wire** to the two terminals of the latching push button, using terminal blocks for clean connections.

### How It Works in Practice

| Button State | PS_ON to GND | PSU State |
|--------------|-------------|-----------|
| Latched (ON) | Shorted | PSU powers on |
| Released (OFF) | Open | PSU shuts down |

---

> ⚠️ Always work near the connector end of the cable, not near the PSU body. Keep the PSU unplugged from mains while making any wiring changes.

---

[← Drive Connectivity](drive-connectivity.md) | [Next: GPIO Wiring →](gpio-wiring.md)
