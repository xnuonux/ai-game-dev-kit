# AI GAME DEV KIT

**You are an AI coding agent. You have been asked to build a game. Read this file first.**

Any game. 2D or 3D, phone or desktop or console, single-player or networked, web or native engine.
This kit is agent-agnostic (Claude Code, Codex, Kimi, Cursor, Aider, Windsurf) and
**engine-agnostic** ... it tells you how to choose, then gets out of the way.

There is nothing to install and nothing to run. It is decisions already made and traps already mapped,
so your budget goes into the game instead of into the problems every game re-solves from scratch.

---

## Structure

```
skills/     the universal core ... true for every game ever made
tracks/     the platform + dimension choice ... pick ONE
genres/     the shape you are building, with a real P0 definition
stack/      dependency maps per track, every licence verified
```

**Core first, then a track, then a genre.** In that order. The core is the part that transfers; the
track is the part that expires.

---

## Step 1 ... the brief

`skills/game-brief`. **No code before this exists.** A one-line prompt produces a demo: technically
fine, no loop, no reason to play twice.

## Step 2 ... pick a track

`tracks/TRACKS.md` decides for you from three questions. Do not shop; the trade-offs are already
written down.

| track | when |
|---|---|
| `2d-web` | 2D, mobile or web-first. **The cheapest path to a shipped game.** |
| `3d-web` | 3D that must run in a browser or ship to phones from one codebase |
| `desktop-steam` | PC-first, Steam, long sessions, mods, controller |
| `native-godot` | 3D or heavy 2D, native performance, free and open, console-capable |
| `native-unity-unreal` | console certification, AAA fidelity, or an existing team |
| `server-authoritative` | **any game with real-time multiplayer or anything worth cheating at** |

⚠ Tracks compose. Most networked games are `2d-web` **+** `server-authoritative`.

## Step 3 ... pick the genre shape

`genres/GENRES.md`. Each genre gives the verb, the P0 that proves it, the specific way that genre
fails, and which core skills matter most for it.

## Step 4 ... build

```
1. the verb          one input doing one thing
2. the loop          that verb repeated, with a reason
3. THE FEEL          skills/game-feel ... minimum steps 1-3
4. progression       the reason to play tomorrow
5. the shell         menus, save, settings, pause
6. ship              skills/game-ship
```

⚠ **Feel at step 3, not at the end.** A game that is fun with primitive shapes will be fun with art.
One that is not will not be saved by art, and you will spend the whole budget finding out.

## Step 5 ... the gate

`skills/game-verdict`. **You are not permitted to declare a game finished.** You produce evidence; a
human judges.

---

## The core skills

Every one of these is true whether you are building a match-3, a flight sim, an MMO or a visual novel.

| skill | read it when |
|---|---|
| `game-brief` | before any code |
| `game-design` | genre patterns, difficulty curves, progression maths |
| `game-loop` | update/render architecture, any platform |
| `game-feel` | it works but feels dead |
| `game-audio` | **you need sound and have no sound files** |
| `game-vfx` | particles, impact, screen effects, 2D or 3D |
| `game-input` | touch, gamepad, keyboard+mouse, and switching between them |
| `game-ai` | enemies, pathfinding, behaviour, companions |
| `game-net` | **two or more players. the hardest thing in this kit.** |
| `game-save` | persistence, and the patch after launch |
| `game-economy` | currency, stores, IAP, without dark patterns |
| `game-perf` | frame budget on the hardware you actually target |
| `game-art` | art and no artist; 2D and 3D pipelines |
| `game-a11y` | accessibility ... and it is a store requirement now, not a nicety |
| `game-ship` | store, signing, ratings, per platform |
| `game-verdict` | before claiming anything is done |
| `engine-skills` | **before writing any engine API code** |

---

## Laws

Platform-independent. Breaking one produces a specific, predictable failure.

1. **The brief precedes the code.**
2. **Fixed timestep for simulation, interpolated render.** Anything else desyncs and breaks physics on
   slow hardware ... and makes networked play impossible.
3. **Never play the same sound twice identically.** Pitch-randomise every playback.
4. **Hitstop before particles.** If you add one effect, freeze the frame on impact.
5. **The save file has a version number from the first commit.**
6. **Never trust the client.** For anything competitive or purchasable. → `game-net`, `game-economy`.
7. **Deterministic RNG, seeded and stored.** `Math.random()` cannot be replayed, debugged or synced.
8. **Test on the worst hardware you support**, not on your machine. Your machine lies.
9. **No dark patterns** ... and the honest version converts better. → `game-economy`.
10. **Read the engine's own skill before writing against its API.** → `engine-skills`.

## What this kit will not do

Generate a game from a prompt. Make aesthetic decisions. Tell you your game is good ... only that it is
ready to be judged.

MIT. Take it, fork it, strip it.
