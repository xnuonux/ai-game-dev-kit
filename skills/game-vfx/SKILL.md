---
name: game-vfx
description: Use when a game needs particles, hit flashes, blood, screen effects or shaders. Covers the particle stack, gore that reads well at phone size, decals, post-processing, and what to build versus what to import.
---

# VFX

**Rule zero: VFX is not decoration, it is feedback.** Every effect answers a question the player is
asking — did that hit? did I take damage? is that thing dangerous? An effect that answers nothing is
a frame-rate cost with a pretty face.

If you only build one thing from this file, build the **hit flash**. It is six lines and it does more
than a particle system.

---

## The stack

| need | use | licence |
|---|---|---|
| Particles (PixiJS) | `@pixi/particle-emitter` | MIT |
| Particles (Phaser/Excalibur) | **engine built-in** — do not add a library | — |
| Confetti / celebration | `canvas-confetti` | ISC |
| Tweening | `@tweenjs/tween.js` | MIT |
| Screen shake, hitstop, flash | **hand-rolled, ~40 lines** — see below | — |
| Shaders | write GLSL by hand | — |
| Post-processing (3D only) | `postprocessing` (pmndrs) | MIT |

⚠ **`lygia` is on the shelf and its licence is NOT plain MIT.** Read it before shipping a single line
of it in a commercial game.

⚠ **Do not import a particle library your engine already has.** Phaser and Excalibur both ship
competent emitters. Adding a second one is bundle weight and a second API to learn.

---

## The four effects that matter, in order

### 1. Hit flash — build this first

The single highest-value visual effect in any action game. On damage, tint the sprite pure white for
~60ms. It reads at any size, on any background, at any frame rate.

```ts
function flash(sprite: { tint: number }, ms = 60) {
  const original = sprite.tint
  sprite.tint = 0xffffff
  setTimeout(() => { sprite.tint = original }, ms)
}
```

### 2. Hitstop — the illusion of weight

Freeze the whole simulation for 60-120ms on a significant impact. Costs nothing, and it is why some
games feel heavy and others feel like sliding stickers.

```ts
let stopUntil = 0
export const hitstop = (ms: number) => { stopUntil = performance.now() + ms }
export const frozen = () => performance.now() < stopUntil
// in the loop: if (frozen()) { render(); return }  // render, do NOT simulate
```

⚠ **Scale it.** A small hit gets 40ms, a boss kill gets 150ms. Constant hitstop reads as lag.
⚠ **Silence the audio during hitstop** (`skills/game-audio`).

### 3. Screen shake — trauma-based, never additive

Naive shake adds an offset per event and turns to nausea when ten things explode. Use a **trauma**
value that decays, and square it so small events are subtle and big ones are violent.

```ts
let trauma = 0
export const addTrauma = (amount: number) => { trauma = Math.min(1, trauma + amount) }

export function shakeOffset(dt: number, maxPx = 24) {
  trauma = Math.max(0, trauma - dt * 1.8)     // decay
  const s = trauma * trauma                    // squared: perceptually correct
  return {
    x: (Math.random() * 2 - 1) * maxPx * s,
    y: (Math.random() * 2 - 1) * maxPx * s,
  }
}
```

⚠ Offer a **reduce-shake** accessibility setting. Some players get motion sick, and the review will
say so.

### 4. Particles — last, and sparingly

```ts
emitter.emit({
  count: 12,                 // 12 good particles beat 200 cheap ones
  speed: [80, 220],          // ranges, never a constant
  life: [0.25, 0.6],
  gravity: 900,
  scale: [1, 0],             // ALWAYS shrink to zero; particles that vanish mid-size look broken
  alpha: [1, 0],
})
```

**Rules:** every value is a range · always fade AND shrink out · pool aggressively, never allocate
mid-frame · cap the global particle count and drop oldest.

---

## Gore that reads at phone size

The cute-brutal look depends entirely on **contrast and persistence**, not on detail. Nobody can see
detail on a 6-inch screen with 300 entities.

**The three layers:**

1. **The burst** — 8-16 particles, high initial speed, gravity, ~400ms. Reads as violence.
2. **The decal** — a splat drawn to a **persistent render texture** that is never cleared. This is
   the whole trick: the floor accumulates, and a screenshot at minute ten tells the entire story.
3. **The drip** — occasional slow particles from a decal edge. Cheap, and it makes the scene feel wet
   rather than stamped.

```ts
// the decal layer: one texture, drawn once, never cleared, composited under entities
const decals = renderer.createRenderTexture(W, H)
function splat(x: number, y: number, size: number, tint: number) {
  renderer.renderToTexture(decals, splatSprite(x, y, size, tint), { clear: false })
}
```

⚠ **Decals are the cheapest high-impact effect in this genre and almost nobody does them**, because
it requires thinking about a render target instead of spawning more particles.

⚠ **Colour is the tone dial.** The exact same system reads as candy with pink and magenta, and as
horror with dark red. Decide deliberately — it drives your store rating (`skills/game-ship`).

---

## Shaders worth writing by hand

- **White flash** — usually achievable with tint; only write a shader if you need partial flash.
- **Dissolve** — noise threshold against a rising cutoff. The best death effect for soft characters.
- **Outline** — sample 4-8 neighbours, draw where alpha differs. Essential for readability when the
  screen is full.
- **Chromatic aberration** — offset R and B by a few pixels on damage. ⚠ Two frames only. Any longer
  and it reads as a broken display.

---

## Performance, which is where VFX goes wrong

- **Pool everything.** Allocating particles mid-frame causes GC pauses, and a GC pause during combat
  is the exact moment it is most visible.
- **One draw call per effect type.** Batch by texture.
- **Cap by device.** Read `matchMedia('(pointer:coarse)')` and halve the particle budget.
- **Additive blending is expensive on mobile GPUs.** Use it for light, not for everything.
- **Measure with 400 entities on a mid-range Android**, not on your laptop. See `skills/game-perf`.

---

## Checklist

- [ ] Hit flash on every damage event
- [ ] Hitstop scaled to impact size, with audio silenced
- [ ] Trauma-based shake with a reduce-motion setting
- [ ] Particles pooled, capped, and fading + shrinking
- [ ] Decals persistent and composited under entities
- [ ] Measured at target entity count on real hardware
