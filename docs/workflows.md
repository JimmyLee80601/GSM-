# Workflows

Every workflow here is documented from real bench work — not theoretical.

---

## ADB diagnostics

The daily tool. Used for everything from checking device state to pulling data off a bricked phone.

```bash
# Check device state
adb devices
adb shell getprop ro.build.display.id
adb shell getprop ro.product.model

# Battery health
adb shell dumpsys battery

# Storage
adb shell df /data
adb pull /sdcard/DCIM ./recovery/

# Screen capture
adb exec-out screencap -p > screen.png

# Install APK
adb install -r app.apk

# Reboot modes
adb reboot bootloader    # fastboot
adb reboot recovery      # recovery
adb reboot sideload      # sideload mode
```

### Common device states
- **ADB online** — device booted, USB debugging authorized. Best case.
- **ADB unauthorized** — device is booted but USB debugging not approved. Need to tap "Allow" on screen.
- **Fastboot** — bootloader mode. `fastboot devices` to confirm.
- **EDL / 9008** — Qualcomm emergency download mode. Black screen, no ADB/fastboot. Use Firehose/QFIL.
- **Brom** — MediaTek preloader mode. Device won't boot past initial handshake. Use MTK Auth Bypass + SP Flash Tool.

---

## Fastboot

Used for bootloader unlock, partition flashing, and firmware updates.

```bash
# Check connection
fastboot devices

# Unlock bootloader (OEM unlock must be enabled in Developer Options)
fastboot oem unlock
# or for specific brands:
fastboot flashing unlock

# Flash partitions
fastboot flash boot boot.img
fastboot flash recovery recovery.img
fastboot flash system system.img

# Flash full factory image
fastboot update update.zip
fastboot flashall -w    # wipe + flash all

# Reboot
fastboot reboot
fastboot reboot recovery
```

### Notes
- OEM unlock varies by manufacturer — Samsung requires Samsung account + internet, OnePlus/Xiaomi more straightforward
- Some devices need `fastboot flashing unlock_critical` for critical partitions
- Always verify partition names with `fastboot getvar all` before flashing

---

## Qualcomm EDL / 9008

Emergency Download Mode — the "last resort" for Qualcomm devices that won't boot at all.

### When to use
- Device won't boot past black screen
- Brick after bad flash
- Need to re-flash entire firmware when nothing else works

### Entry methods
```bash
# Method 1: adb reboot edl (if ADB works)
adb reboot edl

# Method 2: fastboot
fastboot reboot edl

# Method 3: key combo (varies by device)
# Usually: Power + Vol Up + Vol Down while connected

# Method 4: test point (hardware)
# Short specific test points on the board to force EDL
# Model-specific — need board diagram
```

### Working with EDL
```bash
# Check if device is in EDL (Qualcomm HS-USB QDLoader 9008 in Device Manager)
# Windows: Device Manager → Ports (COM & LPT) → Qualcomm HS-USB QDLoader 9008 (COMx)

# QFIL (Qualcomm Flash Image Loader)
# 1. Select port (COMx)
# 2. Load programmer (firehose) — device-specific .mbn/.elf
# 3. Load rawprogram.xml + patch.xml
# 4. Click Download

# Or use edl (python tool):
edl printgpt --loader=firehose.mbn --port=COM5
edl qfil rawprogram.xml patch.xml --loader=firehose.mbn --port=COM5
```

### Firehose programmer
- Device-specific — must match exact model + chipset
- Sources: manufacturer firmware dumps, XDA, custom dev communities
- If wrong programmer is used → device may brick harder or refuse to enter EDL again

---

## MediaTek Brom mode

Pre-boot ROM mode for MediaTek devices. Used for firmware flashing when the device won't boot.

### When to use
- MediaTek device stuck in bootloop
- Need to flash firmware via SP Flash Tool
- FRP removal on MediaTek devices
- Device detected as "MediaTek USB Port" or "MTK USB Port" in Device Manager

### Entry methods
```bash
# Method 1: Power off + hold Vol Up + Vol Down while connecting USB
# Method 2: Power off + hold Vol Up while connecting USB
# Method 3: SP Flash Tool auto-detect

# Brom mode devices show in Device Manager as:
# - MediaTek USB Port
# - MediaTek PreLoader USB VCOM
# - MTK USB Port
```

### Working with Brom
```bash
# MTK Auth Bypass (bypasses Secure Boot authentication)
python mtk.py da seccfg unlock

# SP Flash Tool
# 1. Download Agent (DA) — device-specific .bin
# 2. Scatter file — maps firmware partitions
# 3. Select firmware files per partition
# 4. Click Download (with device connected in Brom/Preloader mode)

# Or useMTKClient:
python mtk.py rf firmware.bin    # read firmware
python mtk.py wf firmware.bin    # write firmware
python mtk.py reset              # reset FRP
```

