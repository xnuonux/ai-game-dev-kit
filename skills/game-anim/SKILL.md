---
name: game-anim
description: Use when characters or creatures need to move. Hand-authored versus procedural and the one question that decides it, the cancel window that is the most common cause of a game feeling bad, and the interruption rules nobody writes down.
---

# Character motion

`game-feel` covers the 200 lines of juice. `game-vfx` covers particles, hitstop and shake.
**Neither covers a body moving**, and character motion is where a game most often stops feeling like
a game and starts feeling like a slideshow of poses.

🚨 **An animation's first job is legibility. Its second job is beauty. A project that reverses those
ships something gorgeous that players cannot read.**

---

## The one question that decides your whole approach

> ⚠ **Is the character's silhouette fixed, or can the player change it?**

**Fixed silhouette** ... one body, known proportions, known limb count. **Hand-author it.** Nothing
procedural will beat a good animator, and the frame count is finite and knowable.

🚨 **Player-configurable silhouette ... hand animation is impossible and no budget fixes it.** Three
attachment points across eleven parts is already hundreds of combinations, and the part count only
grows. **Nobody hand-animates that, and the moment somebody proposes "we will just do the common
ones," the uncommon ones become the bug list.**

⚠ **When the silhouette is configurable, procedural is not a saving. It is the mechanic**, because
"my character moves differently to yours" is usually the entire reason the configuration exists.

**The split is by subject, not by preference.** A project can and usually should hand-author every
fixed-silhouette creature in the game and drive the one configurable body procedurally. ⚠ **Mixing
them in one project is normal and correct.**

---

## Procedural techniques, by return on effort

**Ordered so a project can stop at any point and still have gained something.**

**1 · squash, stretch and secondary motion.** ⚠ **The cheapest enormous win in the list.** Scale on
the axis of travel, a spring-driven overshoot on stop. **Ten lines. Transforms a stiff sprite into a
body.**

**2 · spring-driven followers.** Tails, antennae, cloth, cables, hair, anything trailing. A chain of
damped springs following the parent transform. ⚠ **Tune damping per element or everything wobbles
identically and it reads as a single material.**

**3 · look-at.** The head, or the sensor, or the eye tracks what the character cares about.
🚨 **Disproportionately effective for one line of code.** A creature whose head turns toward a threat
is understood as *aware*, and awareness is most of what players read as intelligence.

**4 · ground adaptation.** IK foot placement against the actual surface, plus body tilt. ⚠ **Without
it, characters visibly float on slopes and stairs**, and once seen it cannot be unseen.

**5 · procedural locomotion.** Full gait generation for legged bodies: step targets, transfer
timing, body oscillation. **The correct answer for configurable limb counts.** ⚠ Expensive to get
right and worth it only when technique 1 through 4 is not enough.

**6 · additive layers.** A breathing layer, a damage layer, a carrying layer applied on top of a base
pose or clip. ⚠ **This is how a procedural body gets personality without a combinatorial explosion**:
the base handles the mechanics, the additive layers handle who it is.

**7 · ragdoll blending.** For deaths and impacts, blending from animated to simulated. ⚠ **Blend, do
not switch.** A hard cut to ragdoll is the most recognisable cheap effect in games.

---

## 🚨 the cancel window

**The single most common cause of "this game feels bad", and it is not an animation problem, it is an
input problem wearing an animation costume.**

**When a character is mid-animation and the player presses something, one of three things happens:**

| behaviour | feels like |
|---|---|
| the input is **dropped** | 🚨 **broken.** The player is certain they pressed it, because they did |
| the input is **queued and plays after** | sluggish, but honest. Acceptable for heavy actions |
| the animation **cancels into the new one** | responsive |

⚠ **Decide this per action and write it down.** Not per animation ... **per action**, because the same
animation may be cancellable in one context and committed in another.

**The rules that make it feel right:**

