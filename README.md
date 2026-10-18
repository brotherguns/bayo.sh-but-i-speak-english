<p align="center">
  <img src="assets/icon0.png" width="160" alt="bayo.sh">
</p>

<h1 align="center">bayo.sh</h1>

<p align="center"><b>Animated video wallpapers for your PS5 home screen.</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/version-0.3.1-22b8ff?style=flat-square" alt="version">
  <img src="https://img.shields.io/badge/platform-PS5-22b8ff?style=flat-square" alt="platform">
  <img src="https://img.shields.io/badge/tested-13.60%20%2B%20etaHEN-22b8ff?style=flat-square" alt="tested">
  <img src="https://img.shields.io/badge/license-GPL--3.0-22b8ff?style=flat-square" alt="license">
</p>

---

## What it does

- **Your own video as the background** on the PS5 home screen, instead of the static image.
- **Per-game backgrounds**: each game can have its own video when you select it.
- **Effects**: dim, blur, gradients, vignette, haze and parallax.
- **Sound** for videos that include audio, with volume control.
- **Pause while playing**, so the game gets the full video decoder.
- **Normal background in Settings** and a **PlayStation Store-only** mode.
- **Random**: a different video on every boot.
- **Rest mode**: returns on its own when the console wakes up.
- **Home screen app** (Media) to control everything with your controller.
- **From your phone or PC**: open `http://PS5-IP:9050` to upload videos and control everything.
- **Controller light**: the DualSense LED follows the color of your background, or use Fixed, Rainbow, Breathing or Wave modes.
- **Any video**: if it isn't compatible (4K, HEVC, 60+ fps…), the **Convert and apply** button turns it into 1080p from your phone or PC.
- **Download from a link**: paste the link to an .mp4 and the PS5 downloads it to your wallpapers by itself.
- **Your own wallpaper sites**: add any pages or repos you like; they're saved on the PS5.
- **Background music** on the phone/PC page (original dark fantasy, can be turned off with a button).
- **First-use guide** inside the app.
- **10 languages**: español, English, Português, Français, Deutsch, Italiano, 日本語, 한국어, 中文, Русский.
- **etaHEN compatible** on firmware 13.60: it doesn't change the menu's permissions or read the kernel from the inside, and if something fails it stops by itself instead of taking the console down.

## Installation

You need a jailbroken PS5 and a payload loader.

1. Copy your `.mp4` videos to `/data/bayosh/video/` (via FTP), or to a `bayosh` folder on a USB drive.
2. Load `bayosh.elf`.
3. Open **bayo.sh** in Media, pick your video under **Wallpapers** and press **Use**.

You can also upload videos from your phone: open `http://PS5-IP:9050` in your browser (same Wi-Fi network).

**Recommended format:** `.mp4` H.264, 1920×1080, up to 60 fps. To convert any video on Windows, drag it onto `tools/convert_for_ps5.bat` (requires `ffmpeg.exe`).

**In-app controls:** L1 / R1 switch sections, ○ closes windows.

### Before using it

- Try it for a few days by launching it manually before adding it to your autoload.
- The first time, it creates an empty folder at `/system_ex/vsh_asset/wallpaper`. It does not modify any Sony files.
- If something goes wrong, restart without loading the exploit and the console boots normally.
- The log is saved at `/data/bayosh/bayosh.log`. Send it along if you report a problem.

## Building

On Linux or WSL, with:

- [ps5-payload-sdk](https://github.com/ps5-payload-dev/sdk) (tested with v0.44) and clang 18
- [OnionHEN](https://github.com/aydencharles/onionHEN) at tag `v0.9.0-beta2`

```bash
git clone https://github.com/aydencharles/onionHEN.git ~/onionHEN
git -C ~/onionHEN checkout v0.9.0-beta2
ONIONHEN=~/onionHEN PS5_PAYLOAD_SDK=~/ps5-payload-sdk tools/build.sh
```

The output ends up in `dist/bayosh.elf`. The script applies `patches/onionhen-detour.patch` (required for etaHEN), copies `src/` into the OnionHEN tree and builds the `bayosh` target.

### Structure

| File | What it is |
|---|---|
| `src/bayosh_daemon.cpp` | The payload: lists videos, mounts the folder, injects the module into the menu and watches over it |
| `src/bayosh_shellui.cpp` | The module that runs inside the PS5 menu and sets the video background |
| `src/bayosh_web.cpp` | Web server on port 9050 (the app and the phone page) |
| `src/bayosh_tile.cpp` | The app icon on the home screen |
| `src/bayosh_page.html` | The interface (PS5, PC and phone) |

### API (port 9050)

`/api/state`, `/api/select?i=`, `/api/on`, `/api/off`, `/api/dim?v=`, `/api/volume?v=`, `/api/blur?v=`, `/api/look?k=&v=`, `/api/set?k=&v=`, `/api/games`, `/api/gamebg?tid=&i=`, `/api/delete?i=`, `POST /api/upload?name=`, `/api/reset`, `/api/shutdown`, `/video?i=`, `/thumb?i=`, `/gameicon?tid=`.

## Credits

Made by **Josué** · [BayoTech](#).

bayo.sh is a derivative project. The video engine is based on **LiveShell** by ZiZc3 and uses the injector, libraries and groundwork of **[OnionHEN](https://github.com/aydencharles/onionHEN/)**. bayo.sh's changes are listed in [CHANGELOG.md](CHANGELOG.md).

## License

GPL-3.0. See [LICENSE](LICENSE). Provided without any warranty; use it at your own risk.
