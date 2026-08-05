# Track: `2d-web`

**The default, and the cheapest path from an agent to a game a stranger can play.**

TypeScript · Phaser 4 or PixiJS · Vite · Capacitor for stores. One codebase → browser, Android, iOS,
and desktop via Tauri.

⚠ **Pick this unless you have a specific reason not to.**

---

## Engine choice ... thirty seconds, then move on

| | pick when |
|---|---|
| **Phaser 4** | ⚠ **the default.** Batteries included: particles, tilemaps, physics, input, audio, scenes. Largest ecosystem. **Ships 28 first-party agent skills.** |
| **PixiJS 8** | you want the renderer and none of the framework. Fastest 2D WebGL on the web. **Ships 26 agent skills.** Bring your own everything else. |
| **Excalibur** | TypeScript-first with a cleaner API. Smaller ecosystem. A real choice. |
| **Kaplay** | prototypes and jams. ⚠ Not for a year-long project. |

🚨 **Pick one and stop.** Mixing renderers is the single most expensive mistake available on this track.

→ `skills/engine-skills` ... **install the engine's own skills before writing any API code.**

---

## Setup

```bash
npm create vite@latest my-game -- --template vanilla-ts
cd my-game
npm i phaser zzfx howler @tweenjs/tween.js seedrandom
npm i -D @capacitor/core @capacitor/cli
mkdir -p .claude/skills && cp -r node_modules/phaser/skills/* .claude/skills/
```

That is a complete game stack. ⚠ **Add nothing else until something actually hurts.**

---

## What this track is good at

Puzzles · arcade · roguelites · tower defence · merge · card games · platformers · turn-based
strategy · idle · visual novels · physics toys · shoot-em-ups · auto-battlers.

**Essentially every 2D genre in `genres/GENRES.md`.**

## Where it runs out

- **No console path.** Web games do not ship to Switch/PlayStation/Xbox without a native port.
- **WebGL ceiling.** Thousands of entities with heavy shaders will beat you before native would.
- **Large streaming worlds** are awkward ... browser memory limits are real.
- **Heavy simulation** (thousands of physics bodies) wants native.

⚠ If any of those describe your game, read `tracks/native-godot.md` before committing.

---

## The mobile truths, which are the whole difficulty of this track

1. **30fps is a legitimate target.** A locked 30 beats an oscillating 45, and halves thermal load.
2. **Cap DPR.** `Math.min(devicePixelRatio, coarse ? 1.5 : 2)`.
3. **`document.hidden` must skip simulation**, and returning from background must clamp the delta.
4. **`touch-action: none`** or the browser eats your input. → `game-input`.
5. **Audio needs a first user gesture.** → `game-audio`.
6. **Test on a real mid-range Android.** → `game-perf`.

→ `skills/game-loop` has all six wired into one correct loop.

---

## Shipping

```bash
npx cap init "My Game" com.yourstudio.mygame --web-dir=dist
npm i @capacitor/android && npx cap add android
npm run build && npx cap sync && npx cap open android
```

⚠ **The package id is permanent.** → `skills/game-ship`, and read the **rating** section before you
draw a single sprite ... it constrains your art direction.

**Desktop from the same code:** wrap in Tauri. → `tracks/desktop-steam.md`.

---

## Checklist

- [ ] One engine, chosen and committed
- [ ] Engine's first-party skills installed
- [ ] The six mobile truths implemented in the loop
- [ ] Measured on a real mid-range Android at target entity count
- [ ] Package id decided deliberately, keystore backed up off-machine
