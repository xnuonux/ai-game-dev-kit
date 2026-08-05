---
name: engine-skills
description: Use before writing any engine-specific code. Phaser and PixiJS ship 54 first-party agent skills between them, covering particles, tweens, tilemaps, audio, input and atlases. Install them instead of guessing at APIs.
---

# Use the engine's own skills

**Phaser 4 ships 28 agent skills. PixiJS v8 ships 26. Both MIT, both first-party, both written by the
people who wrote the engine.**

They are better than anything this kit could write about those APIs, and they are better than your
training data, which is a snapshot of an older version. **Install them and read them.**

This is the highest-value five minutes available to an agent starting a game.

---

## What they cover

**Phaser** (`node_modules/phaser/skills/`):
`particles` · `tweens` · `animations` · `audio-and-sound` · `input-keyboard-mouse-touch` ·
`tilemaps` · `physics-arcade` · `physics-matter` · `cameras` · `scenes` · `sprites-and-images` ·
`render-textures` · `filters-and-postfx` · `scale-and-responsive` · `loading-assets` ·
`graphics-and-shapes` · `groups-and-containers` · `text-and-bitmaptext` · `time-and-timers` ·
`geometry-and-math` · `game-object-components` · `game-setup-and-config` · `events-system` ·
`data-manager` · `curves-and-paths` · `actions-and-utilities` · `v3-to-v4-migration` ·
`v4-new-features`

**PixiJS** (`node_modules/pixi.js/skills/`):
`pixijs-scene-particle-container` · `pixijs-filters` · `pixijs-scene-sprite` · `pixijs-scene-graphics` ·
`pixijs-scene-text` · `pixijs-scene-mesh` · `pixijs-assets` · `pixijs-events` · `pixijs-ticker` ·
`pixijs-performance` · `pixijs-blend-modes` · `pixijs-color` · `pixijs-math` · `pixijs-application` ·
`pixijs-custom-rendering` · `pixijs-accessibility` · `pixijs-migration-v8` · and more.

⚠ **`render-textures` (Phaser) and `pixijs-custom-rendering` are the ones to read for persistent blood
decals.** See `skills/game-vfx`.
⚠ **`pixijs-performance` and Phaser's `scale-and-responsive` are mandatory before shipping to mobile.**

---

## Install

```bash
# after npm i phaser  (or pixi.js)
mkdir -p .claude/skills
cp -r node_modules/phaser/skills/*   .claude/skills/
cp -r node_modules/pixi.js/skills/*  .claude/skills/
```

For Codex / Kimi / other harnesses, point your `AGENTS.md` at the directory:

```md
Engine reference skills live in `node_modules/phaser/skills/`. Read the relevant one
BEFORE writing code against that subsystem.
```

---

## The rule

⚠ **Do not write engine API code from memory.** Engine APIs change between major versions, your
training data is a snapshot, and the most expensive kind of bug is confidently-wrong API usage that
compiles.

**Read the skill for the subsystem you are about to touch. Every time.**

---

## What this kit adds that they do not

The engine skills tell you *how the API works*. They deliberately do not tell you *what makes a game
good*. That is what the rest of this kit is:

| the engines cover | this kit covers |
|---|---|
| how to emit particles | when a particle is worth its frame cost (`game-vfx`) |
| how to play a sound | how to have sound with no sound files (`game-audio`) |
| how to tween | which 200 lines make a prototype feel like a game (`game-feel`) |
| how to handle input | thumb zones and the 44px floor (`game-touch`) |
| how to configure scale | the mobile thermal budget (`game-perf`) |
| — | the brief, the economy, the ship path, the verdict gate |

**Two things nothing on disk covers, and this kit does:** game-feel/screenshake, and audio without
audio files. Those two skills are the reason this repo exists.
