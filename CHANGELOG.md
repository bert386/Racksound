# Changelog

All notable changes to Racksound Host, newest first.

## 0.11.0 — 2026-11-14
- **Major latency fix: the plugin "window" is now tunable.** The host always
  prepared the plugin at a 2048-sample block, which sized its internal
  look-ahead and added ~20 ms — regardless of the processing block. A generic
  VST host prepares at its buffer (e.g. 16 samples) for near-zero latency.
  System page now has a **Plugin window** selector: dial it down toward the
  processing block to match VSTHost-grade latency. Default stays 2048 (no
  change for existing setups); smaller can fuzz on some plugins, so step down
  and listen. Decoupled mode runs the DSP off the deadline, so small windows are
  clean there.

## 0.10.8 — 2026-11-11
- **Adds a 4-sample processing block** (experimental, lowest possible). Deep into
  diminishing returns — the interface/Dante latency dominates below ~16; watch
  the Dropouts meter.

## 0.10.7 — 2026-11-11
- **Adds an 8-sample processing block** (experimental). Lowest decoupled latency,
  but into diminishing returns — watch the Dropouts meter; the plugin may buffer
  internally so the real latency gain can be small.

## 0.10.6 — 2026-11-11
- **Processing block now goes down to 16 and 32 samples** (System page), for
  lower latency in decoupled mode on a tuned, low-jitter rack. Smaller = less
  latency but tighter on the DSP — step down while watching the Dropouts meter.

## 0.10.5 — 2026-11-11
- **Latency reporting refined + the key lever explained.** The decoupled
  overhead is counted as two processing blocks (one to fill the input, one of
  output priming). The overhead is from **our decoupled mode**, not the plugin —
  turn **Decoupled DFB off** (System page) to run inline like a normal VST host,
  where Defeedback adds no latency. Watch the Dropouts meter.

## 0.10.4 — 2026-11-11
- **Latency is now reported honestly.** The figure was only the audio
  interface's in/out latency. It now includes the **decoupled processing block**
  and the **plugin's own look-ahead**, so the displayed round-trip matches what
  you actually hear.
- Added a **32-sample processing block** option (System → Processing block) for
  lower latency on a tuned rack (watch the Dropouts meter).

## 0.10.3 — 2026-11-10
- **Fix: the per-channel "Test signal (pink)" now works.** It was only
  implemented in the legacy inline processing path, so on a normal (decoupled)
  setup the button did nothing. Pink noise is now injected on the channel's
  output in both modes, so it shows on the output meter and reaches the output.

## 0.10.2 — 2026-11-10
- **Fix: "Check for Updates" could report a stale version.** It now cache-busts
  the feed, so it always sees the current release. (0.10.0/0.10.1 could wrongly
  say you were on the latest.) Also includes the RedNet PCIe device-switch fix.

## 0.10.1 — 2026-11-10
- **Fix: switching audio devices could fail with "unspecified error in
  initialisation."** The host was carrying the previous device's sample
  rate/buffer onto the newly selected one. A card locked to its own clock
  (e.g. a **Dante / Focusrite RedNet PCIe** card at 48 kHz) now opens at its
  own native rate instead.

## 0.10.0 — 2026-09-01
- **Front-panel display knows when the PC is off.** New display firmware (v3)
  adds optional **VBUS-sense**: with one wire it definitively distinguishes
  **PC shut down** from **Windows booting** and from **Racksound stalled/stopped**,
  instead of guessing. Works with or without the wire (falls back to the old
  heuristic). Re-flash the board (Settings → Front-panel display → Update Firmware).

## 0.9.0 — 2026-09-01
- **Optimise clears the operator account password** so auto-login engages even on
  a rack where Windows setup forced a password. Applies to a **local** account.
  Note: **Restore does not put the password back** — set a new one in Windows if
  you want it. After Optimise + reboot, the rack signs in and launches Racksound
  with no interaction.

## 0.8.0 — 2026-09-01
- **Optimise now also sets Racksound to launch on logon** (previously a separate
  "Start with Windows" toggle). One Optimise pass sets the audio tuning,
  auto-login, and host-startup together, so a dedicated rack boots straight into
  Racksound. (Auto-login still needs a passwordless account.)

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
