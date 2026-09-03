---
title: "Batocera emulation appliance on the MINISFORUM UM790Pro"
type: topic-note
category: games
tags: [emulation, batocera, gaming, hardware, minisforum, setup, retro]
created: 2026-08-31
updated: 2026-09-03
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

FACT (direct observation, 2026-08-31): The MT7922 connected to a 2.4 GHz network (Buffalo-G-9630) and received a DHCP lease within seconds of `batocera-wifi start`. A 5 GHz network (Buffalo-A-9630) is also configured as the second entry.

**Correction (2026-09-01):** an earlier version of this note called Buffalo-A-9630 a 6 GHz band. It is 5 GHz — a scan from the machine reports it at 5320 MHz. The band matters when you are choosing which network to make primary, because range differs.

## Why your WiFi looks broken while an ethernet cable is plugged in

This is the single most confusing thing about networking a Batocera box, and it costs hours if you do not know it.

`/etc/connman/main.conf` ships with:

```
PreferredTechnologies=ethernet,wifi
SingleConnectedTechnology=true
```

connman keeps **exactly one** network technology connected, and prefers ethernet. So with a cable plugged in at boot, WiFi is not merely disconnected — it is never scanned. `connmanctl services` lists only the wired connection, and your configured networks do not appear at all. Everything looks like a driver or password fault and none of it is.

Two consequences worth planning around:

- **Checking for a stored lease proves nothing in this state.** An earlier version of this note suggested verifying WiFi from `/var/lib/connman/<service>/settings` while ethernet stayed connected. If connman never scanned, that file does not exist, and its absence tells you nothing about whether your password is right.
- **Connecting WiFi by hand disconnects ethernet**, same rule. If you do that over an SSH session running on the cable, you drop your own connection mid-command. It comes back on WiFi shortly afterwards, but the safe move is to edit `batocera.conf` and let the setting take effect at boot rather than switching the live interface you are sitting on.

The good news is the deployed case needs no intervention. `/etc/init.d/S08connman` regenerates the connman config from `batocera.conf` on every boot for each configured network. With no cable attached there is no ethernet to prefer, so WiFi is what connman brings up.

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

- **Scrapers**: Start → Scraper populates cover art and metadata. **Treat its output as
  unverified.** It matches on filename text, so a ROM whose filename does not closely resemble
  the real title gets a confident wrong answer — see the operating notes below.
- **RetroAchievements**: built-in support if you want achievements on classic games.
- **CRT shaders and bezels**: worth enabling for NES/SNES/PS1 on a large display — Batocera ships a good set of presets.
- **Vulkan renderer**: confirm this is selected for PS2, PS3, and Wii U specifically. The Radeon 780M's Vulkan driver is its strong suit; OpenGL performance on those systems is noticeably worse.

---

## What actually happened when we ran it

Everything above was written during the 2026-08-31 build. The following is FACT from operating the
machine since, and several items contradict what the documentation implies.

### The built-in scraper will confidently mislabel games

EmulationStation's scraper matches on **filename text**. A ROM named in the older GoodTools style
(`Megaman II (U) [!].nes`) does not resemble the real title, so the scraper guesses — and it guessed
**Totally Rad**, applying that game's name, description and box art. `Megaman III` became
**Magic John**. An audit of the whole library found 20 wrong entries: two ROM hacks scraped as the
real game, and eight US releases filed under Japanese or European titles (Streets of Rage 2 as
"Bare Knuckle II", Star Fox 64 as "Lylat Wars").

**The fix is naming, not scraping.** Identify each ROM by checksum against the published No-Intro
or Redump data, rename it to the official dump name, and the scraper's job becomes trivial.
Renaming a ROM orphans its save files, which are keyed to the filename, so those must be carried
across at the same time.

### Bluetooth pairings do not survive a reboot on their own

`/` is a 256 MB overlay whose upper layer lives in RAM. Bluetooth bonds are written there and are
lost on power-off unless the shutdown is clean. A small service that copies the bond data into
`/userdata` and restores it at boot fixes this permanently.

### A plugged-in ethernet cable stops WiFi being scanned at all

Covered above, and worth restating because it looks exactly like broken WiFi hardware: connman is
configured with `SingleConnectedTechnology=true` and ethernet preferred, so with a cable in, WiFi
is never even scanned. Diagnose WiFi with the cable **out**.

### Fast-forward has no controller binding by default

