# Genres

**Each entry gives: the verb, the P0 that proves the game exists, the specific way this genre fails,
and which skills matter most.**

⚠ **The P0 is the important column.** It is the smallest thing that answers "is this fun", and if the
answer is no, everything after it is wasted. Build exactly that, judge it, then continue.

---

## Action

| genre | verb | P0 ... build exactly this | ⚠ how it fails | key skills |
|---|---|---|---|---|
| **Platformer** | jump | one character, one gap, one enemy. **Movement must be fun with nothing to do.** | floaty jump; no coyote time | `game-feel` `game-input` |
| **Shoot-em-up / bullet hell** | dodge | one ship, one enemy pattern, 60 seconds | unreadable at density | `game-vfx` `game-perf` |
| **Arena survivor** | move | auto-fire, one arena, six minutes escalating | flat upgrade choices | `game-design` `game-perf` |
| **Fighting** | commit | two characters, three moves, hitboxes | netcode. 🚨 rollback or nothing | `game-net` `game-feel` |
| **Beat-em-up** | combo | one enemy, three-hit chain, hitstop | no crowd control | `game-feel` `game-ai` |
| **FPS / TPS** | aim | one room, one weapon, one enemy | aim feel; ⚠ recoil and hit registration | `game-input` `game-net` |

## Strategy and thinking

| genre | verb | P0 | ⚠ how it fails | key skills |
|---|---|---|---|---|
| **Tower defence** | place | one lane, three towers, ten waves | ⚠ one dominant strategy | `game-design` `game-ai` |
| **Turn-based tactics** | decide | 3v3 on one grid, one full battle | AI too good or too dumb | `game-ai` `game-design` |
| **RTS** | manage | five units, one order, pathfinding that works | ⚠ pathfinding. it is always pathfinding | `game-ai` `game-net` |
| **Auto-battler** | compose | eight units, one auto-resolve | units interchangeable | `game-design` |
| **Puzzle** | solve | ten hand-made levels, no generator | ⚠ difficulty cliff at ~level 8 | `game-design` |
| **Deckbuilder** | draft | 20 cards, one run, real synergies | no meaningful choices | `game-design` |

## Systems and long-form

| genre | verb | P0 | ⚠ how it fails | key skills |
|---|---|---|---|---|
| **Roguelite** | risk | one floor, five items, permadeath, meta-unlock | ⚠ items that are not build-defining | `game-design` `game-ai` |
| **RPG** | grow | one quest, one fight, one level-up, one save | scope. **always scope.** | `game-save` `game-design` |
| **Survival / crafting** | gather | gather → craft → survive one night | busywork without tension | `game-design` `game-save` |
| **City / base builder** | arrange | ten tiles, one resource, one constraint | no failure pressure | `game-design` |
| **Sim / management** | tune | one system with a visible feedback loop | opaque simulation | `game-design` |
| **Idle / incremental** | wait | tap → automate → prestige, first 40 min tuned | ⚠ punishing absence | `game-design` `game-save` |
| **Merge** | drag | one board, six tiers, one consequence | no tension, ever | `game-design` |

## Feel-led

| genre | verb | P0 | ⚠ how it fails | key skills |
|---|---|---|---|---|
| **Rhythm** | time | one track, one input, the audio reward | 🚨 **audio latency.** solve first or stop | `game-audio` `game-feel` |
| **Racing** | steer | one car, one corner, the handling model | handling; nothing else matters | `game-feel` `game-input` |
| **Physics toy** | ruin | the simulation and three tools | sim quality is the entire product | `game-vfx` `game-perf` |
| **Horror** | fear | one room, one wrong thing, no combat | ⚠ jump scares instead of dread | `game-audio` `game-design` |
| **Narrative / VN** | choose | one scene, one branch that matters | choices that don't | `game-design` `game-a11y` |

## Multiplayer-native

| genre | verb | P0 | ⚠ how it fails | key skills |
|---|---|---|---|---|
| **Async raid** | rob | one base, one 90-second raid, one report | boring raid, or ⚠ purchasable power | `game-net` `game-economy` |
| **Party / local co-op** | shout | four inputs, one screen, one round | ⚠ nobody has four controllers | `game-input` |
| **MMO-lite** | persist | 🚨 **do not.** build async first and prove demand | scope, cost, and empty servers | `game-net` |

---

## Rules that apply across all of them

**Build the P0 exactly. Judge it. Then continue.** ⚠ The most expensive mistake available is building
three genres' worth of systems before finding out the core verb is not fun.

**Every genre's P0 is playable in days, not weeks.** If yours is not, it is not a P0 ... it is the game.

**The failure column is the one to re-read at week three**, when the thing is half-built and something
feels off. It is almost always the listed failure.

**Hybrids work, one genre at a time.** Survivor + deckbuilder, tower defence + roguelite, merge +
horror. ⚠ **Build one genre's P0 first and prove it**, then graft the second. Building both at once
produces something that is bad at both.
