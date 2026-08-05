---
name: game-ai
description: Use when building enemies, companions, NPCs or opponents. Pathfinding, state machines, behaviour trees, utility AI ... and why an LLM is almost never the right answer for moment-to-moment behaviour.
---

# Game AI

**Game AI is not machine learning and it is almost never an LLM.** It is a small set of well-understood
techniques for making something look like it is deciding, at a cost of microseconds.

⚠ **The goal is not intelligence. It is legibility.** The player must be able to predict what an enemy
will do, be surprised when it does something better, and never feel cheated. An opponent that plays
perfectly is not fun; an opponent whose reasoning you can read is.

---

## Pick the simplest that works

### 1. State machine ... start here, always
Idle → Patrol → Chase → Attack → Flee. Explicit states, explicit transitions.

```ts
type State = 'idle' | 'patrol' | 'chase' | 'attack' | 'flee'
function update(e: Enemy, dt: number) {
  switch (e.state) {
    case 'patrol': if (canSee(e, player)) e.state = 'chase'; break
    case 'chase':  if (inRange(e, player)) e.state = 'attack'
                   else if (!canSee(e, player)) e.state = 'patrol'; break
    case 'attack': if (e.hp < e.maxHp * 0.2) e.state = 'flee'; break
  }
}
```

⚠ **90% of games never need more than this.** It is debuggable, predictable, and you can print the
state on screen.

### 2. Behaviour tree ... when the FSM has too many transitions
Composable Sequence/Selector/Condition/Action nodes. Reach for it when your FSM has >8 states or
transitions are duplicating.

### 3. Utility AI ... when the choice is "which of these is best right now"
Score every possible action, take the highest. Excellent for companions, sims, and strategy.

```ts
const options = [
  { act: 'attack', score: nearbyEnemies * 0.4 + (myHp / max) * 0.3 },
  { act: 'heal',   score: (1 - myHp / max) * 0.9 },
  { act: 'flee',   score: (1 - myHp / max) * threat * 0.7 },
]
const best = options.sort((a, b) => b.score - a.score)[0]
```

⚠ **Add a small random jitter to the scores.** Perfectly optimal agents read as robotic, and the
jitter is what makes them feel like they have a personality.

### 4. GOAP / planners ... rarely worth it
Only for genuinely emergent sims. High complexity, hard to debug, hard to make legible.

---

## Pathfinding

- **Grid → A\*.** `rot.js` (BSD-3) has it, and dungeon generation and FOV too.
- **Open 2D/3D → navmesh** or flow fields.
- ⚠ **Many enemies to one target → flow field, not A\* per entity.** One field, computed once,
  serves 500 entities. Per-entity A\* at that count is your frame budget, gone.
- **Always cache.** Recompute on a timer or on obstacle change, never every frame.

---

## Making it feel fair

These are the difference between a good enemy and a hated one:

- **Telegraph everything.** A wind-up before every attack. ⚠ **An unavoidable attack is a bug**, even
  when it is technically dodgeable, if the player could not have known.
- **React visibly to state changes.** An enemy noticing you must look like noticing.
- **Give reaction time.** ~200ms between the player entering view and the enemy acting. Instant
  reaction reads as cheating.
- **Miss on purpose.** Ranged enemies at full accuracy are miserable. Miss the first shot; tighten
  from there.
- **Coordinate, but let some wait.** Six enemies attacking simultaneously is unreadable. Use an
  attack-token system: only 1-2 hold a token at a time; the rest circle.
- ⚠ **Never scale difficulty invisibly.** Rubber-banding that the player can feel but not see is the
  single most-hated mechanic in games, and players always eventually notice.

---

## Where an LLM genuinely fits

**Not** in the update loop. Latency is 200ms-2s, cost is real, and output is non-deterministic ... three
disqualifying properties for per-frame behaviour.

**But it is excellent at authoring time and at the edges:**
- Generating dialogue, barks, item descriptions, quest text ... **at build time, reviewed, then baked.**
- NPC conversation that is genuinely open-ended, out of combat, with a latency budget.
- Designer tooling: generating encounter layouts or tuning tables for a human to approve.

⚠ **If an LLM output can affect game state, validate it against a schema and clamp it.** A model that
can write to your simulation is a model that can break it, and it will.

---

## Checklist

- [ ] Simplest technique that works ... FSM before tree before utility
- [ ] State inspectable on screen in a debug mode
- [ ] Every attack telegraphed
- [ ] Reaction delay on player detection
- [ ] Pathfinding cached; flow field if the count is high
- [ ] Attack tokens so enemies do not all act at once
- [ ] Jitter in any scoring, so behaviour is not robotic
- [ ] ⚠ No invisible difficulty scaling