Batocera's config generator binds fast-forward to the **F12 key** and actively clears controller
bindings for it, so no button combination works out of the box. It has to be set explicitly in
`retroarchcustom.cfg`, and the correct button number must be read from the pad's own entry in
`es_input.cfg` — the same controller model appears there several times with completely different
numbering.

### Multi-disc games show up once per disc

EmulationStation scans `.m3u` **and** the disc files, and there is no setting to hide the members.
A four-disc game therefore occupies five tiles. The gamelist's `<hidden>true</hidden>` flag on each
member disc collapses it to one entry, with the playlist inheriting disc 1's artwork.

### BIOS files are exactly verifiable

`batocera-systems --filter <system>` prints every required BIOS file **with its MD5**. That turns
"find a BIOS" from a trust exercise into a checksum comparison. Note that it reports *every* known
revision as missing when you only need one — having the US PlayStation BIOS is sufficient even
though four other revisions still show as MISSING.

### Arcade systems work differently from everything else

Neo Geo uses FinalBurn Neo arcade romsets rather than cartridge dumps: zips of individual chip
dumps named by short set code (`mslug.zip`), plus `neogeo.zip` as a system BIOS. **Those sets are
version-locked to the emulator core and nothing in the metadata says so** — the only way to
establish compatibility is to load a game and watch for checksum errors.

### Never `pkill -9 -f emulationstation`

The process chain is `S31emulationstation` → `startx` → openbox → `emulationstation-standalone`
(a supervisor loop) → EmulationStation. A `-f` pattern match kills the supervisor too, so nothing
restarts it and the screen stays black. Use `batocera-es-swissknife --restart`.

**One check that matters:** `pgrep -f emulationstation` matches its own command line and will report
a dead frontend as running. Use `ps -C emulationstation`.

---

## PS3 and RPCS3 setup (September 2026)

This section covers what it actually took to get PS3 working. The general overview above says "install PS3UPDAT.PUP through RPCS3's settings," which omits the specifics that matter.

### Firmware install

FACT (2026-09-03): RPCS3 (Batocera 43.1 package) was installed. PS3 firmware 4.93 is installed and confirmed. The firmware version lives at:
```
/userdata/system/configs/rpcs3/dev_flash/vsh/etc/version.txt
```
Content: `release:04.9300:` — that is how `rpcs3Generator.py` confirms the firmware is present.

**How to install:** Download `PS3UPDAT.PUP` from Sony. The ROM rule treats this as a BIOS file — stage it in `/userdata/bios/rpcs3/`. Then run:
```bash
/usr/bin/rpcs3 --installfw /userdata/bios/rpcs3/PS3UPDAT.PUP
```

This works unattended IF you have the correct entries in `CurrentSettings.ini`. Without them, RPCS3 blocks on a "Welcome to RPCS3" dialog and the install hangs indefinitely.

### The dialog suppression INI: critical detail

FACT (2026-09-03): Batocera's `rpcs3Generator.py` reads and writes `[main_window]` section keys, not `[Interface]`. Any attempt to suppress dialogs via `[Interface]` is silently ignored.

The working config is at `/userdata/system/configs/rpcs3/GuiConfigs/CurrentSettings.ini`:
```ini
[Localization]
language=en

[Meta]
attachCommandLine=false

[main_window]
confirmationBoxExitGame=false
infoBoxEnabledInstallPUP=false
infoBoxEnabledWelcome=false
```

**Failed approaches** (hours wasted, do not try again):
- `[Interface]` / `show_welcome=false` — key does not exist in RPCS3's schema, silently ignored
- `[Interface]` / `ib_show_welcome=false` — same
- `QT_QPA_PLATFORM=offscreen` — Qt still processes the event loop and waits for the dialog
- `xdotool click` at estimated coordinates — blocked by fullscreen overlay window
- `xdotool key --window $WID Return` — Qt rejects XSendEvent (synthetic) events on XCB

The discovery that broke the deadlock: reading `rpcs3Generator.py` source at `/usr/lib/python3.12/site-packages/configgen/generators/rpcs3/rpcs3Generator.py` to find the actual key names the generator sets.

### PS3 game format

RPCS3 on Batocera expects PS3 games either:
- As folder dumps with PARAM.SFO at the correct path (extracted from `.pkg` or RAR archives)
- As disc images in a format RPCS3 supports

