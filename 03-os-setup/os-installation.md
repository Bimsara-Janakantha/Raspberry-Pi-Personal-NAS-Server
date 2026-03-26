# OS Installation

The OS was installed headlessly — the Raspberry Pi was configured entirely before it was powered on for the first time, without ever needing a monitor or keyboard.

---

## Step 1 — Download Raspberry Pi Imager

On your main computer (Windows, Mac, or Linux), download and install the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/).

---

## Step 2 — Select the OS

1. Insert your MicroSD card into your main computer.
2. Open Raspberry Pi Imager.
3. Click **Choose Device** → select **Raspberry Pi 5**.
4. Click **Choose OS** → navigate to **Other general-purpose OS → Ubuntu** → select **Ubuntu Server 24.04 LTS (64-bit)**.
5. Click **Choose Storage** → select your MicroSD card.

---

## Step 3 — OS Customisation (Crucial)

When you click **Next**, the Imager will ask if you want to apply OS customisation settings. Click **Edit Settings**.

This is where you configure the Pi before it ever boots — saving you from needing a monitor later.

### General Tab

| Setting | Value |
|---------|-------|
| Hostname | `pinas` |
| Username | `admin` (or your preferred name) |
| Password | Choose a strong password — write it down |
| Configure Wireless LAN | Enter your Wi-Fi SSID and password |

> 💡 Even if you plan to use Ethernet (recommended for a NAS), configuring Wi-Fi here is a useful backup for the first boot.

### Services Tab

| Setting | Value |
|---------|-------|
| Enable SSH | ✅ Checked |
| Authentication | Use password authentication |

---

## Step 4 — Flash and Boot

1. Click **Save**, then click **Yes** to apply settings and write the SD card.
2. Wait for the Imager to write and verify the OS (this takes a few minutes).
3. Remove the SD card and insert it into the **powered-down** Raspberry Pi 5.
4. Connect all four drives, make sure the ATX PSU is switched on, then power up the Pi.

---

## Step 5 — Find the Pi on Your Network

Give the Pi about **3 to 5 minutes** on first boot — it needs to resize its partitions and connect to the network.

To find its IP address:

- **Option A:** Log into your home router's admin page and look at the connected devices list. Look for a device named `pinas`.
- **Option B:** Use a network scanning tool such as [Advanced IP Scanner](https://www.advanced-ip-scanner.com/) (Windows) or [Fing](https://www.fing.com/) (mobile).

---

## Step 6 — Connect via SSH

Once you have the IP address, open a terminal and connect:

```bash
ssh admin@192.168.1.100
```

Replace `192.168.1.100` with your Pi's actual IP address. Enter the password you set during the customisation step.

You should now be at the Ubuntu Server command prompt on your Pi.

---

[← OS Selection](os-selection.md) | [Next: Health Monitoring →](health-monitoring.md)
