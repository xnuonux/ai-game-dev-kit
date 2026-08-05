---
name: 00-start-here
description: Read this first, always. Routes you through core → track → genre, and states the build order that prevents the most common agent failure ... a beautiful demo that is not a game.
---

# Start here

You have been asked to build a game. **Do not start writing code.**

The most common failure for an agent given a game task is a technically impressive vertical slice that
is not a game: no loop, no progression, no feel, no ship path. It demos well for ninety seconds and
then there is nothing to do.

---

## The order

```
1. CORE     skills/game-brief         ... what are you building
2. TRACK    tracks/TRACKS.md          ... where does it run, 2D or 3D, networked or not
3. GENRE    genres/GENRES.md          ... the shape, and its P0
4. BUILD    verb → loop → FEEL → progression → shell → ship
5. GATE     skills/game-verdict       ... before you say anything is done
```

**Core is what transfers. Track is what expires.** Learn the core once; the track is a lookup.

---

## Step 1 ... the brief

If the human gave you one line, **that is not a brief.** → `skills/game-brief`. Minutes to write, and
it is the difference between building a game and building a screenshot.

## Step 2 ... the track

`tracks/TRACKS.md` decides from three questions: where must it run, 2D or 3D, and **do two people
interact in real time.**

⚠ **That third one cannot be deferred.** Retrofitting real-time multiplayer is a rewrite. Everything
else you can move later.

## Step 3 ... the genre

`genres/GENRES.md`. Take the **P0** column literally ... it is the smallest thing that answers "is this
fun". Build exactly that. Judge it. Then continue.

## Step 4 ... build

```
1. the verb          one input doing one thing, on a blank screen
2. the loop          that verb repeated, with a reason to repeat it
3. THE FEEL          skills/game-feel ... minimum steps 1-3 of that file
4. progression       the reason to play tomorrow
5. the shell         menus, save, settings, pause
6. ship              skills/game-ship
```

⚠ **Feel at step 3.** A game that is fun with primitive shapes will be fun with art. One that is not
will not be saved by art, and you will spend the whole budget finding out.

## Step 5 ... the gate

`skills/game-verdict`. **You cannot declare a game finished.** Produce the evidence; a human judges.

---

## Routing

| symptom | go to |
|---|---|
| no brief exists | `game-brief` |
| which engine / platform? | `tracks/TRACKS.md` |
| what shape is this game? | `genres/GENRES.md` |
| **what do we use for sound?** | `game-audio` |
| it works but feels dead | `game-feel` |
| particles, blood, impact, screen effects | `game-vfx` |
| controls feel wrong | `game-input` |
| enemies are stupid / unfair | `game-ai` |
| **two players** | `game-net` |
| difficulty or progression is off | `game-design` |
| it drops frames | `game-perf` |
| I need art and can't draw | `game-art` |
| saves, and the patch after launch | `game-save` |
| currency, a store, IAP | `game-economy` |
| before shipping, always | `game-a11y` |
| how does this reach players | `game-ship` |
| **before writing engine API code** | `engine-skills` |
| is it done? | `game-verdict` |

---

## The six failures this kit exists to prevent

1. **The unplayable demo.** Beautiful, no loop. → the build order above.
2. **The dead game.** Correct, feels like sliding stickers. → `game-feel`.
3. **The silent game.** Audio deferred, then never done. → `game-audio`, it takes ten minutes.
4. **The laptop game.** 60fps on your machine, 19fps on a real device. → `game-perf`.
5. **The unshippable game.** Finished, no store path. → `game-ship`, read **before** P0.
6. **The rewrite.** Multiplayer added at the end. → `game-net`, decide **now**.

---

## On your own certainty

You will be confident the game is fun. **You cannot know that.** You have never played it, you cannot
feel frustration or boredom, and you have read every line so nothing in it can surprise you.

**Report what you measured. Let the human report what they felt.** That division is the whole reason
the verdict gate exists, and honouring it is what makes the rest of your reporting believable.