MiNERVA distributes games as multi-part RAR archives (e.g., `Metal_Gear_Solid_4_Guns_of_The_Patriots_BLUS30109.rar`). Extract the RAR to get the game folder, then place it under `/userdata/roms/ps3/`.

---

## Steam Big Picture: fixes applied (September 2026)

### Fix 1: ES immediately reclaims foreground

**Problem:** Launching Steam Big Picture from ES dropped back to ES immediately.

**Root cause:** the original `batocera-steam` script's no-game path launched Big Picture in the background, slept 3 seconds, and exited. ES detects the emulator process ending and reclaims foreground.

**Fix:** Launch Steam with the `-bigpicture` flag directly (not via `steam://open/bigpicture` URL), then block until Steam exits:

```bash
# OLD: URL approach left a 48x48 stub window; sleep-and-exit gave ES foreground back
flatpak run com.valvesoftware.Steam steam://open/bigpicture &
sleep 3

# NEW: direct flag, block until Steam actually quits
flatpak run com.valvesoftware.Steam -bigpicture &
while steam_is_running; do sleep 3; done
```

`steam_is_running` checks for `flatpak-bwrap.*-- steam` — the bwrap wrapper — not the generic Steam name that also matches helper processes.

FACT (2026-09-03, operator-confirmed): fix deployed in `/boot/boot-custom.sh` (persistent across reboots) and `/usr/bin/batocera-steam` (live). Big Picture now shows fullscreen at 1920x1080.

### Fix 2: Xbox controller not working in Big Picture

**Problem:** Xbox Series X/S controller (045E:0B22) connected and showed in kernel (`/proc/bus/input/devices`) but could not navigate Big Picture menus.

**Root causes found** (both needed to be fixed):

1. **Stuck preview config**: During Plague Inc. controller setup, Steam's controller config editor was left in "preview" mode for app 443510, loading a community workshop config (`steamapps/workshop/content/241100/936992996/767148481031564973_legacy.bin`) on every Big Picture focus cycle. This config overrides navigation bindings while active. Fixed by deleting the workshop file.

2. **Batocera Control Center stealing X11 focus**: The Batocera Control Center (`python3 /usr/bin/batocera-controlcenter-app --hidden`, PID persistent across sessions) has two X11 windows. Openbox is configured with `focusNew=yes` — any new window gets focus automatically. When the Control Center creates or raises windows, it steals focus from the Steam Big Picture window. Steam detects the focus change and switches to the Desktop controller context (App 413080), which loads `empty.vdf` (no bindings). The controller goes dead for several seconds, then Big Picture reclaims focus, and the cycle repeats every 6–9 seconds.

**Fixes applied (2026-09-03):**

- Deleted the stuck preview file: `steamapps/workshop/content/241100/936992996/767148481031564973_legacy.bin`
- Updated `batocera-steam` to kill `batocera-controlcenter` before launching Steam, and added a focus-keeper background loop:

```bash
# Kill Control Center so it cannot steal X11 focus
pkill -f "batocera-controlcenter" 2>/dev/null || true
sleep 1

# ... launch -bigpicture ...

# Focus keeper runs in background; reclaims Steam window whenever focus is lost
(
    sleep 8
    WID=""
    while [ -z "$WID" ] && steam_is_running; do
        WID=$(DISPLAY="$DISP" xdotool search --name "Steam Big Picture Mode" 2>/dev/null | tail -1)
        [ -z "$WID" ] && sleep 2
    done
    while steam_is_running; do
        FOCUS=$(DISPLAY="$DISP" xdotool getwindowfocus 2>/dev/null)
        if [ -n "$WID" ] && [ "$FOCUS" != "$WID" ]; then
            DISPLAY="$DISP" xdotool windowactivate --sync "$WID" 2>/dev/null
        fi
        sleep 2
    done
) &
FOCUS_PID=$!
while steam_is_running; do sleep 3; done
kill "$FOCUS_PID" 2>/dev/null || true
```

**How to diagnose this in future:** `logs/controller_ui.txt` shows `OnFocusWindowChanged to window type: k_nGameIDControllerConfigs_Desktop, AppID 413080` when focus is stolen. `logs/controller.txt` shows `Using preview config for appid: N` when a stuck preview is active.

**The Plague Inc. workshop config** (`configset_45e-28e-1ba49c0.vdf`, app 246620 → workshop 1512491847) is separate and untouched — it only affects controller mappings when playing Plague Inc., not Big Picture navigation.

