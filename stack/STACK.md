# The stack

**The choices are made. Do not shop for alternatives.** Every licence below has been checked against
"can I ship a commercial game with this". Evaluating options is hours of budget spent reaching a
decision that is already in this file.

---

## Core

| role | pick | licence | why this one |
|---|---|---|---|
| Language | **TypeScript** | — | Types are how an agent avoids shipping a whole class of bug it cannot see. |
| Build | **Vite** | MIT | Zero-config, instant HMR, fine output. |
| 2D engine | **Phaser 3** | MIT | Largest ecosystem, best docs, batteries included (particles, tilemaps, physics, input). |
| 2D renderer (no engine) | **PixiJS** | MIT | When you want the renderer and none of the framework. Fastest 2D WebGL on the web. |
| Tiny/jam alternative | **Kaplay** | MIT | Fastest to a playable thing. Use for prototypes, not for a year-long project. |
| TS-first alternative | **Excalibur** | BSD-2 | Cleaner API than Phaser, smaller ecosystem. A real choice, not a fallback. |

⚠ **Pick ONE engine and stop.** Mixing renderers is the most expensive mistake available here.

## Audio → see `skills/game-audio`

| role | pick | licence |
|---|---|---|
| Procedural SFX | **ZzFX** | MIT |
| Designed retro SFX | **jsfxr** | Unlicense |
| File playback | **Howler** | MIT |
| Music / generative | **Tone.js** | MIT |

## VFX → see `skills/game-vfx`

| role | pick | licence |
|---|---|---|
| Particles (Pixi) | **@pixi/particle-emitter** | MIT |
| Particles (Phaser/Excalibur) | **built-in** | — |
| Tweening | **@tweenjs/tween.js** | MIT |
| Celebration | **canvas-confetti** | ISC |
| Shake / hitstop / flash | **hand-rolled** | — |

🚨 **`lygia` is under the Prosperity Public License 3.0.0 and CANNOT be used in a commercial game.**
Verbatim: *noncommercial purposes for free... thirty-day trial for commercial purposes.* A shipped
commercial game is a licence violation after thirty days. **Do not use it, do not vendor it, do not
list it as an option.** If you need a 2D shader library, write the shader by hand or use
`paper-design/shaders` (Apache-2.0).

## Simulation

| role | pick | licence | note |
|---|---|---|---|
| 2D physics | **matter-js** | MIT | The default. Easy, good enough for most things. |
| 2D physics, serious | **planck.js** | MIT | Box2D port. Use when matter's solver isn't enough. |
| ECS | **bitECS** | ⚠ **MPL-2.0** | Only when entity counts actually hurt. Premature ECS is a tax. **Not MIT** — weak file-level copyleft. Linking it in a shipped game is fine; **if you modify a bitECS source file and distribute, you must publish that file's source.** Safe as long as you never fork it. |
| Deterministic RNG | **seedrandom** | MIT | **Mandatory for roguelikes.** `Math.random()` cannot be seeded or replayed. |
| Roguelike toolkit | **rot.js** | BSD-3 | FOV, pathfinding, dungeon generation. |

## Input

| role | pick | licence |
|---|---|---|
| Gestures | **@use-gesture/vanilla** | MIT |
| Virtual joystick | **nipplejs** | MIT |

## Persistence

| role | pick | licence | note |
|---|---|---|---|
| Small saves | **localStorage** | — | Under ~1MB. Fine, and simplest. |
| Real saves | **Dexie** (IndexedDB) | Apache-2.0 | Migrations built in — see `skills/game-save`. |
| Save validation | **Zod** | MIT | Validate on load. A corrupt save that throws is better than one that half-loads. |

## Ship

| role | pick | licence |
|---|---|---|
| Native wrapper | **Capacitor** | MIT |
| Perf overlay | **stats.js** | MIT |

## Content

| role | source | licence |
|---|---|---|
| Sprites, 3D, UI, audio | **Kenney** (kenney.nl) | **CC0** — no attribution required |
| Level editor | **LDtk** | MIT |
| Texture atlases | **free-tex-packer** | MIT |
| On-device ML | **transformers.js** | Apache-2.0 |

---

## Install

```bash
npm create vite@latest my-game -- --template vanilla-ts
cd my-game
npm i phaser zzfx howler @tweenjs/tween.js seedrandom
npm i -D @capacitor/core @capacitor/cli
```

That is a complete game stack. **Add nothing else until something actually hurts.**

---

## The licence law

**Every game ships an attribution file, generated at build time, and the build fails on any
GPL/AGPL/LGPL dependency in the tree.**

Google Play and the App Store both want attribution, and retrofitting it during review is expensive.
Failing the build is cheap. See `skills/game-ship`.

```bash
npx license-checker --production --summary
```

⚠ Nothing in this file is copyleft. If you add a dependency that is, you have changed the licensing
of the game you are building, and you must tell the human before you do it.
