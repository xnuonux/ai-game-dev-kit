# Track: `desktop-steam`

PC-first. Long sessions, keyboard+mouse depth, mods, controller support, Steam.

---

## Stack

**Wrap the web stack, or use Godot.**

| approach | use when | trade |
|---|---|---|
| **Tauri** (Rust shell) | you have a web game and want a small binary | ~10 MB, fast, less mature ecosystem |
| **Electron** | you need Node APIs and maximum familiarity | ⚠ ~120 MB, heavy RAM, but everything works |
| **Godot** | 3D, or heavy simulation | native perf, leaves the JS ecosystem |

Steam integration: **steamworks.js** (Node/Electron/Tauri) or Godot's GodotSteam.

---

## What Steam actually requires

- ⚠ **Full controller support.** Not optional. Steam Input remaps for you, but your game must accept
  gamepad natively. → `game-input`
- **Steam Deck verification** ... a real funnel. Requires: gamepad-only navigation, ⚠ **readable text at
  1280×800**, no launcher popup, and default graphics settings that hit 30fps.
- **Cloud saves.** Cheap to add, and players expect it.
- **Achievements.** ⚠ Design them *into* the game, do not bolt them on. They are a discovery surface.
- **A real store page.** Trailer, screenshots, tags, description. This is a marketing project, not a
  checkbox, and it does more for sales than most features.

---

## What desktop lets you do that mobile does not

- **Long sessions** ... 30-120 minutes. Design for them; add a save-anywhere.
- **Dense UI.** A mouse is precise. Spreadsheets and deep menus are *viable* here and hostile on phones.
- **Mods.** ⚠ Steam Workshop is one of the strongest retention mechanics that exists. If your game
  could support mods, that decision belongs in the brief, not in a patch.
- **Multiple windows, big screens, real keyboards.**

## What it costs

- 🚨 **Discovery is brutal.** Thousands of games launch weekly. Wishlists before launch are the whole
  game ... a Steam page should exist months before the build does.
- **Refunds under 2 hours.** ⚠ Your first two hours must be genuinely good or the refund rate eats you.
- **Reviews are permanent and public**, and the first fifty set the tone forever.

---

## Building

```bash
# Tauri
npm create tauri-app@latest
npm run tauri build     # → .exe / .app / .AppImage

# Electron
npm i -D electron electron-builder
npx electron-builder --win --mac --linux
```

⚠ **Code sign on Windows and macOS.** Unsigned binaries trigger SmartScreen and Gatekeeper warnings
that read as "this is malware" to a normal person, and it will show up in your reviews.

---

## Settings menu ... expected, not optional

Desktop players expect: resolution, windowed/fullscreen/borderless, vsync, framerate cap, ⚠ **FOV
slider** (its absence is a top complaint in 3D games), master/music/SFX volume, full keybinding,
graphics presets, and accessibility (`game-a11y`).

**Persist all of it. Read it before first render**, so nobody sees a wrong-resolution flash.

---

## Checklist

- [ ] Full gamepad support, tested with a real controller
- [ ] Steam Deck: gamepad-only nav, text readable at 1280×800, 30fps default
- [ ] Cloud saves, achievements designed in
- [ ] Signed binaries on Windows and macOS
- [ ] Complete settings menu, persisted, applied before first render
- [ ] ⚠ The first two hours are genuinely good ... the refund window is your real review gate
- [ ] Store page live long before launch