FACT (2026-09-03): fix confirmed working. After applying both changes, the controller navigated Big Picture, opened the on-screen keyboard, and opened game controller settings — all without the focus cycling recurring.

**Why the Control Center steals focus:** `emulationstation-standalone` starts `batocera-controlcenter hidden &` on every ES boot. Despite the `--hidden` flag, it creates two X11 windows. Openbox's `focusNew=yes` setting gives any newly-mapped window automatic focus, pulling focus from Steam. The Control Center does not restart during a Steam session, so killing it once at Steam launch is sufficient.

**The focus keeper loop** in the updated script is a backup — if anything else creates a window and steals focus, the loop reclaims it every 2 seconds using `xdotool search --class "steam"`. The fix is not fragile: killing the Control Center is the primary solution, and the keeper is belt-and-suspenders.

Assessment: `focusNew=no` in the Openbox config (`/etc/openbox/rc.xml`) would fix the root cause without needing the Control Center kill or focus keeper, but that would change all of Batocera's normal window focus behavior — a bigger change to test.

---

## PS3 ROM download via MiNERVA and qbittorrent-nox (September 2026)

### ROM source: MiNERVA Archive

FACT (2026-09-03): Myrient closed around March 2026 after unsustainable hosting costs (~\$6,000/month). Its successor is **MiNERVA Archive** at `minerva-archive.org`. The collection is the same Myrient data — 385 TB — redistributed via BitTorrent only. No direct HTTP downloads.

MiNERVA's per-game pages show:
- The file's `so_id` (the file index within the torrent)
- A magnet link and `.torrent` download for the entire collection
- File size

For PS3: the torrent contains PS3_ALVRO_PART_1 through PART_11 (alphabetical) plus PS3_PSN_1/2.

**What failed:**
- archive.org PS3_ALVRO collections: HTTP 401 — requires account authentication. Direct `wget`/`curl` produces a 0-byte file with no error.
- MiNERVA has no direct HTTP endpoint; all links resolve to torrent/magnet only.

### Torrent client: qbittorrent-nox static binary

Batocera has no torrent client and aria2c is not in the package manager. The solution is a static `qbittorrent-nox` binary from GitHub:

```bash
# One-time download (already done — binary is at /userdata/system/qbittorrent-nox)
URL=$(curl -sL "https://api.github.com/repos/userdocs/qbittorrent-nox-static/releases/latest" | \
  python3 -c "import json,sys; [print(a['browser_download_url']) for a in json.load(sys.stdin)['assets'] if a['name']=='x86_64-qbittorrent-nox']")
wget -O /userdata/system/qbittorrent-nox "$URL"
chmod +x /userdata/system/qbittorrent-nox
```

FACT (2026-09-03): qbittorrent-nox v5.2.3 (x86_64 musl static) runs on Batocera 43.1. Binary is at `/userdata/system/qbittorrent-nox` (persistent). Auto-starts via `boot-custom.sh` Fix 3 on every boot.

Web UI: `http://192.168.11.65:8089`, user `admin`, password changes each session — read from `/tmp/qbt.log`.

### File selection: fastresume edit

qBittorrent 5.x broke the `/api/v2/torrents/filePrio` endpoint (the TorrentsController methods no longer exist). The workaround is to edit the `.fastresume` bencoded file directly while qbt is stopped:

```bash
pkill qbittorrent-nox
# Edit fastresume with Python — key is file_priority (underscore, not dash)
python3 /tmp/fix_fastresume.py   # see script below
# Restart
/userdata/system/qbittorrent-nox --confirm-legal-notice --webui-port=8089 --profile=/userdata/system/qbt-config >>/tmp/qbt.log 2>&1 &
```

The fastresume file is at:
```
/userdata/system/qbt-config/qBittorrent/data/BT_backup/<hash>.fastresume
```

**Failed qbt 5.x API endpoints** (do not try again):
- `POST /api/v2/torrents/filePrio` — returns HTTP 200 with body "Endpoint does not exist"
- `POST /api/v2/torrents/pause` / `resume` — HTTP 200 but no-op (methods removed)
- `POST /api/v2/torrents/stop` / `start` — these DO work in v5.x
- Cookie-based auth: `MozillaCookieJar.load()` fails on qbt's cookie file format; use `opener.open()` with the session opener that did the login, not the cookie file

