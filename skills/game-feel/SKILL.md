---
name: game-feel
description: Use when a game is mechanically correct but feels dead, floaty, or cheap. The ~200 lines that separate a prototype from a product, in priority order.
---

# Game feel

A game that works and a game that feels good are separated by roughly 200 lines of code, and an agent
will never write them unprompted because none of them are required for the game to function.

**This is that list, in the order that buys the most feel per line.**

---

## The order (do not reorder)

1. **Hit flash** ... white tint, 60ms, on every damage event. → `skills/game-vfx`
2. **Hitstop** ... freeze 60-120ms on impact, silence audio. → `skills/game-vfx`
3. **Pitch-randomised SFX** ... never the same sound twice. → `skills/game-audio`
4. **Screen shake** ... trauma-based, squared, decaying.
5. **Squash and stretch** ... scale on land/jump/hit. Below.
6. **Knockback** ... everything hit moves, even slightly.
7. **Anticipation** ... a 60-120ms wind-up before any big action.
8. **Particles** ... last, and few.

⚠ **Numbers 1-3 are 80% of the result.** If you are out of budget, stop after three.

---

## Squash and stretch

Volume-preserving scale. Land = wide and short. Jump = tall and thin. Get hit = a quick pulse.

```ts
function squash(t: number, amount = 0.3, ms = 150) {
  const p = Math.min(1, t / ms)
  const e = 1 - Math.pow(1 - p, 3)              // easeOutCubic
  const s = 1 + amount * (1 - e) * Math.sin(p * Math.PI * 2)
  return { x: 1 / s, y: s }                      // preserve volume
}
```

## Coyote time and input buffering

Two forgiveness windows that players never notice and always feel.

```ts
const COYOTE_MS = 100   // still jumpable this long after leaving a ledge
const BUFFER_MS  = 120  // a jump pressed this early still fires on landing
```

⚠ **Without these, precise platforming feels broken and the player blames themselves, then quits.**
Every good platformer has both. Almost no first-draft platformer does.

## Easing, and the one curve to default to

Never move anything linearly. `easeOutCubic` for arrivals, `easeOutBack` for anything that should feel
snappy or playful.

```ts
const easeOutCubic = (t: number) => 1 - Math.pow(1 - t, 3)
const easeOutBack  = (t: number) => 1 + 2.7 * Math.pow(t - 1, 3) + 1.7 * Math.pow(t - 1, 2)
```

## Numbers that pop

Damage numbers rising and fading are one of the cheapest dopamine mechanisms in games. Randomise the
horizontal drift so a burst does not stack into an unreadable column.

## Camera

- **Lead the movement.** Offset toward where the player is heading, ~15% of the screen.
- **Lerp, never snap.** `cam += (target - cam) * (1 - Math.pow(0.001, dt))` ... framerate-independent.
- **Punch on impact:** briefly zoom in ~2% and back out. Almost invisible, enormously effective.

## Haptics (mobile)

One short pulse on impact, via Capacitor. ⚠ **Sparingly** ... constant vibration drains battery and gets
turned off, taking your good hits with it. And ship a setting.

---

## The anti-patterns

- **Everything shakes.** If every event shakes, nothing feels big. Reserve it.
- **Hitstop on every hit.** Small hits get 0-40ms. Constant freeze reads as lag, not weight.
- **Particles instead of feedback.** A hundred particles cannot fix a hit that has no flash and no stop.
- **Linear motion.** Instantly reads as programmer art, even to people who cannot say why.
- **Effects that outlive their cause.** Every effect ends before the next one starts, or the screen
  becomes noise.

---

## The test

Record 20 seconds of gameplay. Watch it with the sound off. **Can you tell which frames are impacts?**

If not, the visual feedback is not doing its job, and no amount of audio will save it.
