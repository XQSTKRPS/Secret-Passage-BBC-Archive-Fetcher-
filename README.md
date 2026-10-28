```
        ┌────────────┐▓╲
        │ ┃┃▌┃┃ ┃┃▌┃ │▓░╲
        │ ┃┃▌┃┃ ┃┃▌┃ │▓░░│
        ├────────────┤▓░░│
        │ ┃▌┃┃ ╱ ┃▌┃ │▓░░│
        │ ┃▌┃┃╱  ┃▌┃ │▓░░│
        ├────────────┤▓░░│
        │ ┃┃┃▌┃ ┃┃┃▌ │▓░░│
        │ ┃┃┃▌┃ ┃┃┃▌ │▓░╱
        └────────────┘▓╱

     S E C R E T   P A S S A G E
     ·  BBC Sound Effects Library  ·
```

# Secret Passage (BBC Archive Fetcher)

A Max for Live device for Ableton Live that opens a hidden door into the
[BBC Sound Effects](https://sound-effects.bbcrewind.co.uk/) archive: 33,066
recordings you can search, audition and drop into your set without opening a
browser.

> Unofficial. Not made by or affiliated with the BBC. The recordings are
> licensed by the BBC under the RemArc licence (personal, educational and
> research use). Commercial use needs a licence from the BBC's partner.

---

## What it does

- **Searches the whole archive offline.** The full catalogue (metadata only,
  about 11 MB) is cached on first run, so searching is instant and works
  without a connection.
- **85 categories.** The BBC's 23, plus 64 keyword categories grouped into
  *The Wilds*, *Elements*, *Halls*, *Journeys*, *Artefacts*, *Voices*,
  *Battle* and *Curiosities*. The category filter is typeable.
- **Auditions through your track.** Previews stream into the device's track,
  with an oscilloscope-style waveform you can click to scrub. Auditioned
  previews are cached, so they also play offline.
- **Loads straight into Live:**
  - **→ Clip**: the highlighted Session slot on an audio track, or the next
    empty slot below it
  - **→ Arr**: the selected audio track, at the playhead
  - **→ Simpler**: a new MIDI track holding a Simpler with the sound loaded
- **✶ Random** pulls a random recording from whatever is currently on screen.
- **Auto** previews each selection, **Loop** repeats the preview, ★ marks
  favourites, and there are filters for length and sort order.
- **Downloads** are saved as WAV (or MP3) to `~/Music/BBC SFX/<Category>/`.
  Add that folder to Live's Places to drag files from Live's own browser.

## How it looks

The device in the chain is just the symbol. Click it to open the panel, a
floating, resizable window styled after a 1970s studio: enamel panel, lamp
keys, a green phosphor screen, a CRT oscilloscope and pixel-blackletter
lettering.

## Requirements

- macOS, Ableton Live 12.4 or later with Max for Live (Suite, or Standard
  with the add-on). It uses the Max 9 bundled with Live.
- Internet for downloading and for first-time previews. Searching works
  offline once the index is cached.
- Optional: the **Monofonto** font (Typodermic) installed in your user fonts.
  Without it the panel falls back to Share Tech Mono.

## Install

1. Download **`Secret Passage (BBC Archive Fetcher).amxd`** from this repository.
   It is a frozen device: everything it needs is inside that one file.
2. Drop it into
   `~/Music/Ableton/User Library/Presets/Audio Effects/Max Audio Effect/`,
   or drag it straight onto an audio track in Live.
3. In Live's browser, type **BBC** or **Secret** to find it again.
4. Click the symbol to open the panel. On first run, click
   **Build offline index** in the panel footer if it isn't cached yet. It
   takes under a minute.

## Use

| Do this | To get this |
|---|---|
| Click the symbol | Opens the panel |
| Type in the search box | Searches as you type. `-word` excludes a word. |
| Type in *filter categories…*, then press Enter | Picks the first matching category |
| Click a row | Selects it, and previews it if Auto is on |
| Double-click a row, or press Enter | → Clip |
| ↑ / ↓, Space | Step through results / play or stop |
| Click the waveform | Scrubs to that point |
| ✶ Random | A random recording from the current view |

## Known limitation: fullscreen Live

Max for Live windows belong to the separate Max process. When Live is in
macOS fullscreen, macOS moves any such window into its own Space, and Live
slides away. This affects every M4L device that opens a window. Use Live
maximised instead: ⌥-click the green button, or double-click the title bar.

## How it works

```
 Live device (104 px symbol, jweb~)          Panel window (jweb~, floating)
          │ click                                  │ search / select / load
          ▼                                        ▼
   node.script  sp_server.js  ── local HTTP + event stream ── sp_ui.html
     │  catalogue index, search, preview cache, downloads
     │  outlet "load <target> <path>"
     ▼
   deferlow → js sp_lom.js ── Live Object Model ── clip / arrangement / Simpler
```

| File | Role |
|---|---|
| `sp_server.js` | Node for Max server: BBC API, offline index, search, preview cache, downloads, events |
| `sp_tags.js` | Keyword categories and their sidebar groups |
| `sp_ui.html` | The panel |
| `sp_badge.html` | The symbol shown in the device chain |
| `sp_lom.js` | Loads files into clip slots, the arrangement or a new Simpler track |
| `sp_window.js` | Keeps the panel floating, resizable, and remembers its size |
| `fonts/` | Jacquard 24 and Share Tech Mono (SIL Open Font License) |
| `build.py` | Generates the `.amxd` and installs everything into the User Library |

Rebuild after editing with `python3 build.py`. That writes the unfrozen
device into the User Library and the frozen one, with the server, pages and
fonts bundled into a single script, to `dist/`.

Data lives in `~/Music/BBC SFX/.cache/`:

- `index.json`: the catalogue
- `state.json`: favourites, downloads, settings and panel position
- `previews/`: auditioned MP3s, capped at 600 MB
- `debug.log`: every load request and reply from Live

## Credits and licences

- Sound recordings: © BBC, via the BBC Sound Effects archive, under the RemArc
  licence. This device only fetches what the public site serves.
- Jacquard 24: The Soft Type Project Authors, SIL OFL 1.1.
- Share Tech Mono: Carrois Type Design, SIL OFL 1.1.
- Monofonto: Typodermic Fonts. Not bundled, because its desktop licence does
  not allow redistribution.
