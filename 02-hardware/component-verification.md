# Component Verification

Before assembling anything, every component was tested individually. This saves a lot of headache — diagnosing failures inside a fully built system is far harder than catching them before you start.

---

## 1. Raspberry Pi 5

**What to check:** The Pi boots correctly.

1. Insert the MicroSD card with Raspberry Pi OS (or leave it blank to check the bootloader screen).
2. Connect to a monitor via micro-HDMI and attach a USB keyboard.
3. Power on and confirm it reaches the desktop or bootloader prompt.

---

## 2. MicroSD Card

**What to check:** The write completes without errors.

1. On a Windows or Mac PC, download and open [Raspberry Pi Imager](https://www.raspberrypi.com/software/).
2. Select your device, choose Raspberry Pi OS, and select the SD card as the storage target.
3. Click Write and wait for the verification step to pass.

---

## 3. Penta SATA Hat

**What to check:** No physical damage.

Visually inspect the board for bent pins, cracked solder joints, or damaged components. Do not power it yet — full functionality is verified after the complete build.

---

## 4. ATX Power Supply — Paperclip Test

**What to check:** The PSU powers on independently.

The ATX PSU will not start on its own without a motherboard. The paperclip test simulates the motherboard's power-on signal.

1. Locate the **24-pin ATX connector**.
2. Identify **Pin 16 (PS_ON, green wire)** and any **GND pin (black wire)**.
3. Short them together with a paperclip or jumper wire.
4. Plug the PSU into mains power and switch it on.
5. Confirm the PSU fan spins.
6. Use a multimeter to measure voltage between a **yellow wire (+12V)** and a **black wire (GND)**. It should read approximately **12V DC**.
7. **Power off and remove the jumper before continuing.**

> ⚠️ **Always remove the jumper before touching any other wiring.** The PSU is live when the jumper is in place.

---

## 5. SAS Drives

**What to check:** Each drive spins up and is detected.

If you have access to a PC with an HBA (Host Bus Adapter) card or an external SAS enclosure, connect each drive individually and confirm it appears in the OS. On Linux, use `lsblk`. On Windows, use Disk Management.

---

## 6. SATA HDD (1 TB)

**What to check:** The drive is detected.

Connect the drive to a SATA port on a PC and confirm it shows up:

- **Windows:** Open Disk Management (Win + X → Disk Management)
- **Linux:** Run `lsblk`

---

## 7. SAS-to-SATA Adapters

**What to check:** No bent pins, adapter seats firmly.

Visually inspect each adapter for bent or damaged pins. Attach each adapter to a SAS drive and confirm it clicks into place without sitting at an angle.

---

## 8. PC Case

**What to check:** Power button, LEDs, and fans are functional.

Open the case and check:

- The power button is physically intact and springs back correctly.
- Both LEDs (green and red) are not cracked.
- The existing case fans spin freely when given 12V from a power source.

---

[← Bill of Materials](bom.md) | [Next: Drive Preparation →](drive-preparation.md)
