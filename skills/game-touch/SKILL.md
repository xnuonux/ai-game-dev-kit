---
name: game-touch
description: Use when building mobile controls. Thumb zones, tap targets, virtual sticks, and the browser defaults that will silently ruin touch input if you do not disable them.
---

# Touch

The single biggest reason agent-built mobile games feel wrong is input. The logic is fine; the hand
holding the phone was never considered.

---

## The four rules

**1. 44px minimum tap target.** Actual CSS pixels, not "it looks big enough on the laptop".

**2. Nothing important in the top 15% of the screen.** A thumb cannot reach there on a modern phone
without regripping, and regripping mid-action is a lost input.

**3. The bottom third is the only comfortable zone.** Primary controls live there. On a 6.7" phone
held one-handed, the comfortable arc is a rough circle centred near the bottom corner on the thumb
side.

**4. Design for one thumb until proven otherwise.** Two-thumb landscape is a real choice, but it must
be a *choice* in the brief, not a default that emerges from desktop testing.

---

## Kill the browser defaults first

⚠ **Do this before anything else.** Without it: double-tap zooms mid-game, long-press opens a context
menu, and a vertical drag scrolls the page instead of aiming.

```css
canvas, .game-root {
  touch-action: none;            /* the important one */
  user-select: none;
  -webkit-user-select: none;
  -webkit-touch-callout: none;   /* no long-press menu on iOS */
  -webkit-tap-highlight-color: transparent;
}
html, body { overscroll-behavior: none; overflow: hidden; }
```

```html
<meta name="viewport" content="width=device-width, initial-scale=1,
      maximum-scale=1, user-scalable=no, viewport-fit=cover">
```

⚠ **Pointer Events, never touch events.** One code path for mouse, touch and pen.

```ts
el.addEventListener('pointerdown', onDown, { passive: false })
el.addEventListener('pointermove', onMove, { passive: false })
el.addEventListener('pointerup',   onUp)
el.addEventListener('pointercancel', onUp)   // ⚠ forgetting this leaves inputs stuck ON
```

---

## The verbs

**Tap** — under 200ms, under 10px movement. Anything else is a drag.

**Virtual stick (`nipplejs`)** — floating, not fixed. ⚠ **A fixed stick is the most common mobile
control mistake**: the thumb must find it. A floating stick appears wherever the thumb lands.

**Drag-to-aim, release-to-fire** — the best touch combat verb there is. It shows the trajectory while
aiming, and the finger is not covering the target at the moment of firing.

**Hold** — needs visible progress feedback within 150ms or it reads as an unresponsive tap.

⚠ **Avoid swipe-direction gestures for anything time-critical.** Recognition needs ~100ms of travel,
which is an eternity in an action game.

---

## Feel

**Input latency budget: under 50ms** from touch to visible response. Above ~80ms the controls feel
"laggy" even when the framerate is perfect.

**Respond on `pointerdown`, not `pointerup`.** Waiting for release adds the entire duration of the
press to your perceived latency.

**Touch slop.** Allow ~10px of movement before a tap becomes a drag — fingers are not precise and
they move during a press.

**Haptics on confirm.** A 10ms pulse via Capacitor. ⚠ Sparingly, and with a setting.

---

## Safe areas

Notches, home indicators and rounded corners eat your UI.

```css
.hud {
  padding-top: env(safe-area-inset-top);
  padding-bottom: max(16px, env(safe-area-inset-bottom));
}
```

---

## The test

⚠ **Hand the phone to someone and watch their hand, not the screen.**

If they regrip, a control is out of reach. If they use a second hand you did not design for, your
layout assumed a desk. If they tap twice because the first did nothing, your latency or your target
size is wrong.

- [ ] `touch-action: none` and viewport meta set
- [ ] `pointercancel` handled
- [ ] All targets ≥44px, none in the top 15%
- [ ] Virtual stick floats
- [ ] Response on down, under 50ms, measured
- [ ] Safe areas respected on a notched device
