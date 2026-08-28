# Racksound Host

Control host for the **Defeed-Rack** — a 1U live-sound feedback-suppression
appliance by **Eastec Audio**. Racksound Host runs the audio engine, hosts your
feedback-suppression VST3 per channel, and serves the **Racksound Remote** web
control panel to any phone/tablet on the same network.

> This repository distributes the **Racksound Host installer** and is the
> **update source** the host checks for new versions. It does **not** include
> any VST3 plugin — you add your licensed plugin on the box (see below).

---

## Download & install

1. Download the installer from
   **[`bin/RacksoundHost-Setup-0.4.0.exe`](bin/RacksoundHost-Setup-0.4.0.exe)**
   (or grab it from **[Releases](../../releases/latest)** if published there).
2. Run it. Default install path is `C:\Program Files\Racksound Host`.
   Silent install: `RacksoundHost-Setup-0.4.0.exe /VERYSILENT /NORESTART`.
3. Launch **Racksound Host**. It lives in the system tray; the control app opens
   from the tray icon.

Verify the download (optional):

```
CertUtil -hashfile RacksoundHost-Setup-0.4.0.exe SHA256
# expect: 704e395bf8eb127ab97afd5ed405d4b33e67f51426ed0c93c518f3ccc6375ce4
```

### Add your feedback-suppression VST3 (done once, on the box)

The installer does **not** ship a plugin. On the host: **Settings → Manage
Plugins → Add VST3**, point it at your `.vst3`, then pick it per channel. Any
VST3 works; typical choices are AlphaLabs **Defeedback** or **Racksound
Restore**. You are responsible for holding a valid licence for whatever plugin
you load.

### Tune the rack (System Optimiser)

Settings → **System Optimisation** shows whether Windows is tuned for real-time
audio (power plan, background services, multimedia scheduling, etc.). On a
**dedicated rack**, click **Optimise this PC** to apply the tuning (a Windows
elevation prompt may appear; on a locked-down rack image there's no prompt).
Every change is backed up, so **Restore defaults** reverts it cleanly. Don't run
this on a general-purpose computer — it's meant for an appliance.

### Front-panel display (optional)

If you have the front-panel OLED (Waveshare RP2040-Zero + 0.91″ SSD1306), plug a
**blank board** into the rack via USB — the host detects it and offers to **flash
the display firmware** automatically (no Arduino tools or BOOTSEL button). Once
flashed it shows live status; the host renders the screen, so future layout
changes ship in host updates, not firmware re-flashes.

---

## Requirements

- Windows 11 (Pro or IoT Enterprise LTSC), 64-bit.
- An ASIO audio interface (or the bundled engine's device selection).
- The Racksound Remote panel is served on the local network — open the URL the
  host shows (default port `8422`) from a phone/tablet browser.

---

## Updates

`latest.json` in this repo is the machine-readable update feed:

```
https://raw.githubusercontent.com/bert386/Racksound/main/latest.json
```

It carries the current version, the download URL, file size and SHA-256. From
**0.4.0** the host has a **Check for Updates** button (Overview page) that reads
this feed, and — if a newer version exists — downloads the installer, verifies
its checksum, and runs it. To update manually, just download and run the newest
installer — it upgrades in place.

---

---

## Rack imaging (unattended Windows install)

[`unattend/autounattend.xml`](unattend/autounattend.xml) provisions a Windows 11
Pro rack image with zero prompts: wipe + install + debloat + coarse power/USB
tuning, one auto-logon operator account. Drop it at the root of a bootable
Windows 11 USB. See [`unattend/README.md`](unattend/README.md) for the full
walkthrough and the security-posture warnings.

> ⚠️ The bundled profile is deliberately **wide open** (blank-password
> auto-logon, RDP on, Defender/UAC off) and is for a **physically isolated /
> air-gapped rack only**. The README documents how to switch to a hardened,
> networked profile. It also **wipes Disk 0** — read before booting.

The image is provisioning only; the real-time audio tuning is applied by the
separate System Optimiser after first boot.

---

## License

Racksound Host is proprietary software. See [`LICENSE`](LICENSE). It is
distributed as a binary; VST3 plugins are not included and remain the property
of their respective vendors.

## Support

Eastec Audio. Issues and requests: use this repo's **Issues** tab.
