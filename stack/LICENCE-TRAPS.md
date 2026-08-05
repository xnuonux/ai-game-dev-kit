# Licence traps

**Verified by reading the actual LICENSE files on 2026-08-05.** Not from package.json, not from
memory, not from a README claim.

⚠ This page exists because a licence mistake in a shipped commercial game is expensive and
retroactive. Every entry below is a real trap that a reasonable person would walk into.

---

## 🚨 Cannot be used in a commercial game

| what | licence | the problem |
|---|---|---|
| **lygia** (shader library) | **Prosperity 3.0.0** | *"noncommercial purposes for free... thirty-day trial for commercial purposes."* A shipped commercial game is in violation after 30 days. **Not a permissive licence despite appearing on every shader shortlist.** |
| **GLOSSOPETRAE** | AGPL-3.0 | Network copyleft. |
| **kandev** | AGPL-3.0 | Network copyleft. |

## 🚨 Conditional — read before you bundle

| what | licence | the condition |
|---|---|---|
| **react-bits** | MIT **+ Commons Clause** | *"so long as you do not sell, sublicense, or **redistribute the components themselves** — whether alone, in a bundle, or as a ported version."* **Using it in your product is fine, including commercially. Redistributing it inside a kit or template is not.** Point at it; never vendor it. |
| **bitECS** | **MPL-2.0** | ⚠ Widely and wrongly listed as MIT. Weak, **file-level** copyleft: linking it in a shipped game is fine, but if you modify a bitECS source file and distribute, you must publish that file's source. Safe as long as you never fork it. |
| **athena-crisis** | MIT code, **assets excluded** | *"Branding, game content, art and sound assets... are subject to separate licensing terms."* Lift the TypeScript freely. **Do not lift a single sprite, tile, portrait or sound.** |
| **OpenGameArt assets** | varies per file | CC0, CC-BY and GPL sit side by side. Check every file individually. |
| **Freesound** | varies per file | Same. Some require attribution, some forbid commercial use. |

## ⚠ No licence file at all

Unlicensed means **all rights reserved by default.** These cannot be safely shipped from without
resolving provenance with the author:

- `NinjaAdventure` sprite pack
- `kenney-starter-kit-match-3` (its five siblings all carry MIT files; this copy carries no proof)
- `skills-main` (CrewAI docs skills)
- `agent-town`, `fugu`, `devices-module`

## ✅ Clean for a commercial game

**Public domain:** jsfxr (Unlicense).

**MIT:** phaser · pixi.js · @pixi/particle-emitter · kaplay · matter-js · planck.js · howler ·
tone · @tweenjs/tween.js · zzfx · stats.js · @use-gesture · nipplejs · capacitor · LDtk ·
react-three-fiber · drei · seedrandom · free-tex-packer.

**BSD-2:** excalibur. **BSD-3:** rot.js. **ISC:** canvas-confetti.

**Apache-2.0:** transformers.js · dexie · paper-design/shaders.

**CC0:** Kenney asset packs (the ones carrying licence files) · grafxkid-rpg-sprites.

---

## The build gate

```bash
npx license-checker --production --summary
```

**Fail the build on any AGPL / GPL / LGPL in the tree**, and generate a `NOTICES` file from what
remains. Both stores want attribution, and discovering a copyleft dependency during review is
expensive. Failing at build time is free.

⚠ **`license-checker` reads `package.json`, which is sometimes wrong.** bitECS declares itself
correctly, but `free-tex-packer` says ISC in package.json and MIT in its LICENSE file (both
permissive, so harmless — but it demonstrates the point). **For anything load-bearing, open the
actual LICENSE file.**
