# AI Game Dev Kit — Claude Code entry point

**Read [`AGENTS.md`](./AGENTS.md).** It is the universal entry point and it is harness-agnostic.

## Claude Code specifics

Install the skills so they load on demand:

```bash
mkdir -p .claude/skills
cp -r skills/* .claude/skills/
```

Then also install the engine's **first-party** skills once you have picked one — Phaser ships 28 and
PixiJS ships 26, both MIT, both better than anything written from memory:

```bash
cp -r node_modules/phaser/skills/*  .claude/skills/   # or pixi.js
```

See `skills/engine-skills/SKILL.md`.

## Order of operations

1. `00-start-here` — always first
2. `game-brief` — **before any code**
3. `engine-skills` — before touching an engine API
4. build in the order the brief gives
5. `game-verdict` — **before telling the human anything is done**

## The one rule that matters most here

⚠ **You cannot declare a game finished.**

You have never played it. You cannot feel boredom or delight. You have read every line, so nothing in
it can surprise you.

Produce the clip, the first-90-seconds trace, the frame-time graph on a real device, and the list of
everything you measured as falling short. Then stop, and let the human judge.

An agent that says "done, it's great" is worth nothing, because it says that every time.