**Working in qbt 5.x:**
- `GET /api/v2/torrents/info?hashes=<hash>` — returns state, size, progress, speed, peers
- `GET /api/v2/torrents/files?hash=<hash>&indexes=N` — returns file list with index and priority
- `GET /api/v2/app/preferences` — returns DHT/PEX/port settings
- `GET /api/v2/torrents/trackers?hash=<hash>` — tracker status and peer counts
- `POST /api/v2/torrents/add` with multipart `torrents=@<file>` — add a torrent

### MGS4 status

FACT (2026-09-03): Metal Gear Solid 4 (BLUS30109, US) is downloading.
- File index in MiNERVA torrent: 1226 (0-indexed)
- File size: 30,608 MB (~29 GB as a RAR archive)
- MiNERVA torrent hash: `84a87977a30f1c22f09f68ba69d57c489f773adf`
- Torrent: `minerva_myrient` (the full MiNERVA collection torrent)
- As of 2026-09-03: downloading at ~23 KB/s, 19 peers, 1 MB completed

Speed will increase as more peers are found. Let it run in the background. qbittorrent-nox will resume automatically after each reboot.

### Other PS3 games to download (next)

When MGS4 completes, queue these (all from the same MiNERVA torrent, just change the file index):
- Uncharted 2 (BCUS98123): file index 3246 (PART_11)
- Uncharted 1 (BCUS98103): file index 3242 (PART_11)
- God of War 3 (BCUS98111): file index approx. PART_4
- Heavy Rain (BCUS98246): file index approx. PART_4
- Gran Turismo 5 (BCUS98114): file index approx. PART_4

Look up exact file indices via the MiNERVA website or by checking the torrent's file list via the qbt API.

---

## Where to go next

This note is the build. Three companions cover what comes after it:

- **[What to actually put on it](topics/games/batocera-top-games)** — the ranked list of what is
  worth playing per system, plus a section separating that wish list from the 169 titles actually
  installed and verified here.
- **Guides for every game on the machine** — 163 walkthroughs across 16 systems, each covering
  controls, the mechanics that matter, and what changes under emulation. The per-system indexes are
  the entry points:
  [NES](topics/games/nes/) · [SNES](topics/games/snes/) · [N64](topics/games/n64/) ·
  [Game Boy](topics/games/gb/) · [GBC](topics/games/gbc/) · [GBA](topics/games/gba/) ·
  [Mega Drive](topics/games/megadrive/) · [Master System](topics/games/mastersystem/) ·
  [Game Gear](topics/games/gamegear/) · [PC Engine](topics/games/pcengine/) ·
  [Saturn](topics/games/saturn/) · [Dreamcast](topics/games/dreamcast/) ·
  [PlayStation](topics/games/psx/) · [PS2](topics/games/ps2/) · [PSP](topics/games/psp/) ·
  [Neo Geo](topics/games/neogeo/)
- **[The games Batocera ships with](topics/games/bundled-homebrew)** — the freeware titles already
  on a fresh install.

**If you are choosing between emulating and modifying real hardware**, the
[console jailbreak survey](topics/technology/console-jailbreak-landscape) covers the other route:
which consoles are worth opening up, which are hopeless, and why Nintendo hardware is always the
most exploitable. A mini PC running Batocera and a modded handheld solve overlapping problems, and
[homebrew on portable devices](topics/games/portable-homebrew/) is where the two meet.

---

**How much to trust this:** the install, storage, networking and controller sections are FACT from the 2026-08-31 build. Everything under *What actually happened when we ran it* is FACT from operating the machine since, and several items there correct what the earlier sections and the official documentation imply.

Per-system **performance** ratings remain Assessment. 169 titles across 16 systems are installed and every ROM is checksum-verified, but *installed and verified* is not the same as *performance-tested*, and it would be easy to read the two as one thing.

What has actually been observed: the ten Neo Geo games were **launch-tested individually** (they load and the core initialises — not a frame-rate measurement), and a handful of NES titles have been played through by the operator. Everything else is installed and launches but has not been sat with long enough to confirm sustained full speed. PS3, Xbox 360, Wii U, 3DS and Switch have nothing installed at all, so those ratings remain community reports on similar hardware.

*Sources: [Batocera changelog](https://batocera.org/changelog); [Batocera supported controllers wiki](https://wiki.batocera.org/supported_controllers); [Batocera second drive wiki](https://wiki.batocera.org/store_games_on_a_second_usb_sata_drive); [Batocera batocera.conf wiki](https://wiki.batocera.org/batocera.conf); direct build log 2026-08-31*
