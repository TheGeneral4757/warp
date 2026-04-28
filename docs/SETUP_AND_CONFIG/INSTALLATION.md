# Installation

Install Warp on your system.

---

## macOS

### Using Homebrew (Easiest)

```bash
brew install warp
```

Then launch Warp from Applications or run:
```bash
warp
```

### Manual Installation

1. Download the latest macOS release from [releases.warp.dev](https://releases.warp.dev)
2. Open the `.dmg` file
3. Drag "Warp" into the "Applications" folder
4. Launch from Applications or Spotlight

### M1/M2/M3 Mac?

Warp automatically detects your architecture. Just install as above — no special setup needed.

---

## Linux

### Ubuntu / Debian

```bash
curl https://releases.warp.dev/releases/stable/warp-v0.2024.01.09/warp-terminal-v0.2024.01.09.AppImage -o warp.AppImage
chmod +x warp.AppImage
./warp.AppImage
```

Or use your package manager if available.

### Fedora / RHEL

Check [releases.warp.dev](https://releases.warp.dev) for the `.rpm` package:

```bash
sudo dnf install ./warp-v0.2024.01.09.rpm
warp
```

### Arch Linux

Warp is available in the AUR:

```bash
yay -S warp-terminal
warp
```

### Other Distributions

Download the AppImage from [releases.warp.dev](https://releases.warp.dev):

```bash
chmod +x warp-terminal-v0.2024.01.09.AppImage
./warp-terminal-v0.2024.01.09.AppImage
```

---

## Windows

### Using Windows Package Manager

```bash
winget install warp
```

Then launch Warp from Start Menu or search.

### Using Chocolatey

```bash
choco install warp
```

### Manual Installation

1. Download the latest Windows installer from [releases.warp.dev](https://releases.warp.dev)
2. Run the `.exe` file
3. Follow the installation wizard
4. Launch Warp from Start Menu

### Windows Terminal Integration

Warp can be integrated as a profile in Windows Terminal (optional):

1. Open Windows Terminal Settings
2. Go to **Profiles** → **Add new**
3. Set command line: `C:\Program Files\Warp\warp.exe`
4. Name it "Warp"
5. Save

---

## System Requirements

### macOS
- macOS 10.13 or later
- Intel or Apple Silicon

### Linux
- Linux kernel 4.0+
- glibc 2.17+ (usually installed)
- X11 or Wayland desktop environment

### Windows
- Windows 10 or later
- 64-bit system

---

## Verify Installation

After installing, verify Warp is working:

```bash
warp --version
```

Should print something like: `warp 0.2024.01.09`

---

## First Run

When you launch Warp for the first time:

1. **Shell selection** — Choose your default shell (bash, zsh, fish, powershell)
2. **Cloud sync** — Sign in to Warp Cloud (optional, but recommended for syncing settings)
3. **Keyboard shortcuts** — See default shortcuts or customize them
4. **Welcome tour** — Brief tour of main features

---

## Updating Warp

### On macOS (Homebrew)

```bash
brew upgrade warp
```

### On Linux

Download the latest version from [releases.warp.dev](https://releases.warp.dev) and reinstall.

### On Windows (Windows Package Manager)

```bash
winget upgrade warp
```

Warp also notifies you when updates are available.

---

## Troubleshooting Installation

### "Warp won't launch"

1. **Check system requirements** — Above
2. **Reinstall** — Uninstall completely and reinstall
3. **Check logs** — Logs are in:
   - Mac: `~/Library/Application Support/Warp/logs/`
   - Linux: `~/.local/share/warp/logs/`
   - Windows: `%AppData%\Warp\logs\`

### "Permission denied" (Linux)

If you get "Permission denied", you may need to make it executable:

```bash
chmod +x warp.AppImage
```

### "Cannot find command" after installation

Your shell may need to refresh its PATH:

```bash
# Bash
source ~/.bashrc

# Zsh
source ~/.zshrc

# Fish
source ~/.config/fish/config.fish
```

Or just close and reopen your terminal.

### "Warp conflicts with another terminal"

Warp should coexist with other terminals. If you get conflicts:

1. Check which app is trying to claim the keybinding
2. Go to System Preferences → Keyboard → Shortcuts and reassign conflicts
3. Restart Warp

---

## Next Steps

1. **[First Time Setup](./FIRST_TIME_SETUP.md)** — Complete your initial configuration
2. **[QUICK_START](../QUICK_START.md)** — Learn the basics in 5 minutes
3. **[Selecting Models](../AI_MODELS/SELECTING_MODELS.md)** — Choose your AI model

---

[👈 Back to Setup and Config](./README.md)
