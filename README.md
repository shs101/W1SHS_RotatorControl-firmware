# W1SHS Rotator Control — Firmware

Public firmware downloads and over-the-air (OTA) update manifest for the
**W1SHS Rotator Control Enhanced** (Adafruit Feather ESP32-S3). The controller
checks this repository for new firmware and can update itself over WiFi.

> Source code is maintained privately; only built release binaries are published
> here.

## Latest firmware

See the [**Releases**](../../releases) page. Each release contains:

| File | Use |
|------|-----|
| `firmware.bin` | Application image (used by over-the-air update) |
| `firmware-recovery-X.Y.uf2` | **Full recovery image** for the drag-and-drop method below |

---

## Updating your rotator (normal method — over WiFi)

No cables, no software needed.

1. Open the controller's web page (e.g. `http://rotator.local/`).
2. Go to the **Firmware** tab.
3. Click **Check for Updates**. If a newer version exists, it shows the version
   and release notes.
4. Click **Install Update** and confirm.

The controller stops all motion, downloads the new firmware, and reboots into it
(about 20–30 seconds). The page reloads automatically.

**Safe by design:** the new firmware is written to a spare flash slot and only
activated after it is fully downloaded and verified. If WiFi drops or power is
lost mid-update, the controller simply keeps running the **old** firmware —
**an interrupted update will not brick it.**

---

## Recovery — if the controller won't boot or update

If a firmware ever boots into a broken state (rare — only possible from a bad
build, not from an interrupted download), recover it with the **UF2 drag-and-drop**
method. No toolchain required.

1. Download the latest `firmware-recovery-X.Y.uf2` from [Releases](../../releases).
2. Connect the controller to your computer with a **USB cable**.
3. **Double-press the RESET button** (two quick presses). A USB drive named
   something like **`FTHRS3BOOT`** appears.
4. **Drag `firmware-recovery-X.Y.uf2` onto that drive.**
5. The drive copies the image, the controller reboots automatically into the
   restored firmware, and the drive disappears.

That image is a complete restore (bootloader, partitions, and application), so it
recovers the device to a known-good state.

> Tip: keep a copy of the recovery `.uf2` on your computer so it's ready if ever
> needed.

---

## How automatic update checking works (technical)

`version.json` at the repo root is the update manifest the controller reads
(via `raw.githubusercontent.com`, a direct download):

```json
{ "version": "1.5", "url": ".../firmware/firmware.bin", "notes": "..." }
```

The controller compares `version` against its running version and offers the
update when newer. `firmware/firmware.bin` is the current application image it
downloads.
