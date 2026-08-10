# GSM- 🛠️

**Autonomous GSM repair** — documenting the bench, the workflows, and the tools for device repair, diagnostics, firmware flashing, and unlocking.

This is the private brain: what I know, how I work, what I use. Built from real bench time — not theory.

## What this is

GSM- is the knowledge base for running a device repair bench with automation. Every workflow documented here is something that's been done on real devices — screen replacements, board-level micro-soldering, firmware flashing via EDL/Brom/Preloader, FRP removal, carrier unlocking, data recovery, and diagnostics.

The "autonomous" part: using scripting and AI (HarleyStation) to assist, automate, and document the repair workflow end-to-end.

## Structure

```
GSM-/
├── README.md              ← you are here
├── docs/
│   ├── bench.md           ← workstation, software, physical setup
│   ├── workflows.md       ← adb, fastboot, EDL, brom, preloader, firehose, FRP, unlock
│   ├── devices.md         ← device notes, known models, quirks
│   └── hardware.md        ← cables, adapters, tools, boxes
```

## The bench

- **Machine:** jimmysgsmworkstation — Windows 10 Pro 22H2
- **Software:** ADB/Fastboot, Python 3.12, VS 2022, VS Code, Notepad++
- **AI assist:** Ollama (local), LM Studio (local), NVIDIA Cloud, HarleyStation
- **Phone:** Samsung S23 Ultra (USB 3.0, authorized ADB)
- See [docs/bench.md](docs/bench.md) for full setup

## Core workflows

| Workflow | Tool | Status |
|----------|------|--------|
| ADB diagnostics | adb shell / pull / push | ✅ daily use |
| Fastboot flash / unlock | fastboot flash / oem | ✅ |
| EDL / 9008 mode | Qualcomm Firehose / QFIL | ✅ |
| MediaTek Brom mode | MTK Auth Bypass / SP Flash Tool | ✅ |
| FRP removal | ADB + scripting | ✅ |
| Carrier unlock | ADB / diag port | ✅ |
| Data recovery | ADB pull / JTAG / chip-off | ✅ basic |
| Micro-soldering | Board-level repair | ✅ |

See [docs/workflows.md](docs/workflows.md) for details.

## Contact

Jimmy Lee · Brighton, CO · georgiaboy77535@gmail.com
