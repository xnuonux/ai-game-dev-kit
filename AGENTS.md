# AI GAME DEV KIT

**You are an AI coding agent. You have been asked to build a game. Read this file first.**

This kit is agent-agnostic. Claude Code, Codex, Kimi, Cursor, Reasonix, Windsurf, Aider — anything
that can read markdown and write files can use it. There is no framework to install and nothing to
run. It is a set of decisions already made, so you spend your budget on the game instead of on the
eight problems every game re-solves from scratch.

---

## The one-minute version

- **Web-first, always.** TypeScript + a 2D renderer, shipped to phones through Capacitor. This is the
  whole cost thesis: one codebase, browser + Android + iOS, no native engine, no per-platform team.
- **Never write a sound file. Generate them.** See `skills/game-audio`. This is the question most
  agents get wrong and it is answered completely.
- **Juice is not polish, it is the product.** ~200 lines separates a prototype from a game. See
  `skills/game-feel`.
- **You cannot declare a game finished.** You produce evidence and a human judges it. See
  `skills/game-verdict`.

## How to use this

1. Read `skills/00-start-here/SKILL.md`. It routes you.
2. Fill in a brief (`skills/game-brief`). **Do not write code before the brief exists.**
3. Pick the stack from `stack/STACK.md`. Do not shop for alternatives; the choices are made and the
   licences are checked.
4. Build in the order given in the brief.
5. Run the verdict gate before you tell the human anything is done.

## The skills

| skill | read it when |
|---|---|
| `00-start-here` | always, first |
| `game-brief` | before any code exists |
| `game-loop` | setting up the update/render loop |
| `game-feel` | the game works but feels dead |
| `game-audio` | **you need sound and have no sound files** |
| `game-vfx` | particles, hit flashes, blood, screen effects |
| `game-touch` | mobile input, virtual sticks, thumb zones |
| `game-save` | persistence, and the patch that comes after launch |
| `game-economy` | currency, a store, IAP, without dark patterns |
| `game-perf` | it drops frames on a real phone |
| `game-art` | you need art and cannot draw |
| `game-ship` | web build → signed Android bundle → store listing |
| `game-verdict` | before claiming anything is done |

## Laws

These are not style preferences. Breaking them produces a game that fails in a specific, predictable way.

1. **The brief precedes the code.** An agent that starts coding from a one-line prompt builds a demo,
   not a game.
2. **Fixed timestep for simulation, interpolated render.** Anything else desyncs and breaks physics on
   slow devices.
3. **Never play the same sound twice identically.** Pitch-randomise every playback. This single rule is
   the difference between "has audio" and "sounds good".
4. **Hitstop before particles.** If you can only add one effect, freeze the frame on impact.
5. **The save file has a version number from the very first commit.** Retrofitting migration is how
   hobby games die at their first update.
6. **44px minimum touch target, nothing in the top 15% of the screen.** A hand cannot reach there.
7. **Test on a real mid-range phone, not on your laptop.** The laptop lies about everything.
8. **No dark patterns.** See `skills/game-economy` for what that means concretely and why the honest
   version also converts better.

## What this kit will not do

It will not generate a game from a prompt. It will not make aesthetic decisions for you. It will not
tell you your game is good — only that it is ready to be judged.

MIT. Take it, fork it, strip it.
