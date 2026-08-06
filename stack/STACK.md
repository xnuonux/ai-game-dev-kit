# The stack

**The choices are made. Do not shop for alternatives.** Every licence is checked against *"can I ship
a commercial game with this"*. Evaluating options is hours of budget spent reaching a decision that is
already in this file.

⚠ **Read `LICENCE-TRAPS.md` before adding anything not listed here.**

---

## Universal ... every track, every genre

| role | pick | licence | note |
|---|---|---|---|
| Language | **TypeScript** | ... | types are how an agent avoids a bug class it cannot see |
| **Deterministic RNG** | **seedrandom** | MIT | ⚠ **mandatory.** `Math.random()` cannot be seeded, replayed, or synced. → law 7 |
| **Procedural SFX** | **ZzFX** | MIT | ~1KB. a whole game's audio as parameters. → `game-audio` |
| SFX, designed | **jsfxr** | **Unlicense** | sfxr port with a GUI. public domain |
| Audio playback | **Howler** | MIT | files, sprites, the mobile unlock |
| Music / generative | **Tone.js** | MIT | lazy-load it, it is large |
| Tweening | **@tweenjs/tween.js** | MIT | |
| Save validation | **Zod** | MIT | validate on load. → `game-save` |
| Perf overlay | **stats.js** | MIT | |

⚠ **Shake, hitstop and flash are hand-rolled**, ~40 lines. There is no library worth the dependency,
and `game-vfx` has the code.

---

## `2d-web`

| role | pick | licence |
|---|---|---|
| Engine | **Phaser 4** (default) | MIT |
| Renderer only | **PixiJS 8** | MIT |
| TS-first alternative | **Excalibur** | BSD-2 |
| Prototype / jam | **Kaplay** | MIT |
| Build | **Vite** | MIT |
| Store wrapper | **Capacitor** | MIT |

🚨 **One engine. Mixing renderers is the most expensive mistake on this track.**

```bash
npm i phaser zzfx howler @tweenjs/tween.js seedrandom
npm i -D vite @capacitor/core @capacitor/cli
```

## `3d-web`

| role | pick | licence |
|---|---|---|
| Renderer | **three.js** | MIT |
| React layer | **react-three-fiber** + **drei** | MIT |
| All-in-one alternative | **Babylon.js** | Apache-2.0 |
| Physics | **Rapier** | Apache-2.0 |
| Physics, lighter | **cannon-es** | MIT |
| Post-processing | **postprocessing** (pmndrs) | MIT |
| Models | **glTF / .glb** + Draco + KTX2 | ... |

## `desktop-steam`

| role | pick | licence |
|---|---|---|
| Shell, small | **Tauri** | MIT/Apache-2.0 |
| Shell, familiar | **Electron** | MIT |
| Steam | **steamworks.js** | MIT |

## `native-godot`

**Godot 4** ... MIT, no royalties. GDScript for logic, C# for hot paths.
⚠ Nothing from the npm lists above applies. Bake ZzFX-designed sounds to WAV and import them.

## `server-authoritative`

| role | pick | licence |
|---|---|---|
| Rooms + state sync | **Colyseus** | MIT |
| Raw realtime | **ws** / **uWebSockets.js** | MIT / Apache-2.0 |
| Ephemeral state | **Redis** | BSD-3 / RSAL ⚠ check version |
| Durable state | **Postgres** | PostgreSQL |

---

## Simulation and content

| role | pick | licence | note |
|---|---|---|---|
| 2D physics | **matter-js** | MIT | the default |
| 2D physics, serious | **planck.js** | MIT | Box2D port |
| ECS | **miniplex** | MIT | ⭐ the clean default. And only when entity counts actually hurt |
| ECS, alternative | **koota** (pmndrs) | ISC | archetype-based, r3f-friendly |
| ~~ECS~~ | ~~bitECS~~ | 🚨 **MPL-2.0** | ⚠ **not MIT**, and widely mislabelled as such. file-level copyleft. **prefer miniplex or koota** |
| Roguelike toolkit | **rot.js** | BSD-3 | FOV, pathfinding, dungeon gen |
| Particles (Pixi) | **@pixi/particle-emitter** | MIT | |
| Particles (Phaser/Excalibur/Godot) | **built-in** | ... | ⚠ do not add a second one |
| Gestures | **@use-gesture/vanilla** | MIT | |
| Virtual joystick | **nipplejs** | MIT | |
| Saves | **Dexie** (IndexedDB) | Apache-2.0 | migrations built in |
| On-device ML | **transformers.js** | Apache-2.0 | |
| Art | **Kenney** | **CC0** | 2D and 3D. no attribution required |
| Levels | **LDtk** | MIT | |
| Atlases | **free-tex-packer** | MIT | |

🚨 **`lygia` (shaders) is Prosperity 3.0.0 and CANNOT be used in a commercial game.** 30-day trial
only, and it appears on every shader shortlist as though it were permissive.

⭐ **Use `webgl-noise` (MIT, Stefan Gustavson) instead.** It is the canonical GLSL simplex/classic
noise implementation and it is what anyone actually wanted from lygia. For larger effect libraries,
`paper-design/shaders` (Apache-2.0).

⚠ **`PathFinding.js` is rejected despite 8.7k stars: it has no licence file, which means all rights
reserved by default.** Use `rot.js` (BSD-3), which includes A\*, FOV and dungeon generation.

---

## The licence gate

```bash
npx license-checker --production --summary
```

**Fail the build on any AGPL / GPL / LGPL**, and generate `NOTICES` from what remains. Both stores
want attribution and retrofitting it under review is expensive.

⚠ **`license-checker` reads `package.json`, which is sometimes wrong.** For anything load-bearing,
open the actual LICENSE file. → `LICENCE-TRAPS.md`.
