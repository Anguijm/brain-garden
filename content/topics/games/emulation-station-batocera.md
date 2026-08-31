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

The UM790Pro (Ryzen 9 7940HS, Radeon 780M, 64 GB DDR5) makes a capable emulation appliance. This note covers the full build: what hardware actually shipped in the box, what the install process looks like in practice, and how to configure WiFi remotely once the machine is running.

Assessment: The hardware is well-matched to Batocera 43.1 and the WiFi works out of the box — the chip is a MediaTek MT7922, not the Intel part some specs pages list, and the `mt7921e` driver is bundled in Batocera's kernel with no extra firmware steps needed.

---

## What you need before you start

- A USB drive, 16 GB or larger, to boot the live environment (this becomes the installer, not the permanent OS home)
- A direct ethernet connection to another machine for the install — the USB drive is only needed to get the machine to a shell; the actual OS image streams over the wire
- An Xbox controller (Series X/S or One) — pairs over Bluetooth or USB, recognised immediately
- A router or laptop that can share internet over ethernet if the target machine needs downloads during setup

---

## The WiFi chip: MediaTek MT7922, not Intel

Product pages for the UM790Pro list an Intel Killer AX1675, but the actual hardware is a **MediaTek MT7922** (Wi-Fi 6E, confirmed via `lspci` on a live Batocera 43.1 system):

```
02:00.0 Network controller: MEDIATEK Corp. MT7922 802.11ax PCI Express Wireless Network Adapter
```

FACT (direct observation, 2026-08-31): Batocera 43.1 (kernel 6.18.16) ships the `mt7921e` driver, which handles the MT7922 without any additional firmware files. The interface (`wlan0`) appeared and scanned on first boot.

This matters because a lot of forum advice about WiFi issues on the UM790Pro assumes the Intel part and will send you chasing `iwlwifi` firmware files that do nothing. Skip that entirely.

---

## How Batocera works as an appliance

Batocera boots straight into EmulationStation — a controller-driven frontend — with no desktop OS. You navigate your library with the Xbox controller, launch games, and return to the frontend when done. No keyboard or mouse required after initial setup. Everything can be managed from the controller or via SSH from another machine on the network.

FACT (from Batocera 43.1 documentation): Batocera keeps your ROMs, saves, and configuration on a `share` partition, separate from the OS. A reinstall or upgrade does not touch the game library.

---

## Storage layout after install

FACT (direct observation, 2026-08-31): After writing the Batocera 43.1 x86_64 image to a 1 TB NVMe and booting, the partition table looked like this:

- `/dev/nvme0n1p1` — 10 GB, FAT32 — boot partition (EFI, kernel, `batocera-boot.conf`)
- `/dev/nvme0n1p2` — 928 GB, ext4 — share partition, auto-expanded from the image's stub on first boot

The `autoresize=true` setting in `batocera-boot.conf` handles the expansion automatically. The share partition mounts at `/userdata` and holds ROMs, saves, BIOS files, themes, and system configuration.

The UM790Pro has two M.2 2280 PCIe Gen4 slots. The second slot can be pointed at by setting `sharedevice=<UUID>` in `batocera-boot.conf` if you want a dedicated ROM drive.

---

## Install: streaming from another machine over direct ethernet

The standard install path (GUI installer inside the live environment) works, but if your USB drive is unreliable or you want to avoid writing the image to USB at all, a faster route is to stream the image directly from a laptop over a direct ethernet cable.

**On the laptop:**

1. Download the Batocera x86_64 image: `batocera-x86_64-43.1-20260529.img.gz` from batocera.org (~4.1 GB compressed, ~11 GB uncompressed)
2. Set a static IP on the ethernet interface:
   ```bash
   nmcli connection add type ethernet ifname enp3s0 con-name batocera-link \
     ipv4.method manual ipv4.addresses 10.0.0.1/24
   nmcli connection up batocera-link
   ```
   Or, to get DHCP sharing and internet pass-through in one step:
   ```bash
   nmcli connection modify batocera-link ipv4.method shared
   nmcli connection up batocera-link
   ```
   The `shared` method starts NetworkManager's built-in DHCP server and NAT on that interface, so the UM790Pro gets an IP automatically and can reach the internet through the laptop's WiFi.

3. Serve the image over HTTP:
   ```bash
   python3 -m http.server 8080 --bind 10.42.0.1 --directory /tmp
   ```
   (Bind to the ethernet interface address to keep it off your WiFi.)

**On the UM790Pro** (boot a live Batocera USB and open a terminal):

```bash
curl -L http://10.42.0.1:8080/batocera-x86_64-43.1-20260529.img.gz \
  | gunzip | dd of=/dev/nvme0n1 bs=4M status=progress
```

