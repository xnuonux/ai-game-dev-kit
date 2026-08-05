# Tracks

**Pick one. Do not evaluate alternatives ... the trade-offs are written below and the decision is
three questions deep.**

Choosing an engine is the most over-researched decision in game development and one of the least
consequential, because **every engine here can build every genre.** What actually differs is the cost
of shipping to a given platform, and that is what these tracks encode.

---

## The three questions

**1. Where must it run on day one?**
phone → `2d-web` or `3d-web` · Steam → `desktop-steam` or `native-godot` ·
console → `native-godot` or `native-unity-unreal` · browser → `2d-web` / `3d-web`

**2. Is it 2D or 3D?**
Not "could it be 3D". ⚠ **3D is roughly 3-5x the asset and engineering cost** for the same amount of
game. Choose 2D unless the game genuinely requires a third axis.

**3. Do two people interact in real time?**
Yes → **add `server-authoritative`.** ⚠ Retrofitting networking is a rewrite, not a feature. Decide now.

---

## The tracks

### `2d-web` ... the default, and the cheapest path to a shipped game
**Stack:** TypeScript · Phaser 4 or PixiJS · Vite · Capacitor for stores.
**Ships to:** browser, Android, iOS, and desktop via Tauri. One codebase.
**Best for:** anything 2D. Puzzles, arcade, roguelites, strategy, merge, tower defence, platformers,
card games, visual novels, idle.
**Costs:** no console path. WebGL performance ceiling below native. Large-world streaming is awkward.
⚠ **Pick this unless you have a specific reason not to.** It is the shortest distance between an
agent and a game a stranger can play.

### `3d-web` ... 3D from one codebase
**Stack:** TypeScript · three.js (+ react-three-fiber if React) or Babylon.js · Rapier physics · glTF.
**Ships to:** browser, mobile via Capacitor, desktop via Tauri.
**Best for:** stylised 3D, low-poly, first-person exploration, physics toys, 3D puzzle games.
**Costs:** mobile GPU budget is tight. No console. Asset pipeline is real work.
⚠ **Do not attempt open-world or high-fidelity here.** Stylised and small-scope is where this wins.

### `desktop-steam` ... PC-first
**Stack:** the web stack wrapped in **Tauri** (small, Rust) or Electron (heavy, familiar), OR Godot.
Steamworks SDK for achievements, cloud saves, workshop.
**Best for:** long-session games, mod support, keyboard+mouse depth, anything with a UI-heavy design.
**Costs:** Steam is discovery-hard. A store page is a marketing project of its own.
⚠ Controller support is not optional on Steam ... see `skills/game-input`.

### `native-godot` ... when web is not enough and you want to stay free
**Stack:** Godot 4 · GDScript (fast to write) or C# (faster to run).
**Ships to:** Windows, macOS, Linux, Android, iOS, web (heavier), **and consoles via third-party ports.**
**Best for:** 3D that needs real performance, heavy 2D, anything wanting one engine across every
platform. MIT licensed, no royalties, no seat fees.
**Costs:** you leave the JS ecosystem. Smaller talent pool. Web export is large.
⚠ **The strongest choice for a serious 3D project with no budget.**

### `native-unity-unreal` ... console certification and AAA fidelity
**Best for:** shipping to console under your own name, photorealism, an existing team or codebase.
**Costs:** licensing terms that change (⚠ read the current ones, they have moved before), heavy
tooling, slow iteration, a large team assumption.
⚠ **Do not choose this because it is famous.** For an agent-built game it is almost always the wrong
answer ... the iteration loop is slow and the engines assume humans in an editor.

### `server-authoritative` ... composes with any of the above
**Not an engine choice. An architecture choice, and the most consequential one in this document.**
Required for: real-time multiplayer, competitive play, leaderboards that matter, anything purchasable.
→ `skills/game-net`.

---

## Cost, honestly

| track | P0 to playable | shipped 1.0 | ceiling |
|---|---|---|---|
| `2d-web` | days | weeks | high for 2D |
| `3d-web` | 1-2 weeks | months | medium |
| `desktop-steam` | 1-2 weeks | months | high |
| `native-godot` | 1-2 weeks | months | very high |
| `native-unity-unreal` | weeks | many months | highest |
| `+ server-authoritative` | **multiplies everything by ~2** | | |

⚠ These assume an agent building with a human reviewing. **They are the honest numbers, not the
optimistic ones**, and the multiplier on networking is real.

---

## Switching later

**Cheap:** 2D-web → desktop (wrap in Tauri). Godot desktop → Godot mobile.
**Expensive:** 2D → 3D. Web → native. Anything → console.
🚨 **Effectively a rewrite:** single-player → real-time multiplayer.

**Decide the third question now.** Everything else you can move later.
