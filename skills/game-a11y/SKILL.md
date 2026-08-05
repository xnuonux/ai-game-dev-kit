---
name: game-a11y
description: Use before shipping any game. Accessibility features that are cheap to build, expand your audience, and are increasingly required by law and by store policy.
---

# Accessibility

**This is not charity and it is not a nicety.** Roughly 20% of players have a disability that affects
how they play. Several of the features below are legally required in the EU from 2025 under the
European Accessibility Act, and platform holders increasingly ask.

⚠ **And every single one of them is cheap if built in and expensive if retrofitted**, which is the
actual reason to read this before P0 rather than after.

---

## The cheap ones with the biggest reach

### Subtitles and captions
⚠ **Subtitles on by default.** Most players use them, disability or not.
- Adjustable size, high-contrast background behind the text.
- **Speaker names** when more than one character talks.
- **Caption important non-speech audio** ... *[door creaks behind you]*. A player who cannot hear the
  thing sneaking up is playing a different, worse game.

### Do not rely on colour alone
🚨 **The most common accessibility failure in games.** ~8% of men have some colour vision deficiency.

If red = enemy and green = ally, **also** differentiate by shape, outline, icon or motion.
**The test:** desaturate a screenshot completely. ⚠ If you cannot tell the factions apart, it fails  ... 
and this is exactly the silhouette test from `game-art`, which is why it costs nothing to satisfy both.

### Remappable controls
Full rebinding, every action, including modifiers. → `game-input`.
⚠ A player with one hand, limited mobility, or an adaptive controller cannot play without it.

### Reduce motion
Screen shake, chromatic aberration, camera bob and heavy particles cause real nausea.
**Ship a toggle. Respect `prefers-reduced-motion` as its default.**

```ts
const reduceMotion = matchMedia('(prefers-reduced-motion: reduce)').matches
const shakeScale = settings.reduceShake ?? (reduceMotion ? 0 : 1)
```

### Text size and contrast
- **Nothing below 16px** on mobile. Ever.
- **4.5:1 contrast minimum** for anything readable.
- A UI scale slider is one multiplier and it serves an enormous number of people.
- ⚠ **No critical information conveyed by a thin outline or a low-contrast tint.**

### Difficulty options
Not "easy mode is for babies". **Separate the axes:** enemy damage, enemy health, timing windows,
and puzzle hints should be independent sliders.

⚠ **A player who cannot complete your game does not rate it lower ... they rate it not at all**, and
they tell people it is inaccessible.

---

## Genuinely cheap wins

- **Pause anywhere.** Including cutscenes. Life interrupts.
- **No timed-input-only sequences** without an alternative. QTEs exclude a lot of people.
- **Hold instead of mash.** Repeated rapid pressing is painful or impossible for many; offer hold.
- **Skippable cutscenes**, always.
- **Screen reader labels on menus** ... semantic HTML on web gets you most of this free.
- **Photosensitivity:** ⚠ no full-screen flashing above 3Hz. This can cause seizures and it is the
  one entry here that can genuinely hurt someone.

---

## Test it

- **Desaturate everything.** Can you still play?
- **Mute everything.** Do you miss critical information?
- **One hand only**, on your default binding.
- **Zoom to 200%.** Does the UI survive?
- ⚠ **Turn off every effect toggle at once.** The game must still be playable and legible ... a lot of
  games break entirely in this state and nobody ever checks.

---

## Checklist

- [ ] Subtitles on by default, resizable, with backgrounds
- [ ] Important non-speech audio captioned
- [ ] ⚠ Nothing conveyed by colour alone ... desaturation test passed
- [ ] Full control remapping
- [ ] Reduce-motion toggle, defaulting from `prefers-reduced-motion`
- [ ] Text ≥16px, contrast ≥4.5:1, UI scale slider
- [ ] Independent difficulty axes
- [ ] Pause anywhere, skippable cutscenes, hold-instead-of-mash
- [ ] 🚨 No flashing above 3Hz
- [ ] An accessibility section on the store page ... players search for it
