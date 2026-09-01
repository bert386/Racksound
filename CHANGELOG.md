# Changelog

All notable changes to Racksound Host, newest first.

## 0.7.0 — 2026-09-01
- **Update Display Firmware** button (Settings → Front-panel display). Shows the
  connected board's firmware version and, when it's behind the bundled one,
  reflashes it over USB — **no BOOT button** (uses the 1200-baud bootloader
  touch). The display firmware now reports its version in the beacon.

## 0.6.0 — 2026-08-28
- **Front-panel display: proper "PC OFF" state.** When the computer is shut down
  (with the display still on standby power), the panel now shows a calm
  **"PC OFF / standby"** instead of a red "not responding" error. Requires
  re-flashing the display board (or flashing a fresh one) with the bundled
  firmware.

## 0.5.0 — 2026-08-28
- **Auto-login** is now part of System Optimisation. On a dedicated rack,
  Optimise sets the box to sign in automatically and launch the host — no manual
  login. (Engages for a passwordless operator account; reversible via Restore.)

## 0.4.0 — 2026-08-28
- **Check for Updates** button (Overview page). Reads the release feed, tells you
  if a newer version exists, and — on confirmation — downloads the installer,
  verifies its checksum, and launches it.

## 0.3.0 — 2026-08-28
- **Flash the front-panel display from the host.** Plug in a blank RP2040 board
  and the host detects it and offers to flash the OLED display firmware — no
  Arduino tools or BOOTSEL button needed.
- **Display firmware auto-detects the OLED** at I²C 0x3C or 0x3D, and blinks the
  status LED red (instead of going blank) if no panel answers — making a wiring
  problem obvious.

## 0.2.0 — 2026-08-28
- **Built-in System Optimiser.** Settings → System Optimisation shows a live
  "is this PC tuned for real-time audio?" status (10 OS checks), with **Details**
  per check.
- **One-click Optimise / Restore.** Applies power, service and multimedia tuning
  via an elevated helper; every change is backed up first, so **Restore defaults**
  reverts cleanly. Intended for a dedicated rack.

## 0.1.0 — 2026-08-14
- Initial public release.
- Multi-VST host: add plugins to a library, select per channel, curated
  parameter surfacing to the channel strip.
- Decoupled processing (off-callback reblock) as the default.
- Racksound Remote web control panel served on the local network.
- Optional front-panel USB OLED status display support.
