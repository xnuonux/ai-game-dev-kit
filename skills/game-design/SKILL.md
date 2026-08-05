---
name: game-design
description: Use when tuning difficulty, progression, pacing or economy curves. The maths behind why a game feels well-paced, and the specific curves that work.
---

# Design maths

Most "the difficulty is wrong" problems are one bad curve, and most bad curves are the same three
mistakes. This file is the maths, because tuning by feel without knowing the shape you are aiming at
is how a game ends up impossible at wave 12 and trivial at wave 40.

---

## Difficulty

**The target is a sawtooth, not a ramp.** Rising tension, then relief, then rising from a higher
floor. A monotonic ramp is exhausting; a flat line is boring.

```
     /|  /|   /|
    / | / |  / |
   /  |/  | /  |
  ────────────────>  every 4-8 encounters, drop the floor and rise again
```

**Growth: geometric, not linear.**

```ts
const enemyHp = base * Math.pow(1.12, wave)     // ~12% per wave
const reward  = base * Math.pow(1.10, wave)     // ⚠ rewards grow SLOWER than threat
```

⚠ **Rewards must grow slower than difficulty**, or the game trivialises itself by wave 30. The gap is
where upgrades live.

**Spike rule: never more than ~1.5x the previous step.** A 3x jump reads as a wall, and the player
blames the game rather than themselves. ⚠ Plot your actual curve and look for the spike ... it is
almost always there and almost always invisible in the code.

**Boss cadence: every 5-10 encounters.** It is the sawtooth's peak and the memory the player keeps.

---

## Progression

**The right shape is a logarithm.** Fast early, slowing forever, never stopping.

```ts
const xpForLevel = (n: number) => Math.floor(100 * Math.pow(n, 1.5))
// 1→100, 5→1118, 10→3162, 20→8944, 50→35355
```

**Time-to-first-upgrade under 90 seconds.** ⚠ This single number predicts retention better than
almost anything else you can measure. If the first reward is at minute five, most people never see it.

**Every upgrade must be visible.** +5% damage that changes nothing on screen is not a reward. Change
the sprite, the sound, the particle colour ... **something the player can see happened.**

**Prestige/reset loops:** the multiplier must make the first ten minutes of the next run **faster than
the last run's first ten minutes.** If a reset feels like a punishment, nobody resets twice.

---

## Pacing

| session | rule |
|---|---|
| **90 seconds** | ⚠ no menus, no tutorial, one decision. |
| **5 minutes** | one arc: build, peak, resolve. A reward at the end, always. |
| **30 minutes** | ⚠ needs a save, a pause, and 3-4 sub-arcs. Nobody plays 30 minutes in one sitting on a phone, whatever the design doc says. |

**The first 90 seconds carries disproportionate weight.** First input in under 5 seconds. First
feedback instantly. First meaningful decision by ~30s. First reward by ~90s.

⚠ **Most games are lost here** and it is the cheapest part to fix once it is visible. → `game-verdict`.

---

## Economy curves

**Sinks must exceed faucets, gently.** A currency that accumulates faster than it can be spent stops
being a currency and becomes a number.

**Price the next upgrade at 1.5-2x the last.** Under 1.5x and progression is meaningless; over 2.5x
and the wall arrives.

**Soft cap, never hard cap.** Costs that keep rising beat a level 50 ceiling with nothing after it.

→ `game-economy` for monetisation specifically.

---

## Randomness that does not feel unfair

⚠ **True random feels rigged**, because humans are terrible at intuiting streaks and always suspect
the game.

- **Pity timers.** Guarantee a drop after N failures. Almost every good game does this quietly.
- **Bags, not dice.** Draw without replacement and reshuffle ... a "1 in 5" that actually delivers once
  per five.
- **Weight against repeats.** Halve the weight of whatever dropped last.
- **Always seed and store the seed.** → law 7. It makes bugs reproducible and runs shareable.

---

## The measurements that tell you it is wrong

- **Where do people stop?** One wave/level with a cliff in the retention curve is a spike. Fix that one.
- **Time to first upgrade.** Over 90s, fix it.
- **Session length vs the brief.** Diverging by 2x means the brief or the game is wrong. Say which.
- **Deaths per encounter.** ⚠ A step where this triples is a spike you did not intend.

**Plot the curve. Do not tune by feel alone.** An agent tuning by feel is tuning by nothing, because
it cannot feel anything ... plot it, look at the shape, and hand the shape to a human.
