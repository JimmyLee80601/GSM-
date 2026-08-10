# The Bench

## Workstation

- **Machine:** jimmysgsmworkstation
- **OS:** Windows 10 Pro 22H2 build 19045.7548
- **Storage:** ~183 GB free on C:
- **CPU/RAM:** [fill in — not documented yet]

## Software stack

### Core tools
- **ADB / Fastboot** — Android Debug Bridge, device communication, firmware flashing, shell access
- **Python 3.12** — scripting, automation, custom tools
- **Visual Studio 2022 Community** — C# apps, Windows tooling
- **VS Code** — scripting, markdown, project work
- **Notepad++** — quick edits, log viewing

### AI assist
- **Ollama** (port 11434) — local LLM inference
- **LM Studio** (port 1234) — local model serving, currently serving Qwen3-4B + Qwen2.5-3B + Gemma-3-4B + Qwythos-9B
- **NVIDIA Cloud** — deepseek-v4-flash-0731 via API (free tier)
- **HarleyStation** — personal AI assistant, voice control, device management

### Communication
- **Tailscale** — machine at 100.78.184.121, phone at 100.126.38.38
- **HarleyLink** — relay for phone-to-PC control (screen sharing, input, typing)
- **Chrome Remote Desktop** — remote access backup

## Phone (daily driver)

- Samsung Galaxy S23 Ultra
- Serial: R3CW10HMPCZ
- USB 3.0 connected to workstation
- ADB authorized
- Tailscale VPN connected

## Physical setup

- **Bench location:** Brighton, CO — home bench, customers come to me
- **Backup meetup:** McDonald's (when home bench isn't convenient)
- **Hours:** Mon–Sat 10am–8pm, Sun by appointment
- **Payment:** cash or digital on completion

## Network

- Tailscale funnel: https://jimmysgsmworkstation.tail8deeb5.ts.net
  - Port 443 → localhost:8443 (HarleyLink relay)
- Phone Tailscale IP: 100.126.38.38

## Notes

- Windows schannel cannot TLS to api.buywander.com (SEC_E_ILLEGAL_MESSAGE) — use httpx/curl, not PowerShell Invoke-RestMethod
- ShellExperienceHost crash-looping (Windows.UI.Xaml.dll 10.0.19041.7548) — notification center dead, waiting for Aug 11 patches
