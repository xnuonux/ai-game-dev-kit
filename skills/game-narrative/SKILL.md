---
name: game-narrative
description: Use when a game has a world, a history, or secrets. The two ledgers (what is true, and what the player has been told), contradiction checking that is deterministic rather than vibes, the bible you never show, and every surface a story leaks through.
---

# Narrative, as a system rather than a document

Most game writing lives as prose in boxes ... a wiki, a Notion database, a folder of docs. Which means
**the project has no idea what its own story says.**

It cannot tell you that the character you just killed is alive in a scene written last year, that two
cities are four days apart in one chapter and nine in another, or that a character knows something
they were never told.

🚨 **Consistency is currently maintained inside one person's head, and it is the most common reason
long projects collapse.** Series bibles exist because this is brutal, and a series bible is a
document that also goes stale.

⚠ **And generation made it worse.** A model asked to write in your world will confidently invent,
because it produces plausible text rather than consulting a record. **More generated prose against an
unchecked world means contradictions arrive faster than any human can catch them.**

---

## The two ledgers

**This is the whole skill. Everything else is implementation.**

| ledger | question it answers |
|---|---|
| **TRUTH** | what is actually so in this world |
| 🚨 **REVELATION** | **what the audience has actually been told, and where** |

**Almost every tool tracks the first. Almost none tracks the second.**

Every fact carries the scene, chapter, level, episode or line in which it was revealed ... **or the
value `unrevealed`.** So you can ask *"what does the player currently know about this character?"* and
get a real answer instead of a guess.

⚠ **That is the entire craft of mystery, foreshadowing and dramatic irony, and it is currently done on
index cards.**

**And it catches the single most expensive error in serial storytelling: a character acting on
something they were never told.** Nothing else finds that. Not a reader, not a proofread, not a
model. **It is a query.**

---

## Facts are rows, not prose

Not pages. **Assertions.**

```
(subject, relation, object, valid_from, valid_until, source, revealed_in, confidence)
```

⚠ **Prose is stored too, and it is always a VIEW over the facts rather than the place they live.**
🚨 **The moment prose is the source of truth, nothing can be checked.**

**Time is first-class, and it is what makes history possible instead of a flat wiki.** A city has
three names across four centuries. A border moves. A person is a child, then a ruler, then a corpse.
⚠ **All of those statements are correct at their own time**, and a system without validity intervals
has to call three of them wrong.

---

## Contradiction checking, in three tiers

⚠ **The tiers are visible to the writer.** A tool that mixes certainty with suspicion trains people to
click past all of it.

**🚨 HARD ... deterministic, non-negotiable, always right.**

- a person acting after their death date
- a person in two places at the same time
- a place founded after an event held in it
- the same distance or duration asserted at two different values
- a cycle in a lineage or a chain of causation
- an object in two hands at once

**These are errors. They are counted. They fail the build.**

**⚠ SOFT ... structural but arguable.** A journey whose duration disagrees with the map. A population
that grew impossibly. A technology used before it exists on the timeline. **Flagged, ranked,
dismissible ... and the dismissal is kept with its reason**, so it does not resurface every week.

**LOOSE ... a model reading prose and suggesting a possible clash.** ⚠ **Observe-only, always labelled
as a suggestion, and it can NEVER raise a hard error.**

> 🚨 **A model-judged claim that is allowed to fail a build is confident wrongness with authority.**
> This kit holds the same line everywhere: **deterministic rules are gates, judged dimensions stay
> observe-only until a human locks them.**

---

## The bible you never show

**Most worlds are better with a history the audience never receives.** The history is not there to be
delivered. **It is there so that every name, every ruin, every posture and every offhand line is
drawn FROM something rather than invented per-asset.** That is what makes a world hold up under
scrutiny instead of unravelling.

⚠ **Which means one rule governs what goes in it:**

> 🚨 **If a fact does not cash out as something the player can SEE, it does not go in the bible.**

**A fact that can only be delivered as exposition is not canon. It is a temptation.**

**The test for every entry:** name the physical consequence. *What does this change about a room, a
silhouette, a direction something faces, a line of dialogue that gets cut, an item description?* ⚠ **No
consequence, no entry.**

---

## The delivery hierarchy

**Best to worst, and most projects invert it:**

