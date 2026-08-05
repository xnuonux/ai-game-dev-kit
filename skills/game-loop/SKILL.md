---
name: game-loop
description: Use when setting up the update/render loop. Fixed timestep, interpolated render, and the mobile-specific truths that break naive loops on real phones.
---

# The loop

Almost every hand-written game loop is wrong in the same way: it multiplies by `deltaTime` and hopes.
That works until the frame rate drops, and then physics tunnels, collisions miss, and the game
becomes non-deterministic.

**Fixed timestep for simulation. Interpolated render. Nothing else.**

---

## The loop

```ts
const STEP = 1 / 60          // simulation runs at exactly 60Hz, always
const MAX_STEPS = 5          // never spiral: drop time instead of catching up forever

let acc = 0
let last = performance.now()

function frame(now: number) {
  requestAnimationFrame(frame)

  // 1. a hidden tab must not simulate
  if (document.hidden) { last = now; return }

  // 2. mobile frame budget: 30fps on touch devices is a deliberate choice, not a failure
  if (MIN_FRAME_MS && now - lastDraw < MIN_FRAME_MS) return

  // 3. hitstop renders but does not simulate
  if (frozen()) { render(1); lastDraw = now; return }

  let dt = (now - last) / 1000
  last = now
  if (dt > 0.25) dt = 0.25              // returning from background: clamp, never replay

  acc += dt
  let steps = 0
  while (acc >= STEP && steps < MAX_STEPS) { simulate(STEP); acc -= STEP; steps++ }
  if (steps === MAX_STEPS) acc = 0      // give up on the backlog rather than death-spiral

  render(acc / STEP)                     // alpha for interpolation
  lastDraw = now
}
requestAnimationFrame(frame)
```

## The mobile truths

**1. `visibilitychange` is not optional.** A phone locks, a call arrives, the user switches apps. On
return you get one enormous delta. Without the clamp the game simulates ten minutes in one frame and
the player dies instantly to something they never saw.

```ts
document.addEventListener('visibilitychange', () => {
  if (!document.hidden) { last = performance.now(); acc = 0 }
})
```

**2. Cap DPR on touch devices.** A 3x device pixel ratio on a phone is 9x the fragment work for a
difference nobody can see at arm's length.

```ts
const coarse = matchMedia('(pointer:coarse)').matches
const dpr = Math.min(devicePixelRatio || 1, coarse ? 1.5 : 2)
```

**3. 30fps is a legitimate target on mobile.** A locked, stable 30 feels better than a 45 that
oscillates, and it roughly halves thermal load. ⚠ Sustained full-rate rendering makes the phone hot,
and a hot phone throttles ... so a game that runs at 60 for four minutes and 24 forever after is
*worse* than one that runs at 30 always.

**4. `requestAnimationFrame` timing lies.** Never derive game time from frame count.

**5. Pause when it matters.** Losing focus mid-run in a game with permadeath should pause, not kill.

---

## Interpolation

Render between simulation steps or 60Hz sim on a 120Hz screen will judder.

```ts
const drawX = prevX + (x - prevX) * alpha
```

Store `prevX/prevY` at the top of each `simulate()`. Two lines, and it is the difference between
smooth and stuttery on high-refresh phones ... which is most phones now.

---

## Checklist

- [ ] Fixed timestep, `MAX_STEPS` guard, no spiral
- [ ] Delta clamped on return from background
- [ ] `document.hidden` skips simulation entirely
- [ ] DPR capped on touch
- [ ] Interpolated render with stored previous positions
- [ ] Verified: lock the phone mid-run, return, nothing has died
