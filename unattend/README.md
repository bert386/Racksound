# Racksound / Defeed-Rack — Unattended Windows 11 install

`autounattend.xml` in this folder provisions a Windows 11 Pro rack image with
zero prompts: wipe + install + debloat + coarse power/USB tuning, one
auto-logon operator account, Windows Update disabled.

It is **provisioning only**. It does **not** install or launch the Racksound
host, and it does **not** apply the real-time audio tuning (MMCSS, IRQ/DPC
steering, timer resolution, etc.) — that is the separate **System Optimiser**,
run *after* first boot. So a full rack build is two steps:

1. This unattend → clean, debloated, tuned-enough Windows.
2. System Optimiser → the audio-specific real-time settings.

---

## ⚠️ Security posture of THIS file — "appliance-open"

This profile is deliberately **wide open**, matching an isolated/air-gapped
rack:

- Local admin **`Racksound`** with a **blank password**, **persistent
  auto-logon**.
- **Remote Desktop ON** (+ firewall rule).
- **Windows Defender, UAC, SmartScreen all OFF.**
- **Windows Update disabled** (services + policy).
- Telemetry minimised, Fast Startup off, sleep/hibernate off, USB selective
  suspend off, High Performance power plan.

**Only deploy this on a physically isolated / air-gapped LAN.** Never on a box
reachable from the open internet. For a networked rack, use the **Hardened**
changes at the bottom of this file.

---

## Build a bootable USB

1. Make a normal Windows 11 install USB (Rufus, or the Media Creation Tool, or
   just extract a Win 11 ISO onto a FAT32/exFAT USB).
2. Copy **`autounattend.xml`** to the **root** of that USB (same level as
   `setup.exe` / the `sources` folder). The filename must be exactly
   `autounattend.xml` — Windows Setup auto-detects it there.
3. Boot the target rack PC from the USB. Setup runs end-to-end with no prompts
   and reboots into a ready, logged-in desktop.

> Alternatively the file also works if placed at `sources\$OEM$\` style layouts
> or passed via `setup.exe /unattend:autounattend.xml`, but USB-root is simplest.

**Disk warning:** it targets **Disk 0** and **wipes it completely**
(`WillWipeDisk`). Make sure the rack's OS SSD is disk 0 and nothing else you
care about is attached.

---

## What it configures (quick map)

| Pass | Does |
|------|------|
| `windowsPE` | Bypasses TPM/SecureBoot/RAM/CPU/storage checks; GPT-partitions disk 0 (300 MB ESP + 16 MB MSR + Windows fills the rest); applies the **Windows 11 Pro** image; generic edition key. |
| `specialize` | Computer name `RACKSOUND`; AEST timezone; UAC/SmartScreen/Defender off; RDP on; Windows Update off; telemetry off; Fast Startup off; long paths on. |
| `oobeSystem` | en-AU locale (US keyboard); blank-password auto-logon admin; persistent auto-logon; GeoID 12; power/USB tuning; debloat ~35 bundled apps; Explorer tweaks. |

Region is **Australia**: `SystemLocale`/`UserLocale` = `en-AU`, keyboard = US
(`0c09:00000409`), GeoID 12, timezone `AUS Eastern Standard Time`. UI language
stays `en-US` (no extra language pack needed). To reship elsewhere, change those
five values.

---

## Notes / gotchas

- **Product key** `VK7JG-NPHTM-C97JM-9MPGT-3V66T` is Microsoft's *public generic
  Win 11 Pro key*. It only selects the **edition** during setup — it does **not
  activate** Windows. Activate separately (your own licence / KMS / MAK).
- **No recovery partition.** WinRE lives inside the Windows partition. Simpler
  and bulletproof on any SSD size; fine for an appliance. Add a trailing
  recovery partition only if you specifically need one.
- **Defender may not stay fully off** on the very latest Win 11 builds — Tamper
  Protection can re-enable it at runtime despite the service/registry changes.
  If you need it *guaranteed* off, use the **offline-hive method** (see below),
  which sets the service start values *before* first boot when Tamper Protection
  isn't running yet.
- **Windows 11 IoT Enterprise LTSC:** this file works as-is (same setup engine).
  The debloat list is a no-op for apps IoT LTSC doesn't ship (they're just
  "not installed" and skipped). On IoT you additionally get UWF / Shell
  Launcher / Assigned Access — configure those *after* install, not here.

---

## Guaranteed-Defender-off variant (offline hive)

If the runtime service disable isn't sticking, disable Defender against the
*offline* target hive during `windowsPE`, before Windows ever boots. That
requires applying the image with a script (so the target drive letter is known)
rather than the declarative `<ImageInstall>` used here — the same technique the
reference LSH file uses. Ask and I'll produce that variant; it's a bigger,
script-driven `windowsPE` pass, so I kept the default file clean and declarative.

---

## Turning this into the "Hardened" (networked) profile

Change these in `autounattend.xml`:

1. **Account password** — put a real value in **both** `<Password>` blocks
   (the `LocalAccount` and the `AutoLogon`), or drop `<AutoLogon>` entirely so
   the box requires a login.
2. **RDP off** — set `fDenyTSConnections` back to `1` (specialize Order 5) and
   remove the firewall `SynchronousCommand` (Order 10). Our Racksound Remote
   replaces RDP anyway.
3. **UAC on** — set `EnableLUA` to `1` (Order 2).
4. **SmartScreen on** — remove Orders 3–4.
5. **Defender on** — remove Orders 6–9.
6. **Windows Update** — decide: leave scheduled but pin/pause it rather than
   hard-disabling (remove Orders 10–12) so the box still gets security fixes.

Keep the debloat + power/USB/no-sleep tuning in both profiles — those are
appliance hygiene, not security posture.
