# GPIO Wiring — LEDs and Buzzer

The PC case's built-in LEDs and an active buzzer were wired to the Raspberry Pi GPIO pins to provide real-time visual and audio feedback directly from the hardware.

---

> ⚠️ **Important:** Raspberry Pi GPIO pins operate at **3.3V** with a maximum current of approximately **16mA per pin**. Always use a current-limiting resistor (330Ω) in series with each LED to prevent damage to the GPIO pins.

---

## Pin Reference Table

| Function | GPIO (BCM) | Physical Pin | Notes |
|----------|-----------|--------------|-------|
| Power LED (Green) | GPIO 17 | Pin 11 | 330Ω resistor in series |
| Disk Activity LED (Red) | GPIO 27 | Pin 13 | 330Ω resistor in series |
| Temperature Buzzer | GPIO 16 | Pin 36 | Via NPN transistor — buzzer needs 5V |
| GND (shared) | — | Pin 6, 9, 14… | Multiple GND pins available |
| 5V (buzzer circuit) | — | Pin 2 or 4 | Do not draw more than 500mA total |

---

## A — Power LED (Green) → GPIO 17

This LED indicates that the system is powered on and running.

**Wiring:**

1. Locate the green LED connector from the PC case front panel bundle.
2. Connect the **anode (+)** through a **330Ω resistor** to **GPIO 17 (Physical Pin 11)**.
3. Connect the **cathode (−)** to any **GND pin** on the Pi (e.g. Physical Pin 9).
4. Apply shrink tubing to all exposed joints.

---

## B — Disk Activity LED (Red) → GPIO 27

This LED flashes whenever there is an active disk read or write operation.

**Wiring:**

1. Locate the red LED connector from the PC case front panel bundle.
2. Connect the **anode (+)** through a **330Ω resistor** to **GPIO 27 (Physical Pin 13)**.
3. Connect the **cathode (−)** to any **GND pin** on the Pi.
4. Apply shrink tubing to all exposed joints.

---

## C — Temperature Warning Buzzer → GPIO 16

This buzzer emits an audible beep when the CPU temperature exceeds the configured threshold (default: 50°C).

**Why a transistor is needed:** An active buzzer requires 5V, but GPIO pins only output 3.3V. A small NPN transistor is used as a switch — the GPIO pin drives the base, and the buzzer is powered from the Pi's 5V rail.

**Wiring diagram:**

```
GPIO 16 ──[1kΩ]──► Base   (NPN transistor, e.g. 2N2222 or BC547)
5V      ─────────► Buzzer (+) ──► Collector
GND     ─────────► Emitter
                   Buzzer (−) ──► Collector
```

**Steps:**

1. Connect a **1kΩ resistor** from GPIO 16 to the transistor **Base**.
2. Connect **5V (Pin 2 or 4)** to the **positive terminal of the buzzer**, then to the transistor **Collector**.
3. Connect the transistor **Emitter** to **GND**.
4. Insulate all joints.

---

[← ATX Power Integration](atx-power.md) | [Next: Penta SATA Hat Setup →](penta-sata-hat.md)
