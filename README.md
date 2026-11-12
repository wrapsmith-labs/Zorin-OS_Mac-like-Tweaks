# Zorin Mac Setup

A one-script installer that makes [Zorin OS](https://zorin.com/os/) look and feel more like macOS. Built with older Intel MacBook Pros in mind, but the theming parts work on any Zorin machine.

> **Status: pre-release.** This script has had limited testing. Take a snapshot before running it (the script offers to do this for you), and expect rough edges.

## What it does

| Area | What you get |
|---|---|
| **Look** | [WhiteSur](https://github.com/vinceliuice/WhiteSur-gtk-theme) GTK theme, icon theme and cursors, with Inter as the system font |
| **Layout** | Window buttons on the left, a centred dock (Dash to Dock), translucent panels (Blur my Shell) |
| **Input** | Natural scrolling and tap-to-click on the touchpad |
| **Launcher** *(optional)* | [Ulauncher](https://ulauncher.io/) as a Spotlight-style launcher |
| **Shortcuts** *(optional)* | [Kinto](https://kinto.sh) for Mac-style keyboard shortcuts (Cmd+C, Cmd+Tab, etc.) |
| **MacBook hardware** | Fan control (mbpfan), battery tuning (TLP), F-keys acting as media keys by default |
| **Safety** | Creates a Timeshift snapshot before changing anything |

## Requirements

- Zorin OS 17 or 18 (Ubuntu-based, GNOME desktop)
- An internet connection
- A normal user account with `sudo` rights
- About 1 GB of free disk space

## Quick start

```bash
git clone https://github.com/wrapsmith-labs/Zorin-OS_Mac-like-Tweaks.git
cd Zorin-OS_Mac-like-Tweaks
chmod +x zorin-mac-setup.sh
./zorin-mac-setup.sh
```

**Do not run it with `sudo`.** The script asks for your password when it needs it. Running it as root would put the theme files in the wrong place.

When it finishes, **log out and back in** (or reboot) to activate the extensions and the function-key change.

## Options

```
./zorin-mac-setup.sh [options]
```

| Flag | Effect |
|---|---|
| `--no-theme` | Skip the theme, icons, cursors, fonts and GNOME extensions |
| `--no-hardware` | Skip fan control, TLP and the function-key change (use this on non-MacBook hardware) |
| `--ulauncher` | Also install the Ulauncher launcher |
| `--kinto` | Also install Kinto (runs its own interactive installer) |
| `--dark` / `--light` | Pick the WhiteSur colour variant (default: dark) |
| `-h`, `--help` | Show usage |

**Suggested first run:** `./zorin-mac-setup.sh --ulauncher`. Add `--kinto` in a second run, so if something misbehaves you know which piece caused it.

## Before you run it: set up Timeshift

The script takes a snapshot before changing anything, but Timeshift has to be configured once first.

1. Open **Timeshift** from the app menu.
2. Choose a snapshot type (RSYNC is fine) and a location with enough space.
3. Finish the wizard, then run the script.

If Timeshift isn't configured, the script will stop and ask whether you want to continue without a snapshot.

## After installing

- **Launcher hotkey:** open Ulauncher's preferences and set the hotkey to `Super+Space`.
- **Two docks?** Zorin ships its own taskbar, which can clash with Dash to Dock. Disable one:
  ```bash
  gnome-extensions disable zorin-taskbar@zorinos.com
  ```
- **Wi-Fi not working?** Many MacBooks use Broadcom chips that need a proprietary driver. Open **Software & Updates → Additional Drivers**.
- **Wallpaper:** the script doesn't change it. Add a macOS-style wallpaper through Settings → Appearance.

## Known issues and trade-offs

- **Updates can break the theme.** Heavy theming (WhiteSur, Blur my Shell and Dash to Dock together) can misbehave after a GNOME or Zorin update. If that happens, disable the extensions first, then check the WhiteSur repo for updates.
- **Older hardware pays a small performance cost.** Blur effects are the main cause. If the desktop feels sluggish, disable Blur my Shell in the Extension Manager.
- **Kinto isn't perfect.** It remaps shortcuts system-wide, and a few apps behave oddly with it. It can be uninstalled with its own uninstaller.
- **Limited testing.** This hasn't been tested across every MacBook model or Zorin release.

## Undoing the changes

- **Full rollback:** restore the Timeshift snapshot taken at the start.
- **Reset appearance only:**
  ```bash
  gsettings reset-recursively org.gnome.desktop.interface
  gsettings reset-recursively org.gnome.desktop.wm.preferences
  ```
- **Remove the function-key change:**
  ```bash
  sudo rm /etc/modprobe.d/hid_apple.conf
  sudo update-initramfs -u
  ```

## What the script installs

Everything it fetches comes from public sources, so you can inspect before running:

- `apt` packages: `git`, `curl`, `wget`, `gnome-tweaks`, `gnome-shell-extension-manager`, `timeshift`, `pipx`, `fonts-inter`, `mbpfan`, `tlp`, `tlp-rdw`, plus build dependencies for the theme
- Git repositories: WhiteSur GTK theme, icon theme and cursors (by vinceliuice)
- GNOME extensions via `gnome-extensions-cli`: Blur my Shell, Dash to Dock
- Optional: Ulauncher (from its PPA) and Kinto (from its GitHub installer)

Reading a script before running it is good practice. The whole thing is a single readable file.

## Contributing

Issues and pull requests are welcome, especially reports of which Zorin versions and MacBook models work.

## Licence

Add your preferred licence here (MIT is a common choice for scripts like this).

## Credits

- [WhiteSur themes](https://github.com/vinceliuice) by vinceliuice
- [Kinto](https://github.com/rbreaves/kinto) by rbreaves
- [Ulauncher](https://ulauncher.io/)
- [Blur my Shell](https://github.com/aunetx/blur-my-shell) and [Dash to Dock](https://github.com/micheleg/dash-to-dock)
