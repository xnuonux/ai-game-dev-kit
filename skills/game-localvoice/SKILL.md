---
name: game-localvoice
description: Use when a character speaks or reasons using a model that ships inside the game rather than a server. The three licences that must all clear, the ratings risk nobody budgets for, and the grounding rule that stops a character sounding like a chatbot.
---

# A model inside the binary

**A character whose lines come from a model running on the player's own machine.** No server, no
account, no per-line cost, no death when the servers are switched off, and nothing about a player's
session leaves their device.

⚠ **This is genuinely new territory and it is mostly a licensing and ratings problem wearing an
engineering problem's clothes.** The engineering is the easy part.

🚨 **Read this before writing a line of integration code, because two of the traps below are
retroactive and one of them can stop a release.**

---

## 🚨 three licences, and all of them must clear

**Everyone checks one. There are three, and any single one can be non-commercial.**

**1 · the model weights.** ⚠ **The output restriction is the thing to look for.** A code licence
governs the tool; **a weights licence can govern what the model produced.** `game-artgen` has the
full version of this asymmetry and it applies identically here.

**2 · the inference runtime.** The library that loads and runs the weights. Usually permissive, ⚠ **not
always**, and copyleft here is far worse than in a tool because **you are linking it into a shipped
binary.**

**3 · the training-data provenance claim.** ⚠ **Read what the model card actually asserts about its
data**, particularly for voice models. **A voice model trained on unlicensed speech is a rights
problem that arrives after you ship**, and "the licence said MIT" is not a defence when the licence
was granted by someone who did not hold the rights.

> ⚠ **A concrete example worth knowing: at least one widely-recommended open text-to-speech project
> is research-licence-only on BOTH its code and its weights, with explicit enforcement language.**
> It appears on every shortlist. **It is unusable on any commercial path**, and a team that finds
> this out in month six has to replace a component the whole character depends on.

**The rule, matching `stack/LICENCE-TRAPS.md`:** ⚠ **read the actual LICENSE file for the exact
weights and the exact runtime version you shipped.** Not the model card summary. Not a blog post.
Not this document. **Record what you read and when.**

---

## ⚠ the ratings risk, which nobody budgets for

**A game containing unconstrained generated text may be rated differently, or refused, or require a
disclosure you did not plan for.**

Ratings bodies already treat user-generated and unmoderated content as a distinct category, and
platforms have been adding AI-content disclosure fields. ⚠ **Whether a locally-generated character
line counts is exactly the kind of question you want answered in month one rather than in
submission week.**

🚨 **Ask the ratings body and the platform directly, early, in writing.** ⚠ **Their answer is the
answer. Nothing in this document is.**

**And a second, sharper problem:** ⚠ **a local model can be jailbroken by the player, and there is no
server to fix it.** **What ships is what runs forever.** A hosted model can be patched on a Tuesday.
A model in a binary cannot, and a video of your character saying something appalling is a permanent
artefact.

**Which forces the design:**

- **Constrain hard at generation time**, not by asking politely in a prompt. ⚠ **A system prompt is
  not a control.**
- **Cap length brutally.** A few sentences. ⚠ **The longer the output, the further it drifts from
  anything you tested.**
- **Filter output deterministically** before it reaches the screen.
- 🚨 **Prefer selection over generation wherever it is not worse.** A model choosing between authored
  lines, or filling authored slots, gives most of the feeling with none of the risk. ⚠ **Free-form
  generation is the last resort, not the default.**

---

## 🚨 the grounding rule

**A character that can say anything will eventually say something generic, and one generic line tells
the player it is a chatbot. Everything you built collapses in that sentence.**

⚠ **This is not a prompt-quality problem and better prompting does not fix it.**

> **Bind every line to state the game actually has.**

The place they nearly died. The thing they are carrying. What broke last. Who was there. How long it
has been. ⚠ **If the game already keeps that state ... and most games keep far more than they use ...
then a grounded line cannot be generic, because it references something that genuinely happened to
this player.**

**And 🚨 the character must be allowed to have nothing to say.** ⚠ **Silence is a real, frequent,
unapologetic output.** A companion that sometimes just looks at you is more alive than one that
always answers, and always-answering is the single loudest chatbot tell there is.

⚠ **Frequency should FALL over time, not rise.** Rare and deep are the same requirement.

---

## The lore leak

🚨 **If your game withholds anything, a generative character is the single most likely place it gets
given away.**

⚠ **The character must be structurally unable to say the withheld thing, not merely instructed not
to.** `game-narrative` holds the two-ledger model: **what is true, and what the player has been
told.** **The model receives only the revealed ledger.** Never the truth ledger, never the bible,
never "context for richness."

**A model given the full history will eventually explain it. Not because it is disobedient, because
it is helpful.**

---

## Engineering, briefly

**It is the easy part, and it has four hard numbers.**

**Size.** ⚠ **A model is part of your download.** Mobile stores have delivery limits and every hundred
megabytes costs installs. **Decide the ceiling before choosing a model**, not after. Quantised small
models are the only realistic option, and the quality drop is real and must be auditioned rather
than assumed.

**Latency.** 🚨 **A character that takes four seconds to answer is broken**, regardless of what it
says. **Budget first token and full response on your worst supported device**, not on the machine
that built it.

**The hardware floor.** ⚠ **The model must run on the worst machine you support, or degrade cleanly.**

> 🚨 **Design the silent version FIRST, and make it good.**
> **The model is an amplifier on a character that already worked.** If the character only works when
> the model runs, then it does not work on the low-end device, the thermally throttled phone, or the
> machine that ran out of memory ... and it did not work in your design either.

**Determinism and saves.** ⚠ **Generated text is not reproducible.** Store what was said, not the seed
that made it. `game-save` applies: **the transcript is state and it needs a schema.**

---

## Verification

1. 🚨 **The three-licence audit:** weights, runtime and provenance claim, each read and recorded with
   exact versions. ⚠ **A model-hub listing is not a licence.**
2. 🚨 **The ratings question is asked in writing, early**, and the answer is filed.
3. 🚨 **The leak gate:** the model cannot access an unrevealed fact. ⚠ **Tested adversarially by
   someone trying to make the character explain the plot.** **Planted in CI.**
4. **The jailbreak audit:** a capable person spends a day trying to make the character say something
   that would end the project. ⚠ **Publish internally exactly what they managed**, and constrain
   against it deterministically rather than by prompt.
5. **The silent-build test:** ⚠ **disable the model entirely and confirm the character still works.**
   **If the game is worse in a way players would complain about, the silent design was not finished.**
6. **The floor test:** first token and full response measured on the worst supported device, at
   thermal throttle.
7. **The generic test:** collect several hundred real lines from real sessions and have someone who
   has not played read them. ⚠ **Count how many could have been said by any character in any game.**
   **That number is the product.**
8. **The network gate:** packet capture proves nothing leaves the device. ⚠ **State it publicly and
   tell people how to check.**

## The gate

> **If the model were removed tomorrow, would the character still be someone the player cares about?**

⚠ **If yes, ship the model, because it is making a real thing better.**
🚨 **If no, you have built a chatbot with a body**, and the bond players report is a bond with the
novelty, which expires.

🌙
