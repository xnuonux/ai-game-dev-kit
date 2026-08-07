---
name: game-artgen
description: Use when generating game art with AI models. What generators are actually good at, what they cannot do at all, the model-licence trap that restricts your OUTPUT, and the five-step pipeline where generation is only step one.
---

# Generated art, as a production pipeline

`game-art` establishes the law this skill serves: **coherence beats quality.** A generator does not
give you coherence. It gives you one beautiful asset at a time, each from a slightly different world,
and **a folder of individually gorgeous mismatched images is the exact failure `game-art` warns
about**, arrived at faster and more expensively.

🚨 **Generation is step one of five. Steps two to five are deterministic, scriptable, and they are
what actually makes the art ship.**

---

## The honest capability line

Write this down before planning any art budget around a generator.

**Genuinely good, today:**

- **Large painterly backgrounds and skyboxes.** Few in number, large on screen, never animated. **The
  single best fit that exists.**
- **Textures and tiling materials**, with a seamless pass.
- **Icons, marketing art, key art, store capsules.**
- **Concept exploration.** Twenty directions in an hour, before anyone commits.
- **Normal and height maps derived from an existing image** (§the pipeline, step 4).
- **Upscaling and matting** ... narrow, mature, boring, reliable.

**Cannot do it, and will waste weeks pretending:**

- ⚠ **The same character across frames.** This is the one everyone tries first. Consistency is
  approximate, and animation needs it exact.
- 🚨 **Production pixel art.** See below ... this is the most expensive misunderstanding in the list.
- **Sprite animation of any kind.** ⚠ AI video is temporally inconsistent, has no alpha channel, and
  does not land on frames. **It is not a sprite sheet and no amount of post-processing makes it one.**
- **UI at exact pixel sizes.** Nine-slices, exact hit targets, crisp 1px borders.
- **Anything on a strict grid.**

### 🚨 the pixel art trap

Ask a generator for pixel art and it produces **pixel-LOOKING art**: four thousand colours,
anti-aliased edges, and a "pixel" size that drifts across the image.

It cannot be palette-swapped. It cannot be animated. It does not tile. Downscaling it produces mush,
and **an audience that likes pixel art identifies it instantly and reads it as contempt.**

⚠ **If your mid-ground is pixel art, that layer is human-made or bought. Budget for it.** Generators
serve the layers where pixels are largest and asset count is smallest.

---

## 🚨 the model-licence trap

`stack/LICENCE-TRAPS.md` covers code. **Model weights are a different and worse problem, because a
model licence can restrict what you do with the OUTPUT.**

> ⚠ **A GPL tool does not make your asset GPL.** Using GIMP does not GPL your painting, and this trips
> people in the wrong direction constantly.
> 🚨 **A model licence can, and several do, restrict commercial use of what the model produces.** That
> is the asymmetry to hold in your head.

**The clearest example, and it is one family:** the **FLUX.1** models ship under different licences
per variant ... at least one is explicitly **non-commercial**, and at least one is permissive.
⚠ **Same name, same family, opposite answers.** A team that pulls "FLUX" from a model hub without
reading which variant they got has a retroactive problem in a shipped commercial game.

**The rule, matching this kit's existing standard:**

1. ⚠ **Read the actual LICENSE file for the exact weights you downloaded.** Not the repo README, not
   the model card summary, not a blog post, not this document.
2. **Check for a use-restriction appendix.** RAIL-family licences are commercially permissive but
   carry behavioural restrictions that are still binding terms.
3. **For hosted generators, read the terms of service for commercial rights**, and note that terms
   change and are not retroactively archived for you.
4. 🚨 **Keep a provenance record for every generated asset**: model, exact variant, version, date,
   prompt, and seed. ⚠ **Store review increasingly asks, and platforms are adding AI-disclosure
   fields.** A record you kept from day one costs nothing; one reconstructed under review costs a
   release window.

⚠ **Also check whether your storefront requires AI-content disclosure.** At least one major PC
storefront does. **That is a store-page field, not a legal opinion, and omitting it is a delisting
risk rather than a lawsuit risk** ... which is worse, because it is faster.

---

## The pipeline

