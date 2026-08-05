# Track: `3d-web`

3D from one codebase, shipping to browser, mobile and desktop.

⚠ **3D is roughly 3-5x the asset and engineering cost of 2D for the same amount of game.** Choose it
because the game needs a third axis, never because it sounds more impressive.

---

## Stack

| role | pick | licence |
|---|---|---|
| Renderer | **three.js** | MIT |
| React layer (optional) | **react-three-fiber** + **drei** | MIT |
| All-in-one alternative | **Babylon.js** | Apache-2.0 |
| Physics | **Rapier** (Rust/WASM) | Apache-2.0 |
| Physics (lighter) | **cannon-es** | MIT |
| Models | **glTF 2.0 / .glb** | ... |
| Post-processing | **postprocessing** (pmndrs) | MIT |
| Native wrapper | **Capacitor** or **Tauri** | MIT |

⚠ **glTF, always.** It is the only 3D format with a sane, well-supported web pipeline. Convert
everything to `.glb` at build time, Draco-compressed.

---

## The budget, which is the whole game on mobile

| | mobile | desktop |
|---|---|---|
| Draw calls | **< 100** | < 1000 |
| Triangles | **< 150k** | < 2M |
| Texture memory | **< 100 MB** | < 1 GB |
| Real-time lights | **1-2** | 4-8 |
| Shadow casters | **1** | several |

🚨 **A mobile GPU is not a small desktop GPU.** It is tile-based, bandwidth-starved, and thermally
limited. The three things that kill you:

1. **Overdraw.** Transparent surfaces stacked on each other. ⚠ Alpha blending on mobile is brutally
   expensive ... use alpha *test* where you can, and sort front-to-back for opaques.
2. **Real-time shadows.** One shadow-casting light, maximum. **Bake everything else.**
3. **Draw calls.** Merge static geometry, use instancing for repeats, atlas your textures.

**Bake lighting into textures wherever the geometry is static.** A baked scene with zero real-time
lights outperforms and usually out-looks a dynamically lit one at this budget.

---

## Stylised beats realistic, and not only aesthetically

⚠ **Do not attempt photorealism on this track.** Flat colours, vertex colours, low-poly, toon shading
and strong silhouettes look intentional at 20k triangles. Realism at 20k triangles looks broken.

**Kenney's 3D kits (CC0) are the fastest path to a coherent 3D game** and several shipped games use
nothing else.

---

## Animation

- **Skeletal** in glTF, exported from Blender. Keep bone counts low (< 40 for mobile).
- ⚠ **Instanced/GPU skinning** if more than ~20 animated characters.
- **Vertex animation baked to texture** for crowds ... enormously faster, no skeleton at all.
- **Root motion is a trap on the web.** Drive movement from code, animate in place.

---

## Camera and controls

- **Third-person orbit:** `drei`'s controls, then rewrite when you need game-specific behaviour.
- **First-person:** pointer lock, and handle losing it (`game-input`).
- ⚠ **Camera collision is not optional in third-person 3D** and it is always underestimated. The
  camera clipping through a wall is the single most-reported bug in every 3D game ever made.

---

## Loading

3D assets are large and mobile connections are not.

- **Draco compress** meshes, **KTX2/Basis** compress textures. Together, often 10x smaller.
- **Stream, do not block.** A loading screen that sits at 0% for 8 seconds reads as a crash.
- **Budget the first load under 10 MB.** Everything else lazy.

---

## Checklist

- [ ] Draw calls and triangles measured **on a real phone**, against the table above
- [ ] One shadow-casting light maximum; everything else baked
- [ ] All models glTF, Draco-compressed; textures KTX2
- [ ] First load under 10 MB
- [ ] ⚠ Camera collision handled in third-person
- [ ] Thermal test: frame rate at minute 10 vs minute 1 (`game-perf`)
