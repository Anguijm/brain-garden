---
title: "Batocera emulation appliance on the MINISFORUM UM790Pro"
type: topic-note
category: games
tags: [emulation, batocera, gaming, hardware, minisforum, setup, retro]
created: 2026-08-31
updated: 2026-08-31
sources_staged: false
draft: false
---

# Batocera emulation appliance on the MINISFORUM UM790Pro

The UM790Pro (Ryzen 9 7940HS, Radeon 780M, 64 GB DDR5) is a strong emulation host. This note covers turning it into a dedicated appliance using Batocera — a Linux-based OS designed from the ground up for emulation, with EmulationStation as the frontend and no Windows in the loop.

Assessment: The hardware is well-matched to Batocera. One real risk at install time — see the WiFi section before you start.

---

## What you need before you start

- A USB drive, 16 GB or larger, USB 3.0 or faster (this becomes your installer, not the final home for Batocera)
- A **USB-C to Ethernet adapter** — have one on hand. The UM790Pro's Intel Killer AX1675 Wi-Fi chip has a troubled history in Linux, and whether Batocera's kernel ships the right firmware blob is unverified. Wired Ethernet is your fallback for the initial setup and ROM downloads. Cheap adapters work fine; the USB-C port on the UM790Pro supports standard USB-A dongles via a hub too.
- An Xbox controller (Series X/S or One) — pairs over Bluetooth or USB, works out of the box
- A ROM library on an external drive, or ready to copy from another machine

---

## How Batocera works as an appliance

Batocera boots into EmulationStation — a controller-driven frontend — with no desktop OS visible. You navigate your library with the Xbox controller, launch games, and return to the frontend when done. There is no keyboard or mouse required after setup. The system can be configured entirely from the controller or via a SSH session from another machine on the same network.

Assessment (from Batocera 43.1 documentation): Batocera stores your ROMs, saves, and configuration on a `share` partition, separate from the OS. If you ever reinstall or upgrade Batocera, your game library stays intact.

---

## Storage layout for the appliance

The UM790Pro has two M.2 2280 PCIe Gen4 slots. The clean appliance setup uses both:

