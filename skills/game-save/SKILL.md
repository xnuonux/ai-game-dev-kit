---
name: game-save
description: Use when adding persistence. Schema versioning and migration from the first commit, because the first patch after launch is where hobby games die.
---

# Saves

**The save file has a version number from the very first commit.** Not when you need it — then. It is
two lines on day one and it is unrecoverable on day ninety.

The failure is specific and universal: you ship, players play for a week, you add a feature that
changes the save shape, and every existing player either loses their progress or crashes on launch.
There is no fix after the fact, only apology.

---

## The shape

```ts
import { z } from 'zod'

const CURRENT = 3

const SaveV3 = z.object({
  version: z.literal(3),
  coins: z.number().int().min(0),
  unlocked: z.array(z.string()),
  best: z.object({ wave: z.number().int(), time: z.number() }),
  settings: z.object({ muted: z.boolean(), reduceShake: z.boolean() }),
})
type Save = z.infer<typeof SaveV3>

const FRESH: Save = {
  version: 3, coins: 0, unlocked: [],
  best: { wave: 0, time: 0 },
  settings: { muted: false, reduceShake: false },
}
```

## Migration, forward only

```ts
const migrations: Record<number, (s: any) => any> = {
  1: (s) => ({ ...s, version: 2, best: { wave: s.bestWave ?? 0, time: 0 } }),
  2: (s) => ({ ...s, version: 3, settings: { muted: false, reduceShake: false } }),
}

function migrate(raw: any): Save {
  let s = raw
  while (s.version < CURRENT) {
    const step = migrations[s.version]
    if (!step) throw new Error(`no migration from v${s.version}`)
    s = step(s)
  }
  return SaveV3.parse(s)      // ⚠ validate AFTER migrating, always
}
```

⚠ **Never delete an old migration.** A player who has not opened the game in a year still arrives at
v1. That function is load-bearing forever.

## Loading

```ts
export function load(): Save {
  try {
    const raw = localStorage.getItem(KEY)
    if (!raw) return { ...FRESH }
    const parsed = JSON.parse(raw)
    if (typeof parsed?.version !== 'number') return { ...FRESH }
    const save = migrate(parsed)
    localStorage.setItem(BACKUP_KEY, raw)   // keep the pre-migration original
    return save
  } catch (e) {
    console.error('[SAVE] load failed, starting fresh:', e)
    return { ...FRESH }                      // ⚠ never crash on a bad save
  }
}
```

## The rules

**Validate on load, always.** A corrupt save that throws loudly is better than one that half-loads and
produces a player with `NaN` coins and an unexplainable bug report.

**Write on a debounce, not every frame.** ~2s, plus immediately on `visibilitychange` — mobile kills
backgrounded apps without warning.

```ts
document.addEventListener('visibilitychange', () => { if (document.hidden) saveNow() })
```

**Keep a backup of the last good save.** One extra key. It is the difference between a bad review and
a recoverable support message.

**localStorage under ~1MB; Dexie/IndexedDB above.** Do not put a large event log in localStorage —
it is synchronous and it will stutter your frame.

**Never trust the save for anything that matters commercially.** It is on the player's device and it
is editable. Fine for progress. Not fine for premium currency you sold. See `skills/game-economy`.

---

## Checklist

- [ ] `version` present from commit one
- [ ] A migration test per version, run in CI
- [ ] Load never throws — corrupt input yields a fresh save
- [ ] Save on `visibilitychange`
- [ ] Backup key written before migration
- [ ] ⚠ Verified: a v1 save from launch day loads correctly in the current build