### Common issues
- **Auth failed** — device has Secure Boot enabled, need auth bypass or authorized DA
- **Brom not detected** — try different USB port (USB 2.0 sometimes works better than 3.0)
- **Connection drops** — use shorter cable, avoid USB hubs

---

## FRP removal (Factory Reset Protection)

Google's anti-theft lock. Device asks for previously synced Google account after factory reset. Must be the owner — or have proof of ownership.

### ADB method (preferred — when ADB is available)
```bash
# Check FRP status
adb shell getprop ro.build.type
adb shell content query --uri content://settings/secure --where "name='user_setup_complete'"

# Remove FRP (Android 5-8)
adb shell content insert --uri content://settings/secure --bind name:s:user_setup_complete --bind value:s:1
adb shell am start -n com.google.android.gsf.login/
# or
adb shell am start com.google.android.gsf.login/

# Remove FRP (Android 9+)
adb shell pm uninstall com.google.android.gsf.login/
adb shell pm uninstall com.google.android.setupwizard/
# Factory reset from settings (if accessible)
```

### Qualcomm EDL method (when ADB unavailable)
```bash
# Use QFIL or edl tool
# Flash FRP partition directly
edl qfil frp_rawprogram.xml --loader=firehose.mbn --port=COM5

# Or erase FRP partition
edl erase frp --loader=firehose.mbn --port=COM5
```

### MediaTek method
```bash
# SP Flash Tool: format FRP partition
# Or MTKClient:
python mtk.py reset    # resets FRP + user data
```

### Notes
- FRP is tied to the device, not the SIM
- After FRP removal, device should boot to setup wizard without Google account prompt
- Always get proof of ownership before removing FRP — this is legally required in most jurisdictions

---

## Carrier unlock

Remove carrier SIM lock so device works on any network.

### ADB method
```bash
# Check carrier lock status
adb shell getprop gsm.sim.operator.alpha
adb shell dumpsys iphonesubinfo

# Request unlock code (carrier-dependent)
# Most carriers require: IMEI + account info + proof of purchase
# AT&T: https://www.att.com/deviceunlock/
# T-Mobile: https://www.t-mobile.com/support/account/unlock-your-mobile-device
# Verizon: usually unlocked by default on newer devices

# After getting unlock code:
adb shell am broadcast -a android.intent.action.CARRIER_SIM_UNLOCK --es code "UNLOCK_CODE"
```

### Diag port method
```bash
# Enter diag mode (Samsung)
*#0*# → field test → enable diag
# or
adb shell setprop persist.vendor.sys.modem.diag.mdlog true

# Use diag tool to write NV items / unlock
# Model-specific — Samsung uses different NV item numbers per carrier
```

### Notes
- Most US carriers unlock for free if device is paid off + not reported lost
- Some devices (Samsung) have carrier-specific firmware — unlock + flash unlocked firmware for full bands
- IMEI must not be blacklisted (stolen/lost reported)

---

## Data recovery

Pulling data from dead, locked, or damaged devices.

### ADB pull (device boots but locked/brick)
```bash
# If ADB works, pull everything
adb pull /sdcard/ ./recovery/
adb pull /data/media/0/ ./recovery/

# Photos specifically
adb pull /sdcard/DCIM/ ./recovery/photos/
adb pull /sdcard/Pictures/ ./recovery/pictures/

# WhatsApp / messaging
adb pull /sdcard/Android/media/com.whatsapp/WhatsApp/ ./recovery/whatsapp/
```

### Recovery mode pull
```bash
# Boot to recovery
adb reboot recovery
# or: Power + Vol Up

# Mount data partition (TWRP)
adb shell mount /data
adb pull /data/media/0/ ./recovery/
```

### JTAG / chip-off (hardware-level)
- Last resort for devices that won't boot at all
- Requires: JTAG adapter, soldering, chip-off tools
- Read NAND/eMMC directly from the board
- Model-specific pinouts — need board diagrams or experience
- Data extraction software to reconstruct filesystem from raw dump

---

## Diagnostics

Quick checks before quoting a repair.

```bash
# Full device info
adb shell getprop

# Battery health
adb shell dumpsys battery
adb shell cat /sys/class/power_supply/battery/health

# Storage health
adb shell cat /proc/diskstats
adb shell smartctl -a /dev/block/mmcblk0    # if available

# Sensor check
adb shell dumpsys sensorservice

# Camera test
adb shell am start -a android.media.action.IMAGE_CAPTURE

# Touch screen test
adb shell input tap 500 500    # tap center of screen

# Speaker/mic
adb shell am start -a android.intent.action.VOICE_COMMAND
```

### Diagnostic checklist
- [ ] Power on / boot state
- [ ] Screen (touch, display, brightness)
- [ ] Battery (health, charge, swelling)
- [ ] Cameras (front, rear, flash)
- [ ] Speakers + mic
- [ ] WiFi + cellular
- [ ] Bluetooth
- [ ] Charging port
- [ ] Sensors (accelerometer, proximity, fingerprint)
- [ ] Storage (available space, read/write)
