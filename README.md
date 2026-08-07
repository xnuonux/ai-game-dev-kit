# AI Game Dev Kit

**A kit that lets any AI coding agent build and ship a real game.**

Any game. 2D or 3D, phone or desktop or console, single-player or networked, web or native engine.
Agent-agnostic (Claude Code, Codex, Kimi, Cursor, Aider, Windsurf) and **engine-agnostic** ... it tells
you how to choose, then gets out of the way.

Nothing to install, nothing to run. It is decisions already made and traps already mapped.

**→ Agents: read [`AGENTS.md`](./AGENTS.md) first.**

---

## Why this exists

A game engine is not what is missing. Phaser, PixiJS, three.js, Godot, Unity ... all free or nearly so,
all excellent, all thoroughly documented. What is missing is everything between *"an agent can write
code"* and *"a game a stranger will finish and rate five stars"*:

- **sound, when you have no sound files** ← the question every agent asks and gets wrong
- the ~200 lines of juice that separate a prototype from a product
- input that respects the hand actually holding the device
- a loop that survives a phone lock, a thermal throttle, and a network
- multiplayer decided *before* it becomes a rewrite
- difficulty and progression curves that are maths, not vibes
- a store path that does not get you delisted
- and a gate that stops an agent declaring its own work finished

## Structure

```
skills/     the universal core ... true for every game ever made
tracks/     the platform + dimension choice ... pick ONE
genres/     the shape you are building, with a real P0
stack/      dependency maps, every licence verified
```

**Core is what transfers. Track is what expires.**

## The core skills

| | |
|---|---|
| `00-start-here` | routing, and the build order that prevents the unplayable demo |
| `game-brief` | the one page written before any code |
| `game-design` | difficulty curves, progression maths, pacing |
| `engine-skills` | **Phaser and PixiJS ship 54 first-party agent skills. Install them.** |
| `game-loop` | fixed timestep, interpolation, the platform truths |
| `game-feel` | the 200 lines, in priority order |
| **`game-audio`** | **procedural SFX ... a whole game's audio, zero files, zero licences** |
| `game-vfx` | particles, hitstop, shake, persistent decals |
| `game-input` | touch, gamepad, keyboard+mouse ... one intent layer |
| `game-ai` | pathfinding, state machines, utility AI, and enemies that feel fair |
| **`game-net`** | **multiplayer. the hardest thing here, and the one that cannot be deferred** |
| `game-save` | schema versioning from commit one |
| `game-economy` | currency and stores, without dark patterns |
| `game-perf` | frame budget on the hardware you actually target |
| `game-art` | CC0 sources, palettes, the silhouette test |
| **`game-artgen`** | **generated art as a pipeline. what models cannot do, and the licence that restricts your OUTPUT** |
| `game-a11y` | accessibility ... a store and legal requirement now |
| **`game-measure`** | **headless probes as instruments. a typechecker proves the pieces FIT, not that any of them MOVE** |
| `game-ship` | signing, ratings, store, per platform |
| `game-verdict` | the gate ... the agent brings receipts, the human judges |

## The tracks

`2d-web` (the default) · `3d-web` · `desktop-steam` · `native-godot` ·
`native-unity-unreal` · `server-authoritative` (composes with all of them)

[`tracks/TRACKS.md`](./tracks/TRACKS.md) decides from three questions, with honest cost estimates.

## The genres

Twenty-five shapes in [`genres/GENRES.md`](./genres/GENRES.md), each with its verb, **its P0**, the
specific way that genre fails, and which skills matter most.

## Install

**Claude Code**
```bash
git clone https://github.com/xnuonux/ai-game-dev-kit
cp -r ai-game-dev-kit/skills/* .claude/skills/
```

**Codex / Kimi / anything else** ... copy `AGENTS.md` into your project root or paste it into a system
prompt. Plain markdown, no code, no dependency.

## The laws

1. The brief precedes the code.
2. Fixed timestep for simulation, interpolated render.
3. Never play the same sound twice identically.
4. Hitstop before particles.
5. The save file has a version number from the first commit.
6. Never trust the client.
7. Deterministic RNG, seeded and stored.
8. Test on the worst hardware you support.
9. No dark patterns ... the honest version converts better.
10. Read the engine's own skill before writing against its API.

## What this will not do

Generate a game from a prompt. Make aesthetic decisions. Tell you your game is good ... only that it is
ready to be judged.

---

MIT. Take it, fork it, strip it. Attribution welcome, not required.
