---
name: game-net
description: Use when two or more players interact. The hardest thing in this kit. Covers choosing a model (async, lockstep, client-server, rollback), what to never trust the client with, and why retrofitting networking is a rewrite.
---

# Networking

**This is the hardest thing in this kit, and the only decision here that cannot be deferred.**

Single-player code assumes one authoritative reality that updates instantly. Networked code assumes
several disagreeing realities that reconcile eventually. **Those are different programs**, and turning
the first into the second is a rewrite wearing the word "feature".

⚠ **Decide before P0 whether two people will ever interact in real time.**

---

## Pick a model

### 1. Asynchronous ... no live connection
Players affect each other, never simultaneously. Raids on sleeping bases, turn-based-by-mail,
leaderboards, ghost replays, daily challenges.

**Cost: almost nothing.** It is a REST API and a database.
**Use it whenever you can.** ⚠ **Most games that "need multiplayer" actually need this**, and it is
90% of the social feeling for 5% of the cost.

### 2. Lockstep deterministic ... send inputs, not state
Every client runs the identical simulation from the identical seed, exchanging only inputs. Classic
RTS networking.

**Cost: low bandwidth, brutal determinism requirement.**
🚨 **Every float operation, iteration order, and RNG call must match on every machine.** One
`Object.keys()` in a different order, one `Math.random()`, one platform float difference, and the
games silently diverge. Debugging a desync is genuinely one of the worst experiences in software.
**Use fixed-point maths or integers, and seed everything.** → law 7.

### 3. Client-server authoritative ... the default for anything real-time
The server simulates. Clients send input and render what they are told, predicting locally to hide
latency.

**Cost: a server, and reconciliation logic.**
**Use this for:** shooters, action games, anything competitive, anything with purchasable items.
⚠ **This is the correct default.** The other models are optimisations for specific shapes.

### 4. Rollback ... fighting-game netcode
Predict the opponent's input, roll back and re-simulate when wrong.

**Cost: very high.** Requires a fully deterministic, cheaply-serialisable, re-simulatable state.
⚠ **Do not attempt this unless the game is a fighting game or equivalently twitchy.**

---

## The rule that outranks everything

🚨 **Never trust the client.**

The client is on someone else's machine, running in a debugger they control. Everything it sends is a
suggestion.

```
client sends:  "I pressed fire at t=1234"        ✅ an input
client sends:  "I dealt 50 damage to player 3"   🚨 a claim. never accept this.
client sends:  "I bought the sword"              🚨 verify with the STORE, not the client
client sends:  "my score is 9999999"             🚨 the entire leaderboard is now garbage
```

**The server owns:** damage, health, position validation, currency, inventory, loot, match results.
**The client owns:** input, prediction, and rendering.

⚠ If your game has a leaderboard and the client reports the score, **the leaderboard is fiction**,
usually within a week of anyone caring.

---

## Prediction and reconciliation

Waiting for the server before moving feels terrible (100-200ms of input lag). So:

1. Client applies input **immediately** and locally.
2. Client sends the input with a sequence number.
3. Server simulates authoritatively, replies with state + last-processed sequence.
4. Client discards acknowledged inputs, **snaps to server state, and re-applies unacknowledged ones.**

```ts
function onServerState(state: State, lastSeq: number) {
  applyAuthoritative(state)                              // trust the server, always
  pending = pending.filter(i => i.seq > lastSeq)         // drop what it already saw
  for (const input of pending) simulate(input)           // replay the rest
}
```

**Other players are interpolated, not predicted** ... render them ~100ms in the past between two known
states. Smooth and slightly behind beats jittery and current.

---

## Practical

- **Transport:** WebSocket for most things. WebRTC data channels for peer-to-peer or when you need
  UDP-like behaviour. ⚠ **Never TCP for real-time state** ... head-of-line blocking turns one lost
  packet into a freeze.
- **Send deltas at a fixed tick (10-30Hz), not every frame.** Render rate and network rate are
  unrelated.
- **Serialise compactly.** JSON is fine at 10Hz for a small game and ruinous for 60 players.
- **Handle disconnects as a normal state**, not an error. Mobile networks drop constantly.
- **Clock sync.** Never trust `Date.now()` across machines; establish an offset and use server time.

## Anti-cheat, proportionally

- **Server authority handles 90%** of it for free.
- **Rate-limit and sanity-check every input.** Movement faster than possible is a teleport, not a
  fast player.
- ⚠ **Never ship a client-side "anti-cheat" check as your only defence.** It is code on the
  attacker's machine and it will be removed.
- Accept that a determined cheater in a small game will win. **Design so it does not ruin others'
  experience** ... that matters more than stopping them.

---

## Checklist

- [ ] Model chosen **before** P0, and written in the brief
- [ ] Server owns damage, currency, inventory, results
- [ ] Deterministic seeded RNG, stored per match
- [ ] Fixed timestep ... **networking on a variable timestep does not work**
- [ ] Client prediction + reconciliation, or documented reason for none
- [ ] Remote entities interpolated, not predicted
- [ ] Disconnect/reconnect is a normal path, tested by killing wifi mid-match
- [ ] Rate limits on every client message
- [ ] ⚠ Tested at 200ms latency and 5% packet loss, not on localhost
