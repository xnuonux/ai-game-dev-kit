---
name: game-perf
description: Use when a game drops frames, heats the device, or needs to hold a target entity count. Mobile GPU budgets, the allocation trap, and how to measure instead of guess.
---

# Performance

**Your laptop lies about everything.** It has more GPU, more thermal headroom, and a fan. A game that
runs at 60fps in your browser can run at 19fps on the phone you are shipping to, and you will not
find out until a review says so.

⚠ **Measure on a real mid-range Android.** Not a flagship, not an emulator, not Chrome with device
mode on. A three-year-old £200 phone is the honest target.

---

## The budgets

| target | budget per frame | note |
|---|---|---|
| 60fps | 16.6ms | desktop, flagship phones |
| **30fps** | **33.3ms** | ⚠ the correct default for mobile |
| your actual budget | ~⅔ of the above | the browser and OS take the rest |

**A locked 30 beats an oscillating 45.** And sustained 60 on mobile makes the device hot, and a hot
device throttles — so a game that runs at 60 for four minutes and 24 forever after is *worse* than
one that runs at 30 always. See `skills/game-loop`.

---

## The three things that are actually slow

### 1. Allocation → garbage collection

⚠ **This is the number one cause of stutter in JS games and it does not show up as a low average
frame rate** — it shows as a 40ms spike at the worst possible moment.

```ts
// BAD: allocates every frame, for every entity
function update(e) { e.pos = { x: e.pos.x + e.vel.x, y: e.pos.y + e.vel.y } }

// GOOD: mutate in place
function update(e) { e.x += e.vx; e.y += e.vy }
```

**Never allocate in the update loop.** No object literals, no `.map`/`.filter`/`.slice` per entity, no
closures, no template strings. Pool everything with a lifecycle: particles, bullets, enemies, damage
numbers, vectors.

### 2. Draw calls

Every texture switch is a draw call. **Batch by texture** — one atlas per layer (`free-tex-packer`).
Going from 400 draw calls to 12 is routinely a 3x frame-rate win and costs one build step.

### 3. Overdraw

Mobile GPUs are fill-rate bound. Full-screen transparent layers stacked on each other will kill you
faster than any amount of game logic. ⚠ **Additive blending is expensive** — use it for light, not for
everything.

---

## Scale to the device

```ts
const coarse = matchMedia('(pointer:coarse)').matches
const BUDGET = {
  particles: coarse ? 300 : 1200,
  dpr:       Math.min(devicePixelRatio || 1, coarse ? 1.5 : 2),
  shadows:   !coarse,
  decals:    coarse ? 200 : 800,
}
```

⚠ **Degrade automatically.** If the rolling average frame time exceeds budget for two seconds, halve
the particle cap. Recover slowly. The player must never see a settings menu about this.

---

## Measure

```ts
import Stats from 'stats.js'
const stats = new Stats(); document.body.appendChild(stats.dom)
// stats.begin() at the top of the frame, stats.end() at the bottom
```

**Real device profiling:** `chrome://inspect` from a desktop with the phone on USB gives you the full
performance panel against the actual hardware. This is the only measurement that counts.

**What to look for:** not the average. ⚠ **The 99th percentile frame time and the spike pattern.** An
average of 16ms with a 60ms spike every two seconds is a game that feels broken.

---

## The order of investigation

⚠ **Profile before optimising.** Every time an agent guesses at a performance cause it is wrong, and
the fix is a week of work on the wrong thing.

1. Is it GC? → look for sawtooth memory, then hunt allocations.
2. Is it draw calls? → count them.
3. Is it fill rate? → shrink the canvas by half. If it doubles, you are fill-bound.
4. Is it logic? → *now* look at your code.

**Measure, change one thing, measure again.**

---

## Checklist

- [ ] Profiled on a real mid-range Android, not a laptop
- [ ] Zero allocation in the hot loop, verified in the memory panel
- [ ] Draw calls counted and batched
- [ ] DPR capped on touch
- [ ] Automatic degradation on sustained overrun
- [ ] ⚠ 99th percentile frame time recorded, not just the average
- [ ] Ten-minute thermal test: frame rate at minute 10 vs minute 1
