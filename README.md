# hejran7 PS4 Host Server

A local network server for hosting PS4 jailbreak exploits (PS-Phive / Lapse) on **firmware 7.00 – 9.60**.  
Built with Python stdlib only. Inspired by Leeful/Sinajet.

---

## Features

- **HTTP Server** — multi-threaded, serves files from the `host/` folder on port 80
- **DNS Server** — redirects `lapse.tow` and PlayStation domains to your PC, blocks telemetry
- **AppCache Manifest** — auto-generates `offline.cache` on every start, fingerprinted by file metadata so the PS4 only re-downloads when files actually change
- **PS4 User's Guide method** — strips `/document/en/ps4/` prefix so the exploit loads via `manuals.playstation.net`
- **Correct MIME types** — `.bin`, `.mjs`, `.cache`, `.manifest` all served correctly
- **Esc to stop** — clean shutdown from the terminal

---

## Requirements

- Windows 10 / 11
- Python 3.9+ **or** use the prebuilt `hejran7 PS4 host server.exe`
- Must be run as **Administrator** (required for port 80 and DNS port 53)
- `fakedns.py` must be in the same folder as `server.py` / the exe

---

## Folder Structure

```
psphive-lapse-2-main/
├── host/
│   ├── index.html
│   ├── offline.cache          ← auto-generated on server start
│   └── files/
│       ├── bundle.js
│       ├── style.css
│       ├── img/
│       ├── js/
│       └── *.bin
├── server.py
├── fakedns.py
├── server.bat
└── hejran7 PS4 host server.exe
```

---

## Usage

### Option 1 — EXE (easiest)
1. Right-click `hejran7 PS4 host server.exe` → **Run as administrator**

### Option 2 — Python
1. Right-click `server.bat` → **Run as administrator**  
   or run `py server.py` in an admin terminal

---

## Connecting Your PS4

### Method A — Direct IP (no DNS change needed)
1. On your PS4 browser, type your PC's IP shown in the terminal (e.g. `192.168.1.192`)

### Method B — User's Guide / DNS
1. On PS4 go to **Settings → Network → Set Up Internet Connection**
2. Set **Primary DNS** to your PC's IP (e.g. `192.168.1.192`)
3. Open the PS4 browser and go to `lapse.tow`  
   — or use **User's Guide** from the PS4 home screen

---

## How offline.cache Works

On every server start, the server:
1. Walks the `host/` folder recursively
2. Skips non-essential files (`.py`, `.exe`, `.bat`, `.md`, etc.)
3. Reads each file's **size and modification time** (no full file read — instant)
4. Hashes all metadata into a single MD5 fingerprint
5. Writes `offline.cache` with that fingerprint as a comment

If no files changed since last run, the fingerprint stays the same and the PS4 uses its cached copy instantly. If any file was added, removed, or modified, the fingerprint changes and the PS4 re-downloads everything automatically.

---

## Credits

- **PS-Phive exploit** by [Leeful](https://github.com/Leeful) / Sinajet
- **PSFree WebKit exploit** (CVE-2022-22620)
- **fakedns.py** by [Crypt0s](https://github.com/Crypt0s/FakeDns)
- **Server** by hejran7
