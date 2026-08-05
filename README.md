# AI Game Dev Kit

**A kit that lets any coding agent build and ship a real mobile game.**

Claude Code, Codex, Kimi, Cursor, Aider, Windsurf, Reasonix — anything that reads markdown and writes
files. No framework, no install, no runtime. It is a set of decisions already made and a set of
traps already mapped, so the agent's budget goes into the game instead of into the eight problems
every game re-solves from scratch.

**→ Agents: read [`AGENTS.md`](./AGENTS.md) first.**

---

## Why this exists

A game engine is not what is missing. Phaser, PixiJS, Excalibur and Kaplay are all free, excellent,
and MIT. What is missing is everything between *"an agent can write code"* and *"a game a stranger
will finish and rate five stars"*:

- touch controls that feel right on a real hand
- a loop that survives a phone lock and a thermal throttle
- **sound, when you have no sound files** ← the question everyone asks
- the ~200 lines of juice that separate a prototype from a product
- a save file that survives its own first patch
- a store path that does not get you delisted
- and a gate that stops the agent declaring its own work finished

## What's inside

**12 skills.** Read `skills/00-start-here/SKILL.md` and it routes you.

| | |
|---|---|
| `00-start-here` | routing + the build order that prevents the unplayable demo |
| `game-brief` | the one page written before any code |
| `engine-skills` | **Phaser and PixiJS ship 54 first-party agent skills. Install them.** |
| `game-loop` | fixed timestep, interpolation, the mobile truths |
| `game-feel` | the 200 lines, in priority order |
| **`game-audio`** | **procedural SFX — a whole game's audio, zero files, zero licences** |
| `game-vfx` | particles, hitstop, screen shake, persistent gore decals |
| `game-touch` | thumb zones, the 44px floor, the browser defaults that ruin input |
| `game-save` | schema versioning from commit one |
| `game-economy` | currency and a store, without the dark patterns |
| `game-perf` | mobile GPU budget, the allocation trap, measuring instead of guessing |
| `game-art` | CC0 sources, palette discipline, the silhouette test |
| `game-ship` | Capacitor → signed AAB → store, and the rating mistake that delists games |
| `game-verdict` | the gate — the agent brings receipts, the human judges |

Plus **[`stack/STACK.md`](./stack/STACK.md)** — the dependency map, every licence verified against
*"can I ship a commercial game with this"*.

## Install

**Claude Code**

```bash
git clone https://github.com/xnuonux/ai-game-dev-kit
cp -r ai-game-dev-kit/skills/* .claude/skills/
```

**Codex / Kimi / anything else** — copy `AGENTS.md` into your project root, or paste it into a system
prompt. It is plain markdown. There is no code and no dependency.

## The two things nothing else covers

Most of what an agent needs about an engine's API, that engine already documents better than anyone
else could — see `engine-skills`. **Two gaps are real and this kit fills them:**

1. **Audio with no audio files.** ZzFX and jsfxr generate a complete game's SFX set from a few hundred
   bytes of parameters. Consistent, free, zero licence risk, zero download weight. Plus the craft
   rules — pitch randomisation, layering, voice limiting, the mobile unlock — that separate "has
   audio" from "sounds good".
2. **Game feel.** ~200 lines, in the order that buys the most per line. Hit flash, hitstop,
   pitch-randomised sound, trauma-based shake. An agent will never write these unprompted because the
   game functions perfectly without them and feels like nothing.

## The laws

1. The brief precedes the code.
2. Fixed timestep for simulation, interpolated render.
3. Never play the same sound twice identically.
4. Hitstop before particles.
5. The save file has a version number from the first commit.
6. 44px targets, nothing in the top 15% of the screen.
7. Test on a real mid-range phone. The laptop lies.
8. No dark patterns — and the honest version converts better.

## What this will not do

It will not generate a game from a prompt. It will not make aesthetic decisions. It will not tell you
your game is good — only that it is ready to be judged.

---

MIT. Take it, fork it, strip it. Attribution welcome, not required.
