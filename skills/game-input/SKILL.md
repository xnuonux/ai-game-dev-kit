---
name: game-input
description: Use when building controls for any platform. Touch, gamepad and keyboard+mouse, the browser defaults that silently ruin input, and how to support all three at once without three codebases.
---

# Input

Input is the most common reason an otherwise-correct game feels wrong. The logic is fine; the hand
was never considered.

**Architecture rule: never read a device in your game code.** Read an **intent**.

```ts
// BAD ... now every device needs its own gameplay code
if (keys.has('Space') || gamepad.buttons[0].pressed || touchJumpDown) jump()

// GOOD ... devices write intents; gameplay reads intents
if (input.pressed('jump')) jump()
```

⚠ **Do this on day one.** Retrofitting an abstraction layer after gameplay is written means touching
every system, and it is why so many games ship with good keyboard controls and terrible gamepad ones.

```ts
type Intent = 'up'|'down'|'left'|'right'|'jump'|'fire'|'interact'|'pause'
class Input {
  private held = new Set<Intent>(); private justPressed = new Set<Intent>()
  axis2d(): {x:number,y:number} { /* merged from stick / wasd / virtual stick */ }
  down(i: Intent) { return this.held.has(i) }
  pressed(i: Intent) { return this.justPressed.has(i) }
  endFrame() { this.justPressed.clear() }
}
```

---

## Touch

**The four rules**

1. **44px minimum tap target.** Actual CSS pixels.
2. **Nothing important in the top 15% of the screen** ... a thumb cannot reach without regripping, and a
   regrip mid-action is a lost input.
3. **The bottom third is the only comfortable zone.** Primary controls live there.
4. **Design for one thumb** until two is an explicit decision in the brief.

**Kill the browser defaults first.** ⚠ Without this, double-tap zooms mid-game, long-press opens a
context menu, and a vertical drag scrolls the page instead of aiming.

```css
canvas, .game-root {
  touch-action: none;
  user-select: none; -webkit-user-select: none;
  -webkit-touch-callout: none; -webkit-tap-highlight-color: transparent;
}
html, body { overscroll-behavior: none; overflow: hidden; }
```

```html
<meta name="viewport" content="width=device-width, initial-scale=1,
      maximum-scale=1, user-scalable=no, viewport-fit=cover">
```

⚠ **Pointer Events, never touch events**, and **always handle `pointercancel`** ... forgetting it leaves
inputs stuck on.

**Virtual stick:** floating, not fixed (`nipplejs`). ⚠ A fixed stick means the thumb must *find* it;
a floating one appears where the thumb lands.

**Drag-to-aim, release-to-fire** is the best touch combat verb ... the trajectory is visible while
aiming, and the finger is not covering the target at the moment of firing.

**Safe areas:** `env(safe-area-inset-*)`. Notches and home indicators eat UI.

---

## Gamepad

**Mandatory on Steam and console. Not optional.**

```ts
const pads = navigator.getGamepads()
const p = pads[0]
if (p) {
  const lx = deadzone(p.axes[0]), ly = deadzone(p.axes[1])
  if (p.buttons[0].pressed) input.set('jump')
}
function deadzone(v: number, dz = 0.15) {
  return Math.abs(v) < dz ? 0 : (v - Math.sign(v) * dz) / (1 - dz)  // rescale, don't clip
}
```

⚠ **Radial deadzone, not per-axis.** Per-axis deadzones make diagonal movement snap to the cardinals,
which players feel as "the stick is broken".

- **Never poll in an event handler.** Gamepad state is polled once per frame, at the top.
- **Analog is analog.** A stick at 40% should move at 40%. Binarising it is a common and lazy bug.
- **Button prompts must match the connected pad.** ⚠ Showing "A" to a PlayStation player is the
  single most-reported console-port complaint.
- **Rumble** via the Gamepad Haptics API. Sparingly, with a setting.

## Keyboard + mouse

- **Rebindable, always.** ⚠ Hardcoded WASD is an accessibility failure and a review complaint.
- **`event.code`, not `event.key`** ... `code` is physical position, so AZERTY and Dvorak work.
- **Raw mouse delta for camera look**, not accumulated position.
- **Pointer lock** for first-person, and handle losing it gracefully (Esc always releases).

---

## Switching devices live

Players plug in a controller mid-session. Handle it.

```ts
// track the last device that produced input; swap UI prompts on change
window.addEventListener('gamepadconnected', () => scheme = 'gamepad')
window.addEventListener('keydown', () => scheme = 'kbm')
window.addEventListener('pointerdown', e => { if (e.pointerType === 'touch') scheme = 'touch' })
```

⚠ **Change the prompts, not the layout.** A UI that reflows on device change is disorienting.

---

## Feel

- **Under 50ms** touch/press to visible response. Above ~80ms it feels laggy at any frame rate.
- **Respond on down, not up.** Waiting for release adds the whole press duration to perceived latency.
- **~10px touch slop** before a tap becomes a drag.
- **Coyote time and input buffering** → `game-feel`. Without them, precise play feels broken and the
  player blames themselves, then quits.

---

## Checklist

- [ ] Intent layer exists; **no gameplay code reads a device**
- [ ] `touch-action: none`, viewport meta, `pointercancel` handled
- [ ] Targets ≥44px, none in the top 15%
- [ ] Radial deadzone; analog stays analog
- [ ] Button prompts match the connected pad
- [ ] Full rebinding, using `event.code`
- [ ] Device switching changes prompts, not layout
- [ ] Response on down, under 50ms, **measured**
