# 🦇 CreepyAmp

**A Winamp-inspired audio & video player that lives in a single HTML file.**
Drop your tracks in, get a 10-band equalizer, a glowing LED spectrum analyzer, and thousands of MilkDrop visualizations. No install, no build step, no server, no upload.

Version 1.0:
<img width="1440" height="864" alt="image" src="https://github.com/user-attachments/assets/b1489e36-f14c-4e9e-9a08-72ca28d22f1c" />

Version 2.0 width 3D stereo, and new look:
<img width="1485" height="872" alt="image" src="https://github.com/user-attachments/assets/1fc80c4d-6e5b-4605-bed5-9d515532995e" />


---

## Table of contents

- [Features](#features)
- [Quick start](#quick-start)
- [Supported formats](#supported-formats)
- [Usage](#usage)
  - [Playlist](#playlist)
  - [Transport & playback modes](#transport--playback-modes)
  - [Track info panel](#track-info-panel)
  - [Equalizer](#equalizer)
  - [Visualizations](#visualizations)
  - [Fullscreen mode](#fullscreen-mode)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Saved settings](#saved-settings)
- [How it works](#how-it-works)
- [Browser requirements](#browser-requirements)
- [Privacy](#privacy)
- [Troubleshooting](#troubleshooting)
- [Dependencies](#dependencies)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Single-file app**: everything (HTML, CSS, JavaScript, icon) is in `CreepyAmp.html`.
- **Audio and video playback** from local files, with a shared audio pipeline for both.
- **Drag & drop playlist**: drop files anywhere in the window, reorder tracks by dragging them.
- **Built-in tag reader** (no library): ID3v1, ID3v2.2/2.3/2.4, FLAC Vorbis comments and WAV `INFO` chunks, plus embedded **cover art**.
- **Track info panel**: title, artist, album, year, track number, duration, format, bitrate, file size, genre, sample rate / bit depth / channels (video shows resolution).
- **10-band graphic equalizer** with preamp, 10 presets, a live response curve and mouse / wheel / keyboard control.
- **LED spectrum analyzer**: a full-width retro LED bar display with peak hold.
- **MilkDrop visualizations** through [Butterchurn](https://github.com/jberg/butterchurn), with automatic preset switching, previous / next / random controls.
- **Fullscreen mode** with auto-hiding controls, for the visualization or for video.
- **Shuffle** and **repeat** (off / all / one), with Winamp-style "previous" behavior.
- **Resizable playlist panel**, responsive layout, safe-area support for phones.
- **Persistent settings**: volume, EQ, shuffle, repeat, visualizer mode and last preset are remembered.

<img width="1438" height="866" alt="image" src="https://github.com/user-attachments/assets/b09de873-7964-4fac-9210-0584479f324e" />

## Quick start

1. Download `CreepyAmp.html`.
2. Open it in a modern browser (double-click, or drag it into a browser window).
3. Drag your music or videos into the window, or click **+ ADD** / the eject button.
4. Press play.

That's it. You can also host the file on any static web server (GitHub Pages, Netlify, etc.).

```bash
# Optional: serve it locally
python3 -m http.server 8000
# then open http://localhost:8000/CreepyAmp.html
```

## Supported formats

Creepy Amp uses the browser's native media decoders, so actual playback support depends on your browser and OS.

| Type  | Recognized extensions |
|-------|-----------------------|
| Audio | `mp3` `wav` `ogg` `oga` `flac` `m4a` `aac` `opus` `weba` |
| Video | `mp4` `m4v` `mov` `mkv` `webm` `ogv` `avi` |

**Tag reading** is implemented for:

| Format | Tags read |
|--------|-----------|
| MP3 | ID3v2.2 / 2.3 / 2.4, ID3v1, MPEG frame info (codec, sample rate, channels, bitrate, VBR detection), cover art |
| FLAC (including FLAC with an ID3 header) | STREAMINFO, Vorbis comments, embedded picture |
| WAV | `fmt` chunk (sample rate, bit depth, channels, bitrate), `LIST/INFO` tags |
| Other audio / video | Falls back to the file name; the format is shown from the extension |

## Usage

### Playlist

- **Add files**: drag & drop them anywhere in the window, or use **+ ADD** or the eject-style button in the transport bar. Multiple selection is supported. Non-media files are ignored.
- **Play a track**: click it in the list.
- **Reorder**: drag a track up or down; a marker shows the drop position.
- **Remove**: click the **×** on a track, or **CLEAR** to empty the list.
- **Resize the panel**: drag the thin handle between the player and the playlist. Double-click the handle to reset the width.
- The header shows the number of tracks and the total duration. Durations are probed in the background.

### Transport & playback modes

| Control | Action |
|---------|--------|
| ◀ | Previous track. If more than 3 seconds have elapsed, restarts the current track instead (like Winamp). |
| ▶ / ❚❚ / ■ | Play / Pause / Stop |
| ▶ (next) | Next track |
| Seek bar | Jump within the track |
| VOL | Volume |
| Shuffle | Random order without repeating a track until all have been played |
| Repeat | Cycles **all** → **one** → **off** |

When repeat is off, playback stops at the end of the list.

### Track info panel

Shows cover art (or the Creepy Amp logo as a placeholder), title, artist and album, followed by year, track number, duration, format, bitrate, size, genre and signal details. For video files, the genre field is replaced by the video resolution.

### Equalizer

- **ON / OFF** toggles the equalizer without losing your settings.
- **Bands**: 60, 170, 310, 600 Hz, 1, 3, 6, 12, 14 and 16 kHz, plus a **PRE** (preamp) slider. Range is ±12 dB in 0.5 dB steps.
- **Presets**: Flat, Rock, Pop, Dance, Techno, Classical, Bass, Treble, Vocal, Small speakers. Moving any slider switches the selector to *Custom*.
- **RESET** sets every band and the preamp back to 0 dB.
- The **▾ / ▸** button folds the equalizer to save space.
- A smooth **frequency response curve** is drawn behind the sliders.

Slider controls:

| Input | Effect |
|-------|--------|
| Click / drag | Set the gain |
| Double-click | Reset the band to 0 dB |
| Mouse wheel | ±0.5 dB |
| ← ↓ / → ↑ | ±0.5 dB |
| Page Up / Page Down | ±3 dB |
| Home | Reset to 0 dB |

The equalizer is applied *before* the analyzer and the MilkDrop visualization, so the visuals react to what you actually hear.

### Visualizations

The **EFFECTS** panel switches between two modes:

- **LED**: a retro spectrum analyzer with green / amber / red segments and falling peak markers. The number of bars adapts to the window width.
- **MILK**: MilkDrop-style visualizations rendered with WebGL2 via Butterchurn. The preset packs are loaded on demand from a CDN the first time you enable this mode.

In MILK mode:

- **◀ / ▶** go to the previous / next preset, **RND** picks a random one.
- **AUTO** automatically changes preset every 25 seconds while music is playing (with a 4-second blend).
- The current preset name (`index/total  name`) fades in briefly at each change.
- The song title is animated on screen when a track starts.

### Fullscreen mode

Click the fullscreen button in the title bar, press **F**, or double-click the stage. Only the visualization (audio) or the video is shown, with an overlay containing the title, time, seek bar, previous / play / next buttons and a preset button. The overlay hides itself after 2.5 seconds of inactivity. Click the stage to play / pause. If the browser refuses real fullscreen (for example inside an iframe), a full-window fallback is used instead.

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `F` | Toggle fullscreen |
| `S` | Toggle shuffle |
| `T` | Cycle repeat mode |
| `M` | Switch LED / MILK visualization |
| `N` | Next preset (MILK mode) |
| `B` | Previous preset (MILK mode) |
| `R` | Random preset (MILK mode) |
| `Esc` | Exit fullscreen |

Shortcuts are ignored while a modifier (`Ctrl`, `Alt`, `Cmd`) is held.

Version 2.0 with 2 stereo 3D Expander!
<img width="1174" height="872" alt="image" src="https://github.com/user-attachments/assets/f870bade-9abd-4f64-992f-27bdc492a60c" />

## Saved settings

Settings are stored in your browser's `localStorage` on your device:

| Key | Content |
|-----|---------|
| `creepyamp.settings` | Volume, EQ gains and preamp, EQ preset, EQ on/off, EQ folded state, shuffle, repeat mode, visualizer mode, AUTO on/off, last MilkDrop preset |
| `sideW` | Playlist panel width |

Your playlist itself is **not** saved: files are opened as temporary local object URLs and must be re-added in the next session. To reset everything, clear the site data for the page in your browser.

## How it works

```
 <audio> ─┐
          ├─► bus ─► preamp ─► 10 biquad filters ─► eqOut ─┬─► speakers
 <video> ─┘                                                ├─► AnalyserNode ─► LED spectrum
                                                           └─► Butterchurn (MilkDrop)
```

- All playback goes through a single Web Audio graph. Audio and video elements are wrapped once with `createMediaElementSource`.
- The equalizer uses a low-shelf filter (60 Hz), eight peaking filters (Q = 1.1) and a high-shelf filter (16 kHz).
- The analyzer uses an FFT size of 2048 and maps the spectrum on a logarithmic scale to the visible bars.
- Tags are parsed by hand from the first bytes of the file (and the last 128 bytes for ID3v1). Large files are never fully loaded into memory.
- Durations and tags are read in the background, one file at a time, so adding a large batch of files does not freeze the UI.

## Browser requirements

- A modern desktop or mobile browser (Chrome, Edge, Firefox, Safari) with Web Audio support.
- **WebGL2** is required for MilkDrop mode only. If it is unavailable, the LED spectrum still works and a message is displayed.
- An internet connection is needed the first time MilkDrop mode is used, to fetch the visualization scripts (see [Dependencies](#dependencies)). Everything else works offline.

## Privacy

Your files never leave your device. They are read locally in the browser and are not uploaded anywhere. The only network requests made by the app are the optional downloads of the Butterchurn scripts from a public CDN when MILK mode is enabled.

## Troubleshooting

**The track plays but I hear nothing.**
Browsers keep the audio engine suspended until you interact with the page (this often happens after a drag & drop). Click anywhere, or press play; Creepy Amp shows a hint in the title bar when this happens.

**Autoplay is blocked.**
Same cause. Click ▶ once.

**MilkDrop is unavailable.**
Check that WebGL2 is enabled and that you are online (the scripts are loaded from jsDelivr). The player falls back to the LED spectrum.

**A file is added but will not play.**
Your browser may not support its codec or container (some `mkv`, `avi` or `m4a` files, for example). Try another browser or convert the file.

**Tags or cover art are missing.**
Tag reading covers MP3, FLAC and WAV. Other formats display the file name.

## Dependencies

Creepy Amp has no build step and no package manager. The only external code is loaded lazily from [jsDelivr](https://www.jsdelivr.com/) when MILK mode is first enabled:

- [`butterchurn@2.6.7`](https://github.com/jberg/butterchurn), a WebGL2 implementation of MilkDrop
- [`butterchurn-presets@2.4.7`](https://github.com/jberg/butterchurn-presets) (main, extra and extra2 preset packs)

If you need a fully offline build, download these scripts and change the `MILK_SCRIPTS` array in `CreepyAmp.html` to point to your local copies.

> **Note:** the interface text is currently in French (buttons, tooltips and messages). For reference: *ÉGALISEUR* = Equalizer, *EFFETS* = Effects, *AJOUTER* = Add, *VIDER* = Clear, *Aucun morceau chargé* = No track loaded.

## Contributing

Issues and pull requests are welcome. Because the whole app is one file, changes are easy to review: open `CreepyAmp.html`, edit, refresh the page.

Ideas for future versions:

- Persistent playlists (File System Access API or IndexedDB)
- More tag formats (M4A / MP4 atoms, Ogg Vorbis comments)
- Interface translations
- Fully offline bundle of the visualization engine

## License MIT




