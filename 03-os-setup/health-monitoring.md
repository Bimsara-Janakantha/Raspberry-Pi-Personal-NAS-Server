# Health Monitoring

A Python script runs in the background as a system service to monitor disk activity and CPU temperature, providing real-time feedback through the hardware LEDs and buzzer.

---

## What It Does

| Indicator | Condition |
|-----------|-----------|
| Disk Activity LED ON (GPIO 17) | Active disk read/write detected |
| Buzzer beeping (GPIO 16) | CPU temperature ≥ 50°C |
| Log entry written | Every 5 seconds — uptime and temperature |

Disk activity is polled every **50ms** for a responsive LED flicker. Temperature is checked every **5 seconds**.

---

## Step 1 — Install Dependencies

```bash
sudo apt update
sudo apt install python3-gpiozero -y
```

---

## Step 2 — Configure the Activity LED Overlay

This tells the Pi's firmware to map the internal activity LED signal to GPIO 27.

```bash
sudo nano /boot/firmware/config.txt
```

Scroll to the very bottom and add:

```
dtoverlay=act-led,gpio=27
```

Save (`Ctrl+O`, `Enter`) and exit (`Ctrl+X`), then reboot:

```bash
sudo reboot
```

---

## Step 3 — Create the Monitor Script

```bash
sudo mkdir /usr/local/bin/heath
cd /usr/local/bin/heath
sudo nano health_monitor.py
```

Paste the following script:

```python
from gpiozero import LED, Buzzer
import time
import os
from datetime import datetime

# --- Pin Setup ---
disk_led = LED(17)      # Disk Activity LED
buzzer   = Buzzer(16)   # Temperature Warning Buzzer

# --- Configuration ---
TEMP_THRESHOLD = 50.0
SCRIPT_DIR = os.path.dirname(os.path.abspath(__file__))
LOG_FILE   = os.path.join(SCRIPT_DIR, "nas_status.log")

def get_uptime():
    with open('/proc/uptime', 'r') as f:
        return float(f.readline().split()[0])

def get_temp():
    with open('/sys/class/thermal/thermal_zone0/temp', 'r') as f:
        return float(f.read()) / 1000.0

def get_disk_activity():
    activity = 0
    with open('/proc/diskstats', 'r') as f:
        for line in f:
            parts = line.split()
            if len(parts) >= 10:
                dev = parts[2]
                if dev.startswith(('sd', 'md', 'nvme', 'mmcblk')):
                    activity += int(parts[5])   # Sectors read
                    activity += int(parts[9])   # Sectors written
    return activity

last_disk_activity = get_disk_activity()
last_log_time      = time.time()
buzzer_is_beeping  = False

print("NAS Monitor Started — Disk LED: GPIO 17 | Buzzer: GPIO 16")

try:
    while True:
        current_time = time.time()

        # Disk activity — poll every 50ms
        current_disk_activity = get_disk_activity()
        if current_disk_activity > last_disk_activity:
            disk_led.on()
        else:
            disk_led.off()
        last_disk_activity = current_disk_activity

        # Temperature check — every 5 seconds
        if current_time - last_log_time >= 5.0:
            temp   = get_temp()
            uptime = get_uptime()
            now    = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

            with open(LOG_FILE, 'a') as log:
                log.write(f"[{now}] Uptime: {uptime:.2f}s | Temp: {temp:.2f}°C\n")

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

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

## Step 4 — Run as a System Service

Create the service file:

```bash
sudo nano /etc/systemd/system/nashealth.service
```

Paste:

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

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable nashealth.service
sudo systemctl start nashealth.service
```

Verify it is running:

```bash
sudo systemctl status nashealth.service
```

---

## Viewing the Log

```bash
tail -f /usr/local/bin/heath/nas_status.log
```

Example output:

```
[2025-07-10 14:32:01] Uptime: 3842.55s | Temp: 47.30°C
[2025-07-10 14:32:06] Uptime: 3847.60s | Temp: 48.10°C
[2025-07-10 14:32:11] Uptime: 3852.65s | Temp: 51.20°C
```

---

## Troubleshooting

**Service fails to start**
- Run `sudo journalctl -u nashealth.service -e` to view error logs.
- Confirm the script path in `ExecStart` is correct.
- Ensure `python3-gpiozero` is installed.

**LED/Buzzer not responding**
- Double-check wiring against the [GPIO Wiring guide](../02-hardware/gpio-wiring.md).
- Test a pin manually: `python3 -c "from gpiozero import LED; LED(17).on()"`

**Temperature threshold not triggering**
- Check current temperature: `cat /sys/class/thermal/thermal_zone0/temp` (divide by 1000 for °C).
- Adjust `TEMP_THRESHOLD` in the script if 50°C is too low or too high for your environment.

**Disk activity LED always off**
- Confirm drives are recognised: `cat /proc/diskstats` should list `sd*`, `nvme*`, or `md*` devices.

---

[← OS Installation](os-installation.md) | [Back to OS Overview →](README.md)