1. 🚨 **Excavated.** The player assembles it from what they find, touch, and use. **The strongest,
   because they earn it and therefore believe it.**
2. **Environmental.** A room that is legible. A body positioned a certain way. Something facing a
   direction. ⚠ **No text.**
3. **Rare speech.** ⚠ **Rare and deep are the same requirement.** A world that says little must make
   every single thing it does say worth the silence around it. **And frequency should FALL over time,
   not rise.**
4. **Documents, logs, terminals, codex entries.** ⚠ Legitimate in some genres and a crutch in most.
   **If the story only exists in collectibles, it is not in the game.**
5. **A character explaining the plot to another character who already knows it.** Never.

---

## Every surface a story leaks through

🚨 **The most common way a carefully withheld story gets given away is not a writing mistake. It is a
different department.**

Audit all of these, and put them in the publishing brief rather than the design doc, **because that
is where they get broken:**

- ⚠ **Achievement and trophy names and descriptions.** *The most frequent offender by a distance.* A
  trophy called after your final twist appears in a list players scroll before they play.
- **The store page, the capsule text, the trailer, the press kit.**
- ⚠ **Subtitle and caption files**, which ship as readable text and get datamined day one.
- **Localization strings**, which are handed to people outside the project ... see below.
- **Asset and file names in the shipped build.**
- 🚨 **A generative voice.** ⚠ **If a character speaks from a model, it is the single most likely leak
  in the entire product**, and it must be structurally unable to say the withheld thing rather than
  merely instructed not to.
- **Wiki-bait phrasing** in any public copy.

**One sentence in a store description can undo a project's entire narrative discipline**, and nobody
who writes that sentence has read the bible.

---

## Localization, and the trap inside it

**Facts-as-rows localize well. Prose does not.** But there is a specific and vicious problem:

🚨 **A translator who does not know a secret will spoil it grammatically.**

In gendered languages a translator must choose a gender for a character whose identity is concealed.
In languages with formality registers they must choose a register that encodes a relationship the
player has not learned yet. ⚠ **They will guess correctly from the truth ledger and destroy the
reveal**, or guess wrongly and produce nonsense.

**So: localization notes are generated FROM the revelation ledger, not from the truth ledger.** The
translator is told what the audience knows at that point and what must stay ambiguous. ⚠ **This is
almost never done and it is why reveals land differently in different languages.**

---

## Names are promises

⚠ **Every name you spend is spent forever**, and a name reused for a different thing three years later
is a contradiction nobody will catch.

- **Keep a reserved-names list** with what each name is attached to and whether it is retired.
- ⚠ **Check it before naming anything**, including internal codenames that leak into file paths.
- **A name that implies a fact is a fact.** Calling something *the Second Fall* commits you to a
  first one.

---

## Verification

1. 🚨 **The hard-tier gate:** a corpus of **planted** contradictions of every hard class is caught at
   **100%**, ⚠ **and a corpus of deliberately tricky non-contradictions produces zero false errors.**
   Both in CI. **A hard tier with false positives is worse than no hard tier**, because people learn
   to ignore it.
2. 🚨 **The leak gate:** no export, save file, subtitle, achievement string, localization bundle or
   generated line can emit an `unrevealed` fact. ⚠ **Tested adversarially against every text-bearing
   path, not just the ones you remember.**
3. **The tier gate:** the loose model tier **cannot** raise a hard error. Planted.
4. **The extractor gate:** if prose is parsed into facts, it writes to a **proposal queue** a human
   accepts. ⚠ **Never directly into canon.** An extractor that writes canon poisons the well silently.
5. **The bible audit:** sample entries and name the physical consequence of each. ⚠ **Anything with no
   consequence gets cut**, and the cut is the point.
6. **The publishing brief exists** and lists every leak surface with an owner.

## The gate

> **Did a writer change what they were about to write because the system told them something they had
> forgotten was already true?**

⚠ **That is the moment this exists.** Everything before it is a nicer wiki.

**And for the withheld story:**

> **Did players who were never told anything come away with a coherent, confident, unprompted account
> of what happened ... and was it close?**

⚠ **Close, not correct.** 🚨 **If every player agrees exactly, you explained it somewhere. If nobody
can say anything at all, the physical consequences are not landing and your world is set dressing.**

🌙
