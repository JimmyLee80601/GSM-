# Devices

Known devices, quirks, and notes from bench work.

---

## Samsung Galaxy S23 Ultra

- **Serial:** R3CW10HMPCZ
- **Connection:** USB 3.0 to workstation, ADB authorized
- **Tailscale:** 100.126.38.38
- **Notes:**
  - Needs "Allow USB debugging?" tap when connecting to new PC
  - Screen capture via `adb exec-out screencap -p` works reliably
  - Qualcomm Snapdragon 8 Gen 2 — EDL mode available if needed
  - Samsung-specific: test mode `*#0*#` for hardware diagnostics
  - Carrier unlock via Samsung + carrier portal

### ADB commands (Samsung-specific)
```bash
# Samsung test mode
adb shell am start -a android.intent.action.MAIN -n com.sec.android.app.sctest/.SCTestActivity

# IMEI
adb shell service call iphonesubinfo 1

# Samsung firmware
# Use Frija or SamFirm to download, Odin to flash
```

---

## Device categories

### Qualcomm (EDL / 9008)
- Samsung Galaxy S series (most generations)
- OnePlus series
- Xiaomi (most models)
- Google Pixel (some models)
- Motorola

Entry: ADB → `adb reboot edl`, key combo, or test point
Tool: QFIL, edl (python), Firehose programmer (model-specific)

### MediaTek (Brom / Preloader)
- Most budget/mid-range phones (Oppo, Vivo, Realme, Infinix, Tecno)
- Some Samsung (A series, M series)
- Xiaomi (some models)

Entry: Power off + hold Vol Up+Down while connecting USB
Tool: SP Flash Tool, MTKClient, MTK Auth Bypass

### Exynos (Samsung)
- Samsung Galaxy (international models)
- Entry: Fastboot-like mode, Odin flash
Tool: Odin, Heimdall

---

## Adding a new device

When working on a new model, document:
1. **Chipset** — Qualcomm / MediaTek / Exynos / other
2. **How to enter EDL/Brom/fastboot** — key combos, test points
3. **Firehose/DA file** — where to get it, version
4. **Partition layout** — `fastboot getvar all` or `edl printgpt`
5. **Known quirks** — things that don't follow the standard workflow
6. **Unlock status** — bootloader locked/unlocked, FRP status
