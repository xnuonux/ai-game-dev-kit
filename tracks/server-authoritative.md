# Track: `server-authoritative`

**Not an engine choice. An architecture choice, and the most consequential one in this kit.**

Composes with every other track. `2d-web` + `server-authoritative` is the most common real combination.

🚨 **Decide before P0.** Retrofitting is a rewrite, not a feature. → `skills/game-net` for the models
and the reconciliation loop; this file is the infrastructure.

---

## Do you actually need it?

⚠ **Most games that "need multiplayer" need asynchronous multiplayer**, which is a REST API and a
database and costs almost nothing:

leaderboards · ghost replays · raiding sleeping bases · turn-based-by-mail · daily challenges ·
shared world state that updates on a timer · guilds and chat

**That is 90% of the social feeling for 5% of the cost.** Build it first. Add real-time only when you
have proof people want it.

**You genuinely need real-time when:** players must see each other move; or the game is competitive and
the result matters.

---

## Stack

| role | pick | note |
|---|---|---|
| Runtime | **Node + TypeScript** | same language as the client ... shared simulation code, which is the point |
| Realtime | **WebSocket** (`ws`, `uWebSockets.js`) | uWS if you need thousands of connections |
| Room framework | **Colyseus** (MIT) | ⚠ state sync + rooms + matchmaking solved. Use it before writing your own |
| P2P / UDP-like | **WebRTC data channels** | when you need unordered/unreliable delivery |
| State | **Redis** (ephemeral) + **Postgres** (durable) | |
| Hosting | Fly.io / Railway / Hetzner | ⚠ pick a region near your players; latency is geography |

⚠ **Share the simulation code between client and server.** One `simulate(state, input)` function in a
package both import. If the client and server run different code, prediction diverges and you will
never fully fix it.

---

## Costs, honestly

**Money.** A server runs whether anyone plays or not. Budget $20-200/month before a single player
arrives, and it scales with concurrency, not with revenue.

**Time.** ⚠ **Roughly 2x the whole project.** Matchmaking, reconnection, state sync, cheating,
regions, ops, and a bug class that only appears under real latency.

**Ops.** Servers go down at 3am. If nobody is going to be on call, prefer async.

**⚠ The empty-lobby problem, which kills more multiplayer games than any technical issue.** A
real-time game with no players is unplayable, and *every* game has no players on day one. Mitigate:
bots that are honest about being bots · async modes that work solo · very short queue times over
perfect matchmaking · one region until it hurts.

---

## Non-negotiables

- 🚨 **The server owns everything that matters.** Damage, currency, inventory, results. → `game-net`.
- **Fixed timestep on both sides**, identical. → law 2.
- **Seeded RNG, seed stored per match.** → law 7.
- **Reconnection is a normal path**, not an error. Test it by killing wifi mid-match.
- **Rate-limit every message type.**
- ⚠ **Test at 200ms latency and 5% packet loss.** Localhost is a lie that hides every real bug ... use
  `tc netem`, Clumsy, or your framework's built-in simulator.

## Persistence

Player accounts, inventories and progression now live on your server, which makes you responsible for
them.

- **Never store secrets client-side.** Auth tokens with sane expiry.
- **Back up.** ⚠ Losing player progression is unrecoverable reputationally.
- **GDPR/CCPA apply the moment you store personal data.** Export and deletion must actually work.
- **An audit row for every currency grant.** When someone says "I paid and got nothing", you need it.
  → `game-economy`.

---

## Checklist

- [ ] Async proven insufficient **before** committing to real-time
- [ ] Simulation code shared between client and server
- [ ] Server authoritative over everything valuable
- [ ] Reconnection tested by killing the network mid-match
- [ ] Tested at 200ms / 5% loss, not localhost
- [ ] Rate limits on every message
- [ ] Backups, and a tested restore
- [ ] ⚠ A plan for day one, when there are no other players
