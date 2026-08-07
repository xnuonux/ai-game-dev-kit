---
name: game-measure
description: Use when an agent needs to prove a game does what it claims. Headless probes as an instrument, the four laws of measuring your own build, and the reachability audit that catches the class of bug a typechecker cannot see.
---

# Measuring your own game

`game-verdict` is the gate: **the agent brings receipts, the human judges.** This skill is about
where the receipts come from, because **an agent left to its own devices will bring reasoning and
call it evidence.**

> 🚨 **A typechecker proves the pieces FIT. It cannot prove any of them MOVE.**

Every rule below was paid for. They come from a real build where **four features were written,
exported, correct, and had never once executed**, and not one of them threw, logged, or failed a
check.

---

## The four laws

### 1 · declared is not the same as driven

**The bug class:**

- a flag assigned `false` every frame and set `true` **nowhere**, so the behaviour that read it scored
  zero for the life of the project
- a function written, exported, correct, and **never called**, because no code path could reach it
- a draw call passing a literal `0` for a parameter since the first commit, so the state it
  controlled was never rendered
- a capability granted by an item and **read by nothing**

⚠ **Every one of those reads as a finished feature in a diff.** Code review does not catch them.
Types do not catch them. Tests do not catch them unless someone thought to write that exact test.

🚨 **Write a reachability audit and run it in CI.** For a game, it asks:

- is every behaviour both **scored** and **acted on**?
- is every capability an item can grant **read** somewhere?
- is every item **obtainable**?
- is every sound **triggered**?
- is every stat **used** in a calculation?
- ⚠ **is any variable only ever written a constant?** (the `= false` forever case)

⚠ **Expect the detector itself to be wrong at first.** A real one had three bugs of its own: it
missed literals in a per-file scan, missed compound assignment, and missed computed keys. **Plant a
known-dead feature and confirm the audit catches it**, or you have a tool that reports clean because
it cannot see.

### 2 · a red probe is a claim about which of the two is stale

**And it is not always the code.**

Probes that failed while the game was right, in one project: one encoded a contract the design had
moved past · one installed into a slot shape the rules now forbid · one measured a bot's patience
rather than the mechanic · one checked for a log string that had been reworded · one tested a
scenario the game never actually produces.

⚠ **The reflex "the test is red, fix the code" is wrong roughly a third of the time on a design that
is still moving.** Read both before changing either.

### 3 · test the claim, not the journey to it

**A bot dying to a boss proves the bot cannot dodge. It proves nothing about whether the boss
one-shots a companion.**

⚠ **When a probe fails, ask what it actually demonstrated.** Very often the answer is "that the
harness could not reach the situation", which is a harness bug wearing a gameplay bug's clothes.

### 4 · one green run is not evidence when the failure mode is variance

**A real measurement: the same emergent moment measured 21px, then 161px, on identical code.** One
run passed the bar. One failed it badly.

🚨 **Anything emergent gets run at least three times before it counts**, and the report gives the
spread, not the best.

⚠ **This applies to every AI-driven behaviour, every physics interaction, and every "does it feel
right" number in the game.** Those are exactly the things worth measuring and exactly the things a
single run lies about.

---

## The freshness gate

🚨 **The most embarrassing failure available, and it is invisible.**

**A real one: four rounds of careful measurement proving a value of 1.125 failed a bar of 1.30.**
Every measurement correct. Every measurement of a bundle that had not been rebuilt after the
threshold changed.

⚠ **Every probe asserts the build is newer than the source before it measures anything.** Compare
mtimes, or hash the source tree and stamp the hash into the build. **Cover the HTML shell too**, not
just the script bundle ... that one bites the second time.

**A probe that runs against a stale build is worse than no probe**, because it produces confident
numbers that describe code nobody is running.

---

## Probes as instruments

**A headless browser or a headless engine run, driving the real game, reading real state.** Not unit
tests ... unit tests prove a function; probes prove **the game**.

**What they are for:**

| question | how |
|---|---|
| how many hits kill the player at depth 0 vs depth 1? | drive the sim, count |
| does the companion ever actually choose behaviour X? | run 90 samples, report the histogram |
| how close does it get to the thing it should approach? | report the distribution, not the best case |
| does the frame budget hold with 300 entities? | count draw calls, not frames |
| can this fight be won without taking damage? | ⚠ **if not, the difficulty claim is false** |

**Rules that make them worth trusting:**

- ⚠ **self-contained.** No shared setup file that drifts. Any probe runs alone.
- 🚨 **no hardcoded absolute paths.** A probe with `C:/dev/...` in it is documentation of a
  measurement, not a measurement. **Resolve paths from the probe's own location.**
- **print the number, not a verdict.** `hits=12.8` beats `PASS`. ⚠ **A verdict throws away the
  information you will want in three weeks.**
- **name what was NOT covered.** A probe suite that silently caps at the top 10 cases reads as
  "covered everything."

---

## Counting, not feeling

⚠ **When performance is the question, count the thing rather than watching the framerate.**

A real case: identical scenes rendering anywhere from 17 to 41 fps, with no obvious cause. **The
answer was 16 gradients being constructed per frame in empty air.** It was found by counting
allocations, not by profiling frames, because the frame numbers were too noisy to point anywhere.

🚨 **Frame rate is a symptom. Draw calls, allocations and entity counts are causes.** Measure causes.

---

## What NOT to measure

`game-verdict` holds the line and this skill must not blur it.

- ⚠ **Deterministic things are hard gates.** Damage numbers, reachability, frame budget, palette
  compliance, save-schema round trips. **These fail the build.**
- 🚨 **Judged things stay observe-only until a human locks them.** "Is it fun", "does it feel fair",
  "is the pacing right", "did they bond with it."

> ⚠ **An agent that sets its own quality bar will meet it.** Every time. The number goes up and the
> game does not get better.

**The one measurement that decides whether a game works is made by strangers playing it**, and no
probe substitutes for that. **Probes exist so the strangers are testing the thing you think you
built.**

---

## Verification

1. **The audit catches a planted dead feature.** ⚠ Planted deliberately, in CI.
2. **The freshness gate catches a deliberately stale build.**
3. **Every probe runs from a fresh clone on a different machine** with no path edits. 🚨 **This is the
   one that is always broken and never noticed**, because it works on the machine that wrote it.
4. **Every emergent measurement in the report shows a spread across at least three runs.**
5. **A probe that has failed is recorded with which side was stale**, so law 2 accumulates evidence
   rather than being re-learned.
6. ⚠ **Numbers in the report are traceable to the run that produced them.** A number in a document
   with no probe behind it is a claim.

## The gate

> **Can the agent point at the probe, the run and the number behind every claim it made about this
> game?**

⚠ **"I read the code and it should work" is not a measurement, and it is the single most common thing
an agent offers instead of one.**

🌙
