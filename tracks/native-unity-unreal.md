# Track: `native-unity-unreal`

Console certification, AAA fidelity, or an existing team and codebase.

---

## ⚠ Read this before choosing

**For an agent-built game this is almost always the wrong answer.** Not because the engines are bad  ... 
they are extraordinary ... but because they assume a human in an editor.

- **The iteration loop is slow.** Compile times, editor restarts, and a scene graph that lives in a
  binary file an agent cannot meaningfully read or diff.
- **Much of the work is not code.** It is dragging things in an inspector, wiring blueprints, baking
  lightmaps. An agent cannot do that, so it produces code for a project a human must then assemble.
- **Version control is painful.** Binary scene and prefab files do not merge. Multi-agent or
  multi-human parallel work needs LFS and discipline.
- 🚨 **Licensing has changed before, retroactively.** Read the current terms yourself, today. Do not
  rely on this file, on your training data, or on a blog post.

**Choose this when:** you must self-publish to console under your own name; you need photorealism; or
a team and codebase already exist. **Otherwise `native-godot` gets you native performance and every
platform with none of the above.**

---

## Unity

**Language:** C#. ⚠ Agents write good C#, which is the single strongest argument for Unity here.

**Structure:** components on GameObjects; scenes; prefabs; ScriptableObjects for data.
⚠ **Use ScriptableObjects for tuning data.** Designers and agents can both edit them, they diff as
text, and they keep numbers out of code.

**DOTS/ECS** exists for very high entity counts. ⚠ It is a different programming model with a much
steeper curve ... do not adopt it because the entity count *might* get large.

**Watch for:** `Update()` in thousands of objects (batch them), `GetComponent` in hot loops (cache in
`Awake`), instantiation without pooling, and `FindObjectOfType` anywhere at all.

## Unreal

**Language:** C++ and Blueprints. ⚠ **Blueprints are a visual scripting graph stored as binary** ... an
agent cannot read, write, review or diff them. **Anything an agent maintains must be C++.**

**Best at:** photorealistic 3D, large worlds, Nanite/Lumen fidelity, and a genuinely superb animation
system.

**Costs:** enormous engine, long build times, C++ iteration, and a heavy asset pipeline. ⚠ The default
project is already gigabytes.

---

## Console reality

- **NDAs and approved developer status** are required before you can even read the platform docs.
- **Devkits cost money** and are allocated, not bought casually.
- **Certification is a real process** with a long checklist and rejections.
- ⚠ **Budget months, not weeks**, and expect a porting partner.

**Godot reaches console through third-party porting houses; Unity and Unreal have first-party
support.** That is the single clearest reason to choose this track.

---

## What transfers from this kit

**All of the core skills.** `game-feel`, `game-audio`, `game-design`, `game-ai`, `game-net`,
`game-economy`, `game-a11y`, `game-verdict` are engine-independent ... they are about games, not APIs.

**What changes is the API, never the principle.** Hitstop is `Time.timeScale = 0`. Screen shake is on
the camera transform. Procedural SFX still works ... ⚠ bake your ZzFX-designed sounds to WAV once and
import them.

---

## Checklist

- [ ] 🚨 Current licensing terms read **today**, from the vendor
- [ ] Console path confirmed before committing, including partner and cost
- [ ] ⚠ Unreal: agent-maintained code is C++, not Blueprints
- [ ] Unity: tuning data in ScriptableObjects, not hardcoded
- [ ] LFS configured before the first binary asset lands
- [ ] A human is available for the editor work an agent cannot do
