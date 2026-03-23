# NAS Health Monitor — Raspberry Pi GPIO Status LEDs

A lightweight Python service for Raspberry Pi that uses GPIO pins to give your NAS build real-time visual and audio feedback: disk activity via an LED, and an audible buzzer alert when temperatures rise above a set threshold.

---

## Table of Contents

- [Overview](#overview)
- [Hardware Requirements](#hardware-requirements)
- [Pin Configuration](#pin-configuration)
- [Installation](#installation)
  - [1. Install Dependencies](#1-install-dependencies)
  - [2. Configure the Activity LED Overlay](#2-configure-the-activity-led-overlay)
  - [3. Create the Monitor Script](#3-create-the-monitor-script)
- [Script Reference](#script-reference)
- [Running as a System Service](#running-as-a-system-service)
  - [1. Create the Service File](#1-create-the-service-file)
  - [2. Enable and Start the Service](#2-enable-and-start-the-service)
  - [3. Reload After Edits](#3-reload-after-edits)
- [Log File](#log-file)
- [Troubleshooting](#troubleshooting)

---

## Overview

This monitor runs continuously in the background and provides the following feedback:

| Indicator | Condition |
|-----------|-----------|
| **Disk LED ON** (GPIO 17) | Active disk read/write detected |
| **Buzzer beeping** (GPIO 16) | CPU temperature ≥ 50 °C |
| **Log entry** | Written every 5 seconds with uptime and temperature |

Disk activity is polled every **50 ms** for a responsive flicker effect. Temperature is checked every **5 seconds**.

---

## Hardware Requirements

- Raspberry Pi (any model running Ubuntu/Raspberry Pi OS)
- 1× LED (disk activity indicator) + appropriate resistor
- 1× Active buzzer (temperature warning)
- Jumper wires

---

## Pin Configuration

| Component | GPIO Pin | Physical Pin |
|-----------|----------|--------------|
| Disk Activity LED | GPIO 17 | Pin 11 |
| Temperature Buzzer | GPIO 16 | Pin 36 |
| Activity LED Overlay | GPIO 27 | Pin 13 |
| Ground | GND | Pin 6, 9, 14… |

> **Note:** GPIO 27 is used by the firmware-level `act-led` overlay (Step 2). GPIO 17 is controlled directly by the Python script for disk I/O indication.

---

## Installation

### 1. Install Dependencies

Update your package list and install the `gpiozero` library:

```bash
sudo apt update
sudo apt install python3-gpiozero -y
```

---

### 2. Configure the Activity LED Overlay

This step tells the Pi's firmware to map the internal activity LED signal to GPIO 27.

**2.1** Open the firmware configuration file:

```bash
sudo nano /boot/firmware/config.txt
```

**2.2** Scroll to the very bottom and add the following line:

```
dtoverlay=act-led,gpio=27
```

**2.3** Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`), then reboot for the change to take effect:

```bash
sudo reboot
```

---

### 3. Create the Monitor Script

**3.1** Create the directory and open a new script file:

```bash
sudo mkdir /usr/local/bin/heath
cd /usr/local/bin/heath
sudo nano health_monitor.py
```

**3.2** Paste the following code into the file:

```python
from gpiozero import LED, Buzzer
import time
import os
from datetime import datetime

# --- Pin Setup ---
disk_led = LED(17)     # Disk Activity LED
buzzer   = Buzzer(16)  # Temperature Warning Buzzer

# --- Configuration ---
TEMP_THRESHOLD = 50.0  # Degrees Celsius
SCRIPT_DIR     = os.path.dirname(os.path.abspath(__file__))
LOG_FILE       = os.path.join(SCRIPT_DIR, "nas_status.log")

def get_uptime():
    """Returns system uptime in seconds."""
    with open('/proc/uptime', 'r') as f:
        return float(f.readline().split()[0])

def get_temp():
    """Returns CPU temperature in °C."""
    with open('/sys/class/thermal/thermal_zone0/temp', 'r') as f:
        return float(f.read()) / 1000.0

def get_disk_activity():
    """Returns total sectors read and written across all monitored drives."""
    activity = 0
    with open('/proc/diskstats', 'r') as f:
        for line in f:
            parts = line.split()
            if len(parts) >= 10:
                dev = parts[2]
                # Monitors: SATA (sd*), RAID (md*), NVMe (nvme*), SD Card (mmcblk*)
                if dev.startswith(('sd', 'md', 'nvme', 'mmcblk')):
                    activity += int(parts[5])  # Sectors Read
                    activity += int(parts[9])  # Sectors Written
    return activity

# --- Initial State ---
last_disk_activity = get_disk_activity()
last_log_time      = time.time()
buzzer_is_beeping  = False

print("NAS Monitor Started — Disk LED: GPIO 17 | Buzzer: GPIO 16")

try:
    while True:
        current_time = time.time()

        # 1. DISK ACTIVITY — Poll every 50ms for responsive flicker
        current_disk_activity = get_disk_activity()
        if current_disk_activity > last_disk_activity:
            disk_led.on()
        else:
            disk_led.off()
        last_disk_activity = current_disk_activity

        # 2. TEMPERATURE CHECK — Runs every 5 seconds
        if current_time - last_log_time >= 5.0:
            temp       = get_temp()
            uptime     = get_uptime()
            current_dt = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

            # Append to log file
            with open(LOG_FILE, 'a') as log:
                log.write(f"[{current_dt}] Uptime: {uptime:.2f}s | Temp: {temp:.2f}°C\n")

            # Buzzer control
            if temp >= TEMP_THRESHOLD and not buzzer_is_beeping:
                buzzer.blink(on_time=0.5, off_time=0.5)
                buzzer_is_beeping = True
            elif temp < TEMP_THRESHOLD and buzzer_is_beeping:
                buzzer.off()
                buzzer_is_beeping = False

            last_log_time = current_time

        time.sleep(0.05)

except KeyboardInterrupt:
    pass
finally:
    disk_led.off()
    buzzer.off()
```

**3.3** Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

## Script Reference

| Variable | Default | Description |
|----------|---------|-------------|
| `TEMP_THRESHOLD` | `50.0` | Temperature (°C) above which the buzzer activates |
| `disk_led` | GPIO 17 | LED pin for disk activity indication |
| `buzzer` | GPIO 16 | Buzzer pin for thermal alerts |
| `LOG_FILE` | `nas_status.log` (same dir as script) | Path to the status log file |
| Poll interval | 50 ms | How often disk activity is checked |
| Log/temp interval | 5 s | How often temperature is checked and logged |

---

## Running as a System Service

Running the monitor as a `systemd` service ensures it starts automatically on every boot.

### 1. Create the Service File

```bash
sudo nano /etc/systemd/system/nasleds.service
```

Paste the following configuration:

```ini
[Unit]
Description=NAS Health Status Monitor
After=multi-user.target

[Service]
Type=simple
User=root
ExecStart=/usr/bin/python3 /usr/local/bin/heath/health_monitor.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

> **Custom username?** If you created a non-default user when flashing the OS, update `User=` and the `ExecStart` path to match your username.

---

### 2. Enable and Start the Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable nasleds.service
sudo systemctl start nasleds.service
```

To confirm it is running:

```bash
sudo systemctl status nasleds.service
```

---

### 3. Reload After Edits

Whenever you modify the service file, reload the daemon before restarting:

```bash
sudo systemctl daemon-reload
sudo systemctl restart nasleds.service
```

---

## Log File

The script writes a status entry every 5 seconds to `nas_status.log` in the same directory as the script (`/usr/local/bin/heath/nas_status.log`).

**Example output:**

```
[2025-07-10 14:32:01] Uptime: 3842.55s | Temp: 47.30°C
[2025-07-10 14:32:06] Uptime: 3847.60s | Temp: 48.10°C
[2025-07-10 14:32:11] Uptime: 3852.65s | Temp: 51.20°C
```

To tail the log in real time:

```bash
tail -f /usr/local/bin/heath/nas_status.log
```

---

## Troubleshooting

**Service fails to start**
- Run `sudo journalctl -u nasleds.service -e` to view error logs.
- Confirm the script path in `ExecStart` is correct.
- Ensure `python3-gpiozero` is installed.

**LED/Buzzer not responding**
- Double-check your wiring against the [Pin Configuration](#pin-configuration) table.
- Verify the GPIO numbers in the script match your physical connections.
- Test the pins manually with `python3 -c "from gpiozero import LED; LED(17).on()"`.

**Temperature threshold not triggering**
- The default threshold is `50.0 °C`. Adjust `TEMP_THRESHOLD` in the script if needed.
- Check the current temperature: `cat /sys/class/thermal/thermal_zone0/temp` (divide by 1000 for °C).

**Disk activity LED always off**
- Confirm your drives are recognised: `cat /proc/diskstats` should list `sd*`, `nvme*`, or `md*` devices.