- **Slot 1 (primary):** Batocera OS installed here. Replaces Windows. ~60 GB used by the OS; the rest becomes the share partition for ROMs and saves.
- **Slot 2 (optional second drive):** A second NVMe for overflow ROMs, or a fast drive dedicated entirely to the ROM library. Batocera can be configured to use it automatically via `batocera-boot.conf` (`sharedevice` pointing to the second drive's UUID).

If you want to keep Windows available, an alternative is installing Batocera to an external USB4 SSD (very fast via the UM790Pro's USB4 port) and choosing the boot device on startup. This is slower to set up and adds a cable, but preserves the Windows installation.

Assessment: For a true appliance with no compromise, install to the internal NVMe and let Windows go.

---

## Install steps

**1. Download Batocera**

Go to batocera.org, download the x86_64 image (Batocera 43.1 as of this writing, ~1 GB).

**2. Flash the USB drive**

Use Balena Etcher or Rufus on Windows to flash the image to your USB drive. Rufus: select the image, select the USB drive, leave everything else default.

**3. Boot from USB**

Plug the USB into the UM790Pro. Power on and tap Delete or F2 to enter the BIOS, then set boot priority to the USB drive. Batocera will boot into EmulationStation from USB — this is a live environment, nothing is installed yet.

**4. Test hardware**

At this point test:
- Display output (should work immediately via HDMI)
- Xbox controller: plug in via USB or go to **Start → Controller & Bluetooth settings → Pair a Bluetooth device**, hold the Bluetooth button on the controller
- WiFi: go to **Start → Network Settings** and see if your network appears. If it does not, plug in the USB-C Ethernet adapter — network access is what matters for now, not WiFi specifically

**5. Install to internal NVMe**

From within Batocera (running from USB), go to **Start → System Settings → Install Batocera on a new disk**, select the internal NVMe. This will erase it. The installer partitions the drive, copies the OS, and creates the share partition. Takes a few minutes.

**6. Reboot and remove the USB**

Batocera will boot from the internal drive. Change boot priority back to the NVMe in the BIOS if needed.

---

## WiFi

Assessment: The UM790Pro uses the Intel Killer AX1675 (Wi-Fi 6E). This chip uses the `iwlwifi` Linux driver but requires specific firmware files. Community reports on other Linux distros are mixed — some work out of the box, some require copying firmware manually. Whether Batocera 43.1 ships the right firmware blob is unconfirmed.

If WiFi does not appear in Network Settings after install:

1. Connect via USB-C Ethernet adapter — you can do everything over wired
2. Check the Batocera forums for a current `iwlwifi` firmware fix; it typically involves copying a `.ucode` file to `/lib/firmware/` via SSH or a USB stick
3. Alternatively, a cheap USB Wi-Fi dongle (TP-Link Archer T2U or similar, well-supported in Linux) is a reliable fallback with no configuration needed

---

## Xbox controller

Assessment (from Batocera wiki): Xbox Series X/S and Xbox One controllers pair over standard Bluetooth LE — hold the Bluetooth button on the controller until it flashes rapidly, then pair from Batocera's controller settings. The standard kernel `xpad` driver handles it; no additional software needed.

Update the controller firmware first on Windows or via an Xbox console before moving to the appliance. An outdated controller firmware can cause pairing failures.

---

## What each system runs and how well

Assessment, based on UM790Pro hardware class and Batocera 43.1 emulator versions:

**Everything through sixth generation** (NES, SNES, N64, PS1, Saturn, Dreamcast, GBA, DS): runs at full speed with zero configuration. Upscaling to 4K is available and generally works well.

**PS2** (PCSX2, included): Excellent. The 7940HS handles virtually the entire PS2 library at full speed, most games upscalable. Enable the Vulkan renderer for best performance.

**GameCube / Wii** (Dolphin, included): Excellent. Full speed at 4K on almost everything.

**PSP** (PPSSPP, included): Flawless. Full speed, high resolution.

**3DS** (Lime3DS / Citra fork, included): Good. Most games run well; a few demanding titles need settings adjustment.

**Wii U** (Cemu April 2026 build, included): Very good. Most of the library at 4K/60. Breath of the Wild, Mario Kart 8 — excellent.

**PS3** (RPCS3 0.0.40, included): Good, with per-game tuning needed on demanding titles. Enable Vulkan renderer. God of War 3, Demon's Souls, most first-party titles run well. Gran Turismo 5, some open-world games may need settings work. RPCS3 requires PS3 firmware (`PS3UPDAT.PUP`) — download it from Sony's official servers and install it through RPCS3's settings.

**Xbox 360** (Xenia, included): Mixed. Many titles work, many don't. Xenia compatibility is incomplete and less predictable than RPCS3.

**Nintendo Switch**: NOT included in the default Batocera image. Nintendo has successfully pursued legal action against Yuzu and Ryujinx; most active forks are also under pressure. A community script (`batocera-switch` on GitHub) can add a Switch emulator fork to an existing Batocera install. Doable, but requires manual steps and carries ongoing uncertainty as forks come and go.

**PS4**: shadPS4 is not in Batocera and is still too early for practical use.

---

## Getting ROMs onto the machine

Once Batocera is installed and networked, the share partition appears on your local network as a Samba share (\\batocera or via its IP address). Copy ROMs directly from another machine into the appropriate system folder (`roms/ps2/`, `roms/ps3/`, etc.). Batocera will scan and add them to the frontend automatically.

For PS3 specifically, games need to be in a specific format (either installed PKG or the folder structure from a disc dump). RPCS3's documentation covers this; the short version is that PS3 ISOs need to be extracted to a folder structure with `PARAM.SFO` at the right path.

---

## After setup: what to configure

- **Scrapers**: Batocera includes a scraper that downloads cover art and metadata for your library automatically. Run it from **Start → Scraper** once your ROMs are in place.
- **RetroAchievements**: built-in support if you want achievements on classic games.
- **Bezels and shaders**: Batocera ships with CRT shader presets and bezel overlays for older systems — worth enabling for NES/SNES/PS1 on a large display.
- **Per-system Vulkan**: for PS2, PS3, and Wii U specifically, confirm the Vulkan renderer is selected in the emulator settings (not OpenGL). The 780M's Vulkan driver is its strong suit.

---

**How much to trust this:** Assessment throughout. Hardware compatibility (780M, WiFi) is based on kernel version cross-referencing and community reports — not a direct test on this machine. The WiFi warning is the one real unknown; treat it as a "bring a backup" rather than a "this won't work." Everything else has strong community confirmation on similar AMD mini PC hardware running Batocera.

*Sources: [Batocera changelog](https://batocera.org/changelog); [Batocera supported controllers wiki](https://wiki.batocera.org/supported_controllers); [Batocera second drive wiki](https://wiki.batocera.org/store_games_on_a_second_usb_sata_drive); [minipclab.com Batocera guide](https://minipclab.com/blog/best-mini-pc-for-batocera); Intel Killer AX1675 Linux support thread*
