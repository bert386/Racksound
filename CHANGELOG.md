# Changelog

All notable changes to Racksound Host, newest first.

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
