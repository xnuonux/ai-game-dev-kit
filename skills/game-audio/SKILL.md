---
name: game-audio
description: Use when a game needs sound effects or music and you have no audio files. Covers procedural SFX generation (ZzFX, jsfxr), playback (Howler), generative music (Tone.js), the mobile unlock problem, and the craft rules that separate "has audio" from "sounds good".
---

# Game audio, with no audio files

**The question every agent asks: "what do we use for SFX?"**

The wrong answer is "find some free sounds". You will spend an hour on licence pages and end up with
twelve mismatched WAVs that sound like they came from twelve different games, because they did.

**The right answer: generate them.** A 2D game's entire SFX set can be a few hundred bytes of
parameters, tuned in minutes, perfectly consistent, with zero licence risk and zero download weight.

---

## The stack

| need | use | size | licence |
|---|---|---|---|
| **SFX, procedural** | **ZzFX** | ~1 KB | MIT |
| **SFX, designed** | **jsfxr** (sfxr port, has a GUI) | ~5 KB | Unlicense |
| **Playback of real files** | **Howler** | ~10 KB | MIT |
| **Music / generative** | **Tone.js** | large, lazy-load it | MIT |
| **Anything unusual** | Web Audio API directly | 0 | — |

### ZzFX is the default. Start here.

A complete sound is a single array of numbers. No files, no loading, no CDN, no licence.

```ts
import { zzfx } from 'zzfx'

// pickup
zzfx(...[, , 1675, , 0.06, 0.24, 1, 1.82, , , 837, 0.06])
// hurt
zzfx(...[2.1, , 149, 0.02, 0.02, 0.11, 4, 2.4, , , , , , 1.2, , 0.3, , 0.5, 0.02])
// explosion
zzfx(...[2, 0.1, 250, 0.02, 0.1, 0.5, 4, 1.5, , , , , , 1.5, , 0.5, 0.1, 0.3, 0.1])
```

**Design them in the browser at <https://killedbyapixel.github.io/ZzFX/>** — mash the randomise
button until something is close, tweak, copy the array into your code. Ten minutes gets you a full
game's SFX set that all sounds like one game, because one synth made it.

### jsfxr when you want the classic sfxr feel

The 8-bit arcade vocabulary (`pickupCoin`, `laserShoot`, `explosion`, `powerUp`, `hitHurt`, `jump`,
`blipSelect`). Generator UI at <https://sfxr.me>. Exports a parameter string you play at runtime.

### Howler only for real audio files

Music, voice, and recorded foley. **Use audio sprites** — one file containing every sound with an
offset map. One request instead of forty, which matters enormously on mobile.

---

## The craft rules

These are what actually separate a game that "has audio" from one that sounds good. Almost every
agent-built game skips all of them.

### 1. Never play the same sound twice identically

**This is the single highest-value rule in game audio.** The human ear detects an exact repeat
instantly and reads it as cheap. Randomise pitch on every playback.

```ts
function play(params: number[], variance = 0.08) {
  const p = [...params]
  p[2] = (p[2] ?? 220) * (1 + (Math.random() * 2 - 1) * variance) // frequency
  zzfx(...p)
}
```

±5-10% is invisible as pitch change and completely removes the machine-gun effect.

### 2. A good hit is three sounds, not one

Transient (the click of contact) + body (the substance being hit) + tail (the ring or splatter).
Layering three cheap sounds beats one expensive sound every single time.

### 3. Voice-limit everything

200 enemies dying in one frame = 200 simultaneous plays = clipping mud and a stalled audio thread.

```ts
const lastPlayed = new Map<string, number>()
function playLimited(id: string, params: number[], minGapMs = 40) {
  const now = performance.now()
  if (now - (lastPlayed.get(id) ?? 0) < minGapMs) return
  lastPlayed.set(id, now)
  play(params)
}
```

### 4. Solve the mobile unlock once, correctly

**Browsers refuse to play audio before a user gesture.** Not a bug, not fixable, and it will silently
break your entire soundtrack if you initialise on load.

```ts
let unlocked = false
function unlockAudio() {
  if (unlocked) return
  unlocked = true
  const ctx = new (window.AudioContext || (window as any).webkitAudioContext)()
  if (ctx.state === 'suspended') ctx.resume()
  const b = ctx.createBuffer(1, 1, 22050)
  const s = ctx.createBufferSource()
  s.buffer = b; s.connect(ctx.destination); s.start(0)
}
// bind to the FIRST touch/click/key, then remove
;['touchstart', 'mousedown', 'keydown'].forEach(e =>
  window.addEventListener(e, unlockAudio, { once: true, passive: true }))
```

### 5. Duck the music on big events

Drop music to ~40% for 200ms on an explosion or a boss appearing. It is nearly free and it makes the
event feel enormous.

### 6. Silence is an effect

**Hitstop should stop the audio too.** A frozen frame with sound still running feels broken; a frozen
frame with a beat of silence feels like impact. See `skills/game-feel`.

### 7. Ship a mute button that is honest

Persist it. Respect the device silent switch on iOS. A game that ignores mute gets uninstalled and
one-starred, and it is the most common complaint in mobile game reviews.

---

## If you genuinely need recorded audio

- **Kenney** (kenney.nl) — CC0, no attribution required, huge, and stylistically consistent.
- **Sonniss GDC bundles** — royalty-free, professional, enormous.
- **Freesound** — ⚠ **per-file licences vary wildly.** Check every single one. Some require
  attribution, some forbid commercial use. Never bulk-download and assume.

Whatever you use, **generate an attribution file at build time** and ship it. See `skills/game-ship`.

---

## Checklist before you call audio done

- [ ] Every SFX is pitch-randomised
- [ ] Audio unlocks on first gesture, verified on a real phone
- [ ] Voice limiting caps repeated sounds
- [ ] Mute persists and respects the device
- [ ] Hitstop silences
- [ ] Total audio payload measured (procedural should be ~0 KB)
- [ ] Attribution file exists if any recorded audio ships