**Generation is step one. Run steps two to five on EVERY asset**, including hand-made and CC0 ones.
🚨 **That is the entire trick: coherence comes from the passes, not the source.**

### 1 · generate

Prompt for the layer, not for the game. ⚠ **Fix the camera angle, the light direction and the
distance in the prompt and never vary them within a layer.**

### 2 · matte

Background removal to a clean alpha. Mature, local, free tooling exists. ⚠ **Check the edge at 400%**
... a halo of the old background is the most common defect and it shows up as a bright fringe the
moment the asset sits on a dark scene.

### 3 · palette-lock 🚨 the coherence pass, and the one nobody does

**Quantise every asset to THE palette.** The fixed 16-32 colours from `game-art`. Deterministic,
scriptable, and instant.

**This is what makes a generated painterly plate, a bought pixel sprite and a hand-drawn silhouette
look like one game.** ⚠ **It is also the pass that lets three visual registers coexist in one frame**,
which is otherwise the thing that reads as three different games.

- Ordered dithering for the painterly layers, nearest-colour for anything that must stay crisp.
- ⚠ **Run it in CI on the asset folder**, so an unprocessed asset cannot reach a build.

### 4 · normal and height bake

If the project uses 2D lights, ⚠ **every lit sprite needs a normal map or it sits flat next to
everything that has one**, and the flat one looks broken rather than stylised.

Both ML-based and heuristic tools exist. **Heuristic ones are often better for stylised art**, because
an ML normal map guesses at realistic geometry your art does not have.

🚨 **Bake AFTER the palette-lock**, or the normal map encodes colours the final asset does not have.

### 5 · the silhouette check, automated

`game-art` gives the test: render solid black on white and see if you can tell things apart.
⚠ **Make it a script, not a habit.** Every asset, every build, output to a contact sheet a human
scans in ten seconds.

**A generated asset that fails the silhouette test is a beautiful asset that will read as noise in
motion**, and generators produce these constantly, because they optimise for a still image.

---

## The layered-register strategy

**When a project needs to look expensive and cannot afford to be:** split the frame by distance and
give each band a different register.

| band | register | why it is affordable there |
|---|---|---|
| **far** | painterly, atmospheric | ⚠ costliest per asset, so make **few**, make them **large**, and **never animate them.** Parallax does the work. The best generator fit that exists. |
| **mid** | your crafted style, whatever it is | where gameplay reads. **The density budget lives here and it is human-made.** |
| **near** | silhouette, near-black, big negative space | 🚨 **the cheapest thing in 2D art**, and in the foreground it reads as deliberate style rather than as a saving. |

⚠ **The failure mode is that three registers read as three games.** What marries them is: **one
palette (step 3), one light direction, and one atmospheric haze pass that every layer sits inside.**
🚨 **The haze is the marriage. If the layers ever separate, the haze is too thin.**

---

## What to never generate

- ⚠ **Anything that must be consistent across frames.**
- **Anything on a strict pixel grid.**
- **UI that has exact hit targets or 1px borders.**
- 🚨 **Anything resembling a real person, a real brand, or a recognisable existing character.** A
  store takedown is faster and cheaper for the platform than a conversation.
- **Fonts.** Use a licensed font.
- ⚠ **Anything you cannot record provenance for.**

---

## Verification

1. 🚨 **The licence audit:** every model used has its LICENSE file read and recorded, with the exact
   variant name. ⚠ **A model hub listing is not a licence.**
2. **The provenance ledger exists** and covers 100% of generated assets. Checked in CI as a count
   against the asset folder.
3. **The palette gate:** no asset in the build contains a colour outside the palette. ⚠ **Planted in
   CI** ... it will fail the first time and that is the point.
4. **The silhouette contact sheet** is generated every build and a human has looked at it.
5. **The halo check:** matted assets inspected on the darkest background in the game.
6. ⚠ **The coherence test, and it is the only one that really matters:** show a stranger five assets
   from five different sources, post-pipeline. **Ask them which one came from somewhere else.** If
   they can tell, steps 2 to 5 are not doing their job.

## The gate

> **Would a person who dislikes AI art be able to identify which assets were generated?**

⚠ **If yes, the pipeline failed, not the generator.** ⚠ **If they can identify it AND they are wrong
about which ones, the pipeline worked.**

🌙
