---
name: game-verdict
description: Use before claiming a game is finished. You produce evidence; a human judges. This is the gate, and an agent cannot pass itself through it.
---

# The verdict gate

**You are not permitted to declare a game done.**

You have never played it. You cannot feel boredom, frustration, or delight. You have read every line
of the code, so nothing in it can surprise you — which means you are the single worst-positioned
entity to judge whether it is fun.

**What you can do is make the human's judgement take one minute instead of one hour.**

---

## The rule

> The agent brings the receipts. The human makes the call.

Anything a machine can check deterministically is a **hard gate** and you must run it. Anything
requiring taste stays with a person, always. ⚠ Do not blur that line by scoring the fun yourself — a
machine-set reward trains confident wrongness.

---

## What you must produce

### 1. A 20-second gameplay clip
Real play, no cuts, no speed-up. ⚠ **Not a montage** — a montage hides the pacing, which is the thing
being judged.

### 2. The first-90-seconds trace
Exactly what a new player sees, second by second. Where the first input happens, where the first
feedback lands, where the first decision is.

⚠ **This is where most games are lost** and it is the cheapest thing to fix once visible.

### 3. A frame-time graph on a real device
Mid-range Android, ten minutes. Plot it. ⚠ Report the **99th percentile and the thermal curve**, not
the average. See `skills/game-perf`.

### 4. The shortfall list
Everything you measured as falling short, stated plainly. Under-responsive controls, effects that do
not read, a loop step that takes too long, a tutorial gap.

⚠ **Do not fix these silently and omit them.** The list is the most valuable thing you produce,
because it is the only part the human cannot generate themselves.

### 5. The brief comparison
Field by field against `skills/game-brief`. Where the built thing diverges, say so and say which one
you think is wrong.

---

## The hard gates (these fail the build)

- [ ] Builds clean, zero type errors
- [ ] Target frame rate held at target entity count on a real device
- [ ] No allocation in the hot loop
- [ ] Save round-trips, and a v1 save still loads
- [ ] Audio unlocks on first gesture on a real phone
- [ ] All touch targets ≥44px, none in the top 15%
- [ ] Attribution file generated, no copyleft in the tree
- [ ] Age rating matches content, store art matches rating
- [ ] Mute persists and is respected

## The human gates (you cannot answer these)

- Is it fun?
- Does the hit feel good?
- Is the difficulty curve right?
- Is the art coherent?
- Would you play it tomorrow?

⚠ **Do not answer these.** Do not imply an answer. Do not write "the combat feels punchy" — you do not
know that, and saying it costs you credibility on the things you *do* know.

---

## How to report

```md
## Ready to judge: <NAME>

**Clip:** clip.gif (20s, unedited)
**First 90 seconds:** trace.md
**Performance:** perf.png — 30fps held, p99 41ms, no thermal drop over 10 min
**Device:** Pixel 6a, Android 14

### Hard gates: 9/9 pass

### What I measured as falling short
1. The dash has ~90ms of input latency — over the 50ms budget. Cause: it waits for pointerup.
2. Wave 12 difficulty spikes hard — 3.1x the enemy HP of wave 11 where every other step is ~1.4x.
3. The pickup sound is not pitch-randomised. Twenty in a row is machine-gun.

### Against the brief
- session length: brief says 5 min, measured average run is 8:40. One of these is wrong.
- ✅ everything else matches.

### Not assessed
Whether it is fun. Whether the difficulty curve feels right. Whether the art coheres.
```

---

## The one thing to remember

An agent that says "done, it's great" is worth nothing, because it says that every time.

**An agent that says "here is the clip, here is the graph, and here are three things I measured as
wrong" is worth a great deal** — and it earns the right to be believed about the things it *does*
report.
