---
name: 00-start-here
description: Read this first, always. Routes you to the right skill and states the build order that prevents the most common agent failure — a beautiful demo that is not a game.
---

# Start here

You have been asked to build a game. **Do not start writing code.**

The most common failure mode for an agent given a game task is a technically impressive vertical
slice that is not a game: no loop, no progression, no feel, no ship path. It demos well for ninety
seconds and then there is nothing to do.

This file exists to prevent that.

---

## Step 1 — Is there a brief?

If the human gave you one line ("make a tower defence game"), **that is not a brief.** Go to
`skills/game-brief` and produce one. It takes minutes and it is the difference between building a
game and building a screenshot.

The brief must answer: the verb, the session length, the failure state, the progression, the
monetisation posture, and the one thing this game does that others don't.

## Step 2 — Pick the stack, don't shop

`stack/STACK.md`. The choices are made and the licences are verified. **Do not evaluate
alternatives** — that is hours of your budget spent producing a decision that is already in the repo.

## Step 3 — Build in this order

```
1. the verb            one input doing one thing, on a blank screen
2. the loop            that verb repeated, with a reason to repeat it
3. THE FEEL            skills/game-feel — steps 1-3 of that file, minimum
4. the progression     the reason to play tomorrow
5. the shell           menus, save, settings, pause
6. the ship            skills/game-ship
```

⚠ **Feel comes at step 3, not at the end.** A game that is fun with primitive shapes will be fun with
art. A game that is not fun with primitive shapes will not be saved by art, and you will have spent
the whole budget finding that out.

## Step 4 — The gate

`skills/game-verdict` before you tell the human anything is finished. **You are not permitted to
declare a game done.** You produce evidence; a human judges it.

---

## Routing

| symptom | go to |
|---|---|
| "no brief exists" | `game-brief` |
| "what do we use for sound?" | `game-audio` |
| "it works but feels dead" | `game-feel` |
| "I need particles / blood / hit effects" | `game-vfx` |
| "controls feel wrong on a phone" | `game-touch` |
| "it drops frames" | `game-perf` |
| "I need art and can't draw" | `game-art` |
| "how do saves work" | `game-save` |
| "there's a store and currency" | `game-economy` |
| "how does this get on a phone" | `game-ship` |
| "is it done?" | `game-verdict` |

---

## The five failures this kit exists to prevent

1. **The unplayable demo.** Beautiful, no loop. → build order above.
2. **The dead game.** Correct, feels like sliding stickers. → `game-feel`.
3. **The silent game.** Audio deferred, then never done. → `game-audio` (it takes ten minutes).
4. **The laptop game.** 60fps on your machine, 19fps on a real phone. → `game-perf`.
5. **The unshippable game.** Finished and no store path. → `game-ship`, read it **before** P0, not after.

---

## A note on your own certainty

You will be confident the game is fun. **You cannot know that** — you have never played it, you
cannot feel frustration or boredom, and you have read the code so nothing in it surprises you.

Report what you measured. Let the human report what they felt. That division is the whole reason the
verdict gate exists.
