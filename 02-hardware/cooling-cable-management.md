# Cooling and Cable Management

Proper airflow is critical for keeping drives, the Raspberry Pi, and the SATA Hat's bridge chip at safe temperatures over long periods of continuous operation.

---

## Cooling

### Fan Layout

Two additional 80mm or 120mm PC fans were added to the case (beyond the PSU fan and the Penta SATA Hat fan):

| Position | Role |
|----------|------|
| Front of case | Intake — pulls cool air past the drives |
| Rear of case | Exhaust — pushes hot air out |

Both fans were connected to **12V (yellow)** and **GND (black)** from the ATX PSU using Molex-to-fan adapters or terminal blocks.

### Airflow Path

```
[Front Fan — Intake]
        ↓
   [HDD Drive Bay]
        ↓
[Raspberry Pi + SATA Hat]
        ↓
[Rear Fan — Exhaust] → Out

[PSU] → own separate exhaust
```

The goal is a clean front-to-back path. Hot air from the drives flows directly toward the exhaust fan without recirculating inside the case.

---

## Cable Management

Good cable management matters for two reasons: it keeps airflow channels clear, and it makes future maintenance much easier.

### Steps

1. **Group cables by type** — power cables together, data cables together, GPIO signal wires together.
2. **Bundle with zip ties** at regular intervals (every 10–15 cm), routing along the case frame and away from fan blades.
3. **Shrink tube every exposed joint** — every solder connection and any wire splice.
4. **Leave service loops** — a small amount of slack near each connector so cables can be unplugged without pulling on the joint or the board.
5. **Check fan clearance** — walk a finger along every fan blade path to confirm no cables are in the way.

---

> 💡 **Tip:** Take a photo of the final interior before closing the case. It is much easier to trace a cable six months later if you have a reference photo.

---

[← Pi Power Supply](pi-power.md) | [Back to Hardware Overview →](README.md)
