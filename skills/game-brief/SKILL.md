---
name: game-brief
description: Use before writing any game code. Produces the one-page brief that the finished game is later measured against. The spec is the test.
---

# The brief

**No code until this exists.** An agent that starts from "make a tower defence game" builds a demo:
technically fine, no loop, no reason to play twice.

The brief is short on purpose. One page. Fill every field — **"TBD" in any field means you do not yet
know what you are building**, and building it will be the expensive way to find out.

---

## The template

```md
# <NAME>

**one line:**            what it is, in a sentence a stranger understands
**session length:**      90 seconds / 5 minutes / 30 minutes  (this drives everything)
**platform:**            mobile portrait / mobile landscape / desktop / both
**model:**               premium $X / F2P / free

## the verb
The one thing the player does, constantly. Not "manage resources" — "drag a tower onto the rim".
If you cannot name a single physical verb, the game does not exist yet.

## the loop
1. …
2. …
3. … back to 1
Each step under ten words. If a step needs a paragraph, it is two steps.

## the failure state
What losing looks like, and what it costs. "Nothing" is a valid answer (see the toy genre)
but it must be a DECISION, not an omission.

## progression
What is different tomorrow. The reason to open it again.

## the one thing
What this does that nothing else does. One sentence.
⚠ If it is "it's like X but prettier", stop and find the real answer.

## the anti-goals
What this is deliberately not. Three items minimum.
This section prevents more waste than every other section combined.

## verification
3-5 things that must be TRUE, measurable by someone who was not in the room.
Not "it's fun". "A first-time player survives to wave 10 with no tutorial."
```

---

## The rules

**The session length drives every other decision.** A 90-second game cannot have a menu. A 30-minute
game needs a save. Decide it first and let it cascade.

**The verb must be physical.** Drag, tap, hold, swipe, aim. "Strategise" is not a verb, it is a
feeling produced by verbs.

**The anti-goals are load-bearing.** Every game grows features during the build. The anti-goals are
the only thing that says no, and an agent will otherwise say yes to all of them.

**Verification must be checkable by a stranger.** "It's fun" is not a test. "8 of 10 first-time
players reach minute two without instruction" is. See `skills/game-verdict`.

---

## The brief IS the test

At the end, the finished game gets held against this document, field by field. That is the whole
point of writing it: **it turns "is it good?" from an opinion into a comparison.**

If the built thing no longer matches the brief, one of them is wrong. Say which, out loud, and change
it deliberately. ⚠ **Do not silently update the brief to match what you built** — that is how a
project loses the ability to fail, and a project that cannot fail cannot succeed either.