- **Buffer every input for a short window** (roughly 100 to 200ms) so a press slightly too early still
  lands. ⚠ **Players do not press on the frame. They press on the intention.**
- **Define a cancel window on committed actions**, usually late in the recovery. **Cancelling into
  the next attack during recovery is what makes combat feel fluent.**
- 🚨 **Movement and defence should almost always cancel everything.** A player who cannot dodge because
  they are mid-animation experiences that as the game killing them, ⚠ **and if `game-design` claims
  everything is dodgeable, an uncancellable animation makes that claim false.**
- **Never lock input during a reaction the player did not choose.** Hit-stun that removes control is a
  deliberate cost and it must be a design decision, never an animation side effect.

---

## Motion as communication

**Before any polish, every animation must answer: what does this tell the player, and how early?**

- ⚠ **Anticipation is a telegraph and a telegraph is a fairness contract.** A wind-up that is 200ms on
  one attack and 600ms on another teaches the player a timing that is a lie. **`game-ai` owns the
  fairness rules; animation is where they are kept or broken.**
- **Follow-through sells weight** and costs almost nothing.
- 🚨 **The silhouette-in-motion test.** `game-art` gives the still-frame version. **Do it on the
  animation**: render the clip as solid black and confirm the action is readable at every frame.
  ⚠ **A pose that reads standing still and vanishes in motion is a pose players will die to.**
- **Distinct actions need distinct shapes**, not distinct colours. Colour is unavailable to a
  meaningful fraction of players and to everybody in a busy frame.

### stillness

⚠ **In a world where everything moves, a thing that is on and perfectly still is louder than it could
ever be in a world where nothing moves.**

**Motion is a currency and spending it everywhere devalues it.** A project with fluid animation
throughout should deliberately reserve total stillness for the things that matter.

---

## The traps

- 🚨 **Root motion versus code-driven movement.** Pick one per character and never mix. ⚠ **Mixing
  produces desync that appears only under lag, on slopes, or at low frame rates**, which is to say it
  appears in front of players and never in front of you.
- ⚠ **Animation is where 2D projects silently lose the frame budget.** Every additive layer, every
  spring chain, every IK solve runs per character per frame. **Count them at your worst-case entity
  load** ... `game-perf` and `game-measure` both apply: **count causes, not frames.**
- **Animation events that fire gameplay** (spawn the hitbox on frame 7) tie your balance to your art.
  ⚠ **When an animator retimes a clip, the balance changes and nobody tells the designer.** **Drive
  gameplay from the logic clock and let animation follow it.**
- ⚠ **Blend times are a feel setting, not an art setting.** They belong with the tuning constants,
  visible and adjustable, not buried in a state machine graph.
- 🚨 **Generators cannot animate sprites.** See `game-artgen`. AI video is temporally inconsistent, has
  no alpha, and does not land on frames. **It is not a sprite sheet.**

---

## Verification

1. 🚨 **The cancel table exists**, listing every action and what it can be interrupted by. ⚠ **Written
   down, not implied by the code.**
2. **The input-buffer test:** a press one frame too early lands. Planted.
3. 🚨 **The dodge test:** if the design claims everything is avoidable, **prove there is no animation
   state in which the player cannot defend.** ⚠ **Planted, and it will fail the first time.**
4. **The silhouette-in-motion contact sheet** exists for every action, and a human has looked at it.
5. **The telegraph audit:** wind-up durations across all threats, listed. ⚠ **Inconsistency is a
   fairness bug and it will be reported by players as "unfair" rather than as "inconsistent."**
6. **The budget count:** springs, IK solves and additive layers per frame at worst-case entity count.
7. **The slope test:** every character on stairs, slopes and moving platforms at low frame rate.

## The gate

> **Can a player tell what is about to happen, early enough to act, from the shape alone ... with the
> colour removed and the sound off?**

⚠ **If not, the animation is decoration and the fairness claims in the design document are not true.**

🌙
