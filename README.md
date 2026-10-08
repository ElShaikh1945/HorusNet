<p align="center">
  <img src="assets/horusnet_logo.svg" width="220" alt="HorusNet Logo" />
</p>

# 𓂀 HorusNet v2.1.0

```text
 _   _                         _   _      _   
| | | | ___  _ __ _   _ ___   | \ | | ___| |_ 
| |_| |/ _ \| '__| | | / __|  |  \| |/ _ \ __|
|  _  | (_) | |  | |_| \__ \  | |\  |  __/ |_ 
|_| |_|\___/|_|   \__,_|___/  |_| \_|\___|\__|
```

> **Network Traffic Monitor & Analyzer for Linux**  
> *Offline-First • Zero External Dependencies • Zero Root Required • Flicker-Free TUI*

---

## 🌟 Overview

**HorusNet** is a modern, decoupled, dependency-free network monitoring suite for Linux. It directly interfaces with Linux kernel counters (`/proc/net`) to track bandwidth usage across all network interfaces and individual applications, without requiring root/sudo privileges or external Python packages.

### ✨ Key Features
- **Zero Dependencies:** Pure Python 3 standard library. No `pip install` required.
- **100% Privacy & Offline:** No phone-home, zero telemetry, local SQLite storage.
- **No Root Required:** Maps open sockets and processes for the current user safely.
- **Smart Quota Management:** Calculates daily allowances automatically ($\text{Quota} \div \text{Days}$) and sends native desktop alerts via `notify-send`.
- **Flicker-Free TUI:** Fast, in-place ANSI terminal interface supporting arrow keys and direct number selection.
- **Deep App Inspection:** Historical timeline and active duration tracking for each individual application.
- **Hourly Visual Charts:** Peak-hour traffic visualization with Unicode bar charts.
- **Decoupled Configuration:** Complete separation of identity, themes, settings, and bilingual localization in standalone JSON files.
- **Multiple Export Formats:** Standalone dark-mode HTML dashboards, CSV spreadsheets, and JSON API payloads.

---

## 📁 Repository Structure

```text
HorusNet/
├── horusnet                  # Main executable script
├── branding.json             # App identity, name, ASCII logo & taglines
├── settings.json             # User settings (quota, alerts, language, theme)
├── themes.json               # 7 Built-in palettes + custom theme support
├── LICENSE                   # MIT License
├── README.md                 # Project guide & quick start
├── .gitignore                # Git exclusions
├── assets/                   # Vector logo assets
│   ├── horusnet_logo.svg     # Minimalist monoline vector logo
│   └── horusnet_logo.png     # Rendered high-res PNG (512x512)
├── locales/                  # Localization dictionaries
│   ├── en.json               # English dictionary (126 keys)
│   └── ar.json               # Arabic dictionary (126 keys)
└── systemd/                  # Automated 5-minute background collector
    ├── netusage-updater.service
    └── netusage-updater.timer
```

---

## 🚀 Quick Start & Installation

### 1. Run Directly
```bash
./horusnet
```

### 2. Make Available System-Wide
Create a symlink in your user binary directory:
```bash
mkdir -p ~/.local/bin
ln -sf "$(pwd)/horusnet" ~/.local/bin/horusnet
```
Ensure `~/.local/bin` is in your `$PATH`. You can then launch HorusNet from anywhere by simply typing:
```bash
horusnet
```

### 3. Enable 5-Minute Background Tracking (Systemd)
To ensure continuous traffic tracking even when the terminal interface is closed:
```bash
mkdir -p ~/.config/systemd/user
cp systemd/* ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now netusage-updater.timer
```

---

## 🛠️ CLI Usage Guide

HorusNet supports both an interactive terminal interface and script-friendly command-line flags:

| Command | Description |
| :--- | :--- |
| `horusnet` | Launches the interactive TUI menu |
| `horusnet -d [N]` | Displays daily traffic history (default: last 14 days) |
| `horusnet -m` | Displays monthly traffic history |
| `horusnet -H [DATE]` | Displays hourly visual chart for today or specific date |
| `horusnet -a ALL` | Lists all active applications sorted by traffic |
| `horusnet -a <name>` | Deep inspection of specific application (e.g. `horusnet -a brave`) |
| `horusnet -i` | Shows network interfaces, IP addresses, and status |
| `horusnet -l [IFACE]` | Launches live bandwidth speed meter (press `q` or `Esc` to exit) |
| `horusnet -e html [path]` | Exports a dark-mode interactive HTML report |
| `horusnet -e csv [path]` | Exports traffic data to CSV spreadsheet |
| `horusnet -e json [path]` | Exports traffic data to JSON |

---

## 🎨 Customization (JSON-Driven)

All aspects of HorusNet can be modified without touching Python code:

### 1. Themes (`themes.json`)
Edit existing themes or add your own palettes using standard ANSI color escapes:
```json
"dracula": {
  "id": "dracula",
  "name_en": "Dracula Violet",
  "name_ar": "دراكولا البنفسجي (Dracula)",
  "primary": "\u001b[95m",
  "secondary": "\u001b[35m",
  "accent": "\u001b[32m",
  "highlight": "\u001b[93m",
  "dim": "\u001b[2m",
  "border": "\u001b[95m"
}
```

### 2. Quota & Settings (`settings.json`)
Configure your monthly plan and warning thresholds:
```json
{
  "language": "en",
  "theme": "cyan",
  "desktop_notifications": true,
  "quota": {
    "enabled": true,
    "total_limit_gb": 30.0,
    "cycle_start_day": 16,
    "daily_limit_gb": 0.0,
    "daily_warn_percent": 80,
    "warn_threshold_percent": 80,
    "crit_threshold_percent": 95
  }
}
```
*(Setting `daily_limit_gb` to `0.0` automatically divides remaining quota by remaining cycle days).*

### 3. Identity (`branding.json`)
Customize the application name, ASCII logo, and taglines.

### 4. Localization (`locales/`)
All UI strings and labels are stored in `locales/en.json` and `locales/ar.json`. To add a new language, simply create `<lang_code>.json` in the `locales/` directory.

---

## 📄 License & Privacy

- **License:** [MIT License](LICENSE) © 2026 Muhammad Al-Shaikh
- **Privacy Guarantee:** 100% Offline. No external requests are made under any circumstances.
