# Hardware

Physical tools, cables, and accessories on the bench.

---

## Cables & connections

- **USB-C to USB-C** — primary connection for modern devices (S23 Ultra, most 2020+ phones)
- **USB-A to USB-C** — workstation to device, data + charge
- **USB-C hub** — multi-port for simultaneous charging + data
- **Short cables (1ft)** — preferred for ADB/fastboot work (less signal degradation, less clutter)

### Cable notes
- Data cables ≠ charge-only cables — always verify data lines are present
- USB 2.0 cables work fine for ADB/fastboot; USB 3.0 for faster transfers
- Some devices are picky about cables — if connection drops, try a different cable

---

## Diagnostic tools

- **Multimeter** — voltage, continuity, resistance checks on boards
- **USB Doctor** — USB power meter (checks charge current, identifies bad ports/cables)
- **Magnifying glass / loupe** — for inspecting board damage, connector corrosion

---

## Repair tools

### Hand tools
- Precision screwdriver set (Phillips #000, Pentalobe, Tri-point, Torx T2-T6)
- Spudger set (nylon + metal) — prying open cases without damage
- Tweezers (straight + curved) — handling small components
- Plastic opening picks — separating adhesive-bonded screens
- Heat gun / hair dryer — softening adhesive for screen removal
- Suction cup — lifting screens

### Soldering
- Soldering iron (adjustable temp, fine tip)
- Solder wire (lead-free, rosin core)
- Flux (liquid + paste)
- Solder wick / desoldering braid
- Hot air rework station — for IC removal and reflow
- Micro-soldering tips — for fine-pitch components
- Kapton tape — heat-resistant masking
- Isopropyl alcohol (99%) — cleaning flux residue

### Micro-soldering specific
- USB microscope / digital microscope — inspecting board-level work
- JTAG adapter — for chip-level data recovery
- Chip-off tools — removing NAND/eMMC chips directly
- Stencil kit — BGA reballing

---

## Software dongles / boxes

- **Z3X Samsung Tool Pro** — Samsung firmware flash, unlock, repair
- **Octoplus** — multi-brand service tool
- **UMT (Ultimate Multi Tool)** — flashing, unlocking, IMEI repair
- **Chimera** — Samsung + LG + more, cloud-based

### Notes
- Dongles are expensive — start with free tools (ADB, Fastboot, Odin, SP Flash Tool)
- Dongles save time on high-volume work — worth it when the bench is busy
- Some dongles require annual subscription renewals

---

## Workspace

- Workbench with good lighting
- Anti-static mat — prevents ESD damage to boards
- Parts organizer — small bins for screws, connectors, flex cables
- Label maker — labeling devices + parts by customer/order
- Backup power (UPS) — prevents corruption during firmware flash
