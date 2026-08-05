# Track: `native-godot`

Native performance, every platform, MIT licensed, no royalties, no seat fees.

⚠ **The strongest choice for a serious 3D project with no budget**, and a genuinely good one for heavy
2D.

---

## Why Godot over the web stack

- **Native performance.** No WebGL ceiling, no browser memory limits.
- **One engine, every target:** Windows, macOS, Linux, Android, iOS, web, ⚠ **and console via
  third-party porting houses** (W4 Games and others ... Godot itself cannot ship console directly).
- **MIT.** No royalties, no revenue share, no licence that can change under you.
- **A real editor**, which matters more than agents expect: scene composition, animation and tilemaps
  are enormously faster in a GUI than in code.

## What it costs

- You leave the JS/TS ecosystem entirely.
- ⚠ **Agents are measurably weaker at GDScript than at TypeScript** ... less training data, fewer
  examples. Expect to check its output more carefully, and prefer C# if the project is large.
- Web export is heavy (~10-40 MB).
- Smaller hiring pool than Unity.

---

## GDScript or C#

**GDScript** ... Python-like, engine-native, fastest to write, best editor integration. ⚠ Slower at raw
computation. **Use it for game logic**, which is most of the game.

**C#** ... 5-20x faster in hot loops, better tooling, better agent output. **Use it for simulation,
pathfinding and anything numeric.** ⚠ Adds a .NET dependency and complicates web export.

**Mixing both in one project is normal and supported**, and usually correct: GDScript for scenes and
glue, C# for the expensive parts.

---

## The architecture that actually matters

**Nodes and scenes.** ⚠ Everything is a scene, and any scene can be instanced inside another. Get this
right and the engine is a joy; fight it and everything is painful.

- **One scene per meaningful thing** ... the player, an enemy, a bullet, a UI panel.
- **Compose with child nodes**, do not inherit deep hierarchies.
- ⚠ **Signals, not polling.** `signal health_changed(new_value)`. This is the engine's whole idiom, and
  code that polls other nodes every frame is code fighting the engine.
- **Autoloads for true globals only** ... save, audio, scene transitions. ⚠ Not for game state. An
  autoload holding your game state is the Godot equivalent of a global variable, and it will hurt.

```gdscript
extends CharacterBody2D
signal died

@export var speed: float = 300.0        # @export puts it in the inspector, tunable without a rebuild
@onready var sprite: AnimatedSprite2D = $Sprite

func _physics_process(delta: float) -> void:
    var dir := Input.get_vector("left", "right", "up", "down")   # ⚠ use the input MAP, never raw keys
    velocity = dir * speed
    move_and_slide()
```

⚠ **`_physics_process` is fixed timestep** (60Hz default) and `_process` is per-frame. Simulation in
`_physics_process`, always. → law 2.

⚠ **Use the Input Map** (Project Settings → Input Map), never hardcoded keys. It is Godot's intent
layer and it gives you rebinding and gamepad for free. → `game-input`.

---

## What transfers from this kit unchanged

**All of the core skills.** `game-feel`, `game-audio`, `game-design`, `game-net`, `game-ai`,
`game-economy`, `game-a11y`, `game-verdict` are engine-independent.

**What changes:** the API, not the principle. Hitstop is `Engine.time_scale = 0` for 80ms. Screen
shake is on the `Camera2D` offset. Procedural SFX is `AudioStreamGenerator` instead of ZzFX ... ⚠ or
just bake your ZzFX-designed sounds to WAV once and import them.

---

## Checklist

- [ ] Simulation in `_physics_process`, never `_process`
- [ ] Input Map used; no hardcoded keys
- [ ] Signals over polling
- [ ] Autoloads for services only, **never for game state**
- [ ] C# reserved for genuinely hot paths, if used at all
- [ ] Export templates verified for every target platform **early**, not at the end
- [ ] ⚠ Console requires a porting partner ... budget for it or drop it from the plan
