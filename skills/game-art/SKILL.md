---
name: game-art
description: Use when a game needs art and nobody on the project can draw. CC0 sources, palette discipline, the licence traps, and how to make placeholder art that ships.
---

# Art, without an artist

**The goal is not "good art". It is coherent art.** A game where every asset came from the same
source, at the same resolution, on the same palette, reads as designed even when each individual
sprite is plain. A game with one gorgeous character and twelve mismatched free sprites reads as
broken, and no amount of quality in the good one fixes it.

⚠ **Coherence beats quality. Every time. Pick one source and stay in it.**

---

## Where to get it

| source | licence | note |
|---|---|---|
| **Kenney** (kenney.nl) | **CC0** | The default. Enormous, consistent, no attribution required. A whole game can ship on Kenney alone and look intentional. |
| **OpenGameArt** | ⚠ **varies per asset** | Check every single file. CC0, CC-BY and GPL sit side by side. |
| **itch.io asset packs** | varies | Many are excellent and cheap. Read the licence — some forbid commercial use. |
| **Google Fonts** | OFL | Safe for UI. |

🚨 **Never bulk-download and assume.** The most common licence failure in indie games is a CC-BY asset
shipped without attribution, or a GPL asset shipped at all.

---

## Palette first, art second

**Choose a palette before you choose a sprite.** A fixed 16-32 colour palette applied across
everything is the single cheapest way to make mismatched sources cohere — you can recolour any sprite
into your palette and it instantly belongs.

- **Lospec** (lospec.com/palette-list) — hundreds of curated palettes, free to use.
- Pick one. Write the hex values into a constants file. **Never use a colour that is not in it.**

⚠ For the cute-brutal look: high-saturation pastels for everything, **one** deeply saturated red used
*only* for damage and gore. The restraint is what makes the red land.

---

## Placeholder art that can actually ship

Do not build with grey boxes and plan to "replace it later". Later does not come, and grey boxes hide
readability problems until it is too late to fix them cheaply.

**Instead: build with flat coloured shapes on your final palette, at final size.**

```ts
const PALETTE = { player: 0xffd6e0, enemy: 0x6b2737, pickup: 0xffe66d, hazard: 0xff2e4c }
// a circle in the right colour at the right size tells you EVERYTHING about
// readability, silhouette and hierarchy. a grey box tells you nothing.
```

This is genuinely shippable as a style. Several successful games never left it.

---

## Silhouette is the whole game at phone size

At 6 inches with 300 entities on screen, nobody sees detail. They see **shape and colour**.

**The test:** render everything as solid black on white. ⚠ **If you cannot tell the player from an
enemy from a pickup, no amount of detail will save it** — and this is the single most common
readability failure in mobile action games.

- Player: distinct silhouette, brightest value, often the only one with an outline.
- Enemies: share a family silhouette so a new one reads as "enemy" instantly.
- Pickups: a shape nothing else uses. Rotate or bob so motion identifies them.
- Hazards: the one colour used for nothing else.

---

## Generated art, honestly

Image generators are viable for **backgrounds, textures, icons and marketing art**. They are still
poor at **consistent characters across frames**, which is exactly what animation needs.

⚠ **Check the terms of the generator you use for commercial rights**, and be aware that store review
increasingly asks. Keep a record of what was generated and with what.

**A reasonable split:** generated backgrounds and key art, CC0 or hand-made sprites and animation.

---

## Atlases

Pack everything with `free-tex-packer` (MIT). One atlas per layer. ⚠ This is not an optimisation, it
is the difference between 400 draw calls and 12. See `skills/game-perf`.

---

## Checklist

- [ ] One palette, written down, never deviated from
- [ ] One primary art source, or everything recoloured into the palette
- [ ] Silhouette test passed at target screen size
- [ ] Every asset's licence recorded, attribution file generated at build
- [ ] Atlases packed, draw calls counted
- [ ] ⚠ No GPL assets, no un-attributed CC-BY
