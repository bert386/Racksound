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
   **[`bin/RacksoundHost-Setup-0.1.0.exe`](bin/RacksoundHost-Setup-0.1.0.exe)**
   (or grab it from **[Releases](../../releases/latest)** if published there).
2. Run it. Default install path is `C:\Program Files\Racksound Host`.
   Silent install: `RacksoundHost-Setup-0.1.0.exe /VERYSILENT /NORESTART`.
3. Launch **Racksound Host**. It lives in the system tray; the control app opens
   from the tray icon.

Verify the download (optional):

```
CertUtil -hashfile RacksoundHost-Setup-0.1.0.exe SHA256
# expect: 9f8d938ee71fd2ed400369f9b3e56867fd100937174144eda8c36cc2b1a81c41
```

### Add your feedback-suppression VST3 (done once, on the box)

The installer does **not** ship a plugin. On the host: **Settings → Manage
Plugins → Add VST3**, point it at your `.vst3`, then pick it per channel. Any
VST3 works; typical choices are AlphaLabs **Defeedback** or **Racksound
Restore**. You are responsible for holding a valid licence for whatever plugin
you load.

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

It carries the current version, the release-asset download URL, file size and
SHA-256. A future Racksound Host build can poll this, compare versions, and
offer a one-click update. To update manually, just download and run the newest
installer from Releases — it upgrades in place.

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