FACT (direct observation, 2026-08-31): This wrote 11 GB to NVMe at ~314 MB/s over direct gigabit ethernet. Elapsed time under 40 seconds. The GPT backup table mismatch warning from `fdisk` after the write is expected and harmless — Batocera corrects it on first boot.

Pull the USB, reboot, and the machine comes up from the internal drive.

---

## Configuring WiFi after install

WiFi configuration goes in `/userdata/system/batocera.conf`. Batocera supports up to three networks natively. Edit the file over SSH or from the EmulationStation menu:

```
wifi.country=JP
wifi.enabled=1
wifi.ssid=YourPreferredSSID
wifi.key=YourPassword
wifi2.ssid=YourFallbackSSID
wifi2.key=YourPassword
```

After editing, apply with:

```bash
batocera-wifi start
```

This restarts connman and picks up the new config. The machine will auto-connect to the first network it finds from the list.

FACT (direct observation, 2026-08-31): The MT7922 connected to a 2.4 GHz network (Buffalo-G-9630) and received a DHCP lease within seconds of `batocera-wifi start`. The 6 GHz band (Buffalo-A-9630) is also configured and will be used as the preferred network going forward.

If you are configuring remotely (over ethernet) and want to verify WiFi without disconnecting ethernet, check `/var/lib/connman/<service>/settings` — a `IPv4.DHCP.LastAddress` line there confirms the network connected at least once and the password is correct.

---

## Xbox controller

Assessment (from Batocera wiki): Xbox Series X/S and Xbox One controllers pair over standard Bluetooth LE. Hold the Bluetooth button until it flashes rapidly, then from EmulationStation go to **Start → Controller & Bluetooth Settings → Pair a Bluetooth device**. The `xpad` kernel driver handles everything; no additional software needed.

Update controller firmware via Windows or an Xbox console before bringing it to the appliance. Outdated firmware is the most common cause of pairing failures.

---

## What each system runs and how well

Assessment, based on UM790Pro hardware class and Batocera 43.1 emulator versions:

**Everything through sixth generation** (NES, SNES, N64, PS1, Saturn, Dreamcast, GBA, DS): full speed, zero configuration. 4K upscaling available and generally solid.

**PS2** (PCSX2): Excellent. The 7940HS handles the full library at full speed, most games upscalable. Use the Vulkan renderer.

**GameCube / Wii** (Dolphin): Excellent. Full speed at 4K on essentially everything.

**PSP** (PPSSPP): Flawless.

**3DS** (Lime3DS / Citra fork): Good. Most titles run well; a handful of demanding ones need settings work.

**Wii U** (Cemu): Very good. Breath of the Wild, Mario Kart 8 — excellent at 4K/60.

**PS3** (RPCS3): Good, with per-game tuning on demanding titles. Use Vulkan. First-party games and most third-party run well. Requires the PS3 firmware file (`PS3UPDAT.PUP`) — download from Sony's servers and install through RPCS3's settings.

**Xbox 360** (Xenia): Mixed. Many titles work; many don't. Less predictable than RPCS3.

**Nintendo Switch**: Not in the default image. Legal pressure on Yuzu and Ryujinx has pushed the forks into grey territory. A community installer exists but requires manual steps and carries ongoing uncertainty.

**PS4**: shadPS4 is not in Batocera and too immature for practical use.

---

## Getting ROMs onto the machine

Once networked, the share partition is a Samba share. Copy from another machine on the same network:

- Windows: `\\batocera\share` or `\\<IP>\share`
- macOS/Linux: `smb://batocera/share` or `smb://<IP>/share`

Drop ROMs into the appropriate system folder (`roms/ps2/`, `roms/ps3/`, etc.). Batocera scans and adds them to the frontend automatically.

For PS3: games need to be in folder format with `PARAM.SFO` at the right path — RPCS3's documentation covers the exact structure. Raw ISOs need to be extracted first.

---

## After setup: what to configure

- **Scrapers**: Start → Scraper — downloads cover art and metadata for the whole library automatically.
- **RetroAchievements**: built-in support if you want achievements on classic games.
- **CRT shaders and bezels**: worth enabling for NES/SNES/PS1 on a large display — Batocera ships a good set of presets.
- **Vulkan renderer**: confirm this is selected for PS2, PS3, and Wii U specifically. The Radeon 780M's Vulkan driver is its strong suit; OpenGL performance on those systems is noticeably worse.

---

**How much to trust this:** FACT claims above are from direct observation during the 2026-08-31 build. Emulation performance is Assessment based on hardware class and community reports on similar AMD mini PC hardware with Batocera — not exhaustive per-game testing.

*Sources: [Batocera changelog](https://batocera.org/changelog); [Batocera supported controllers wiki](https://wiki.batocera.org/supported_controllers); [Batocera second drive wiki](https://wiki.batocera.org/store_games_on_a_second_usb_sata_drive); [Batocera batocera.conf wiki](https://wiki.batocera.org/batocera.conf); direct build log 2026-08-31*
