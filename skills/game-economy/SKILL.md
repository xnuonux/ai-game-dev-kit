---
name: game-economy
description: Use when a game has currency, a store, or IAP. How to build a real economy that converts, without the dark patterns ... and the specific reasons the honest version performs better.
---

# Economy

You can build a store that makes money without building one that makes people feel used. This file is
how, and it is written in commercial terms rather than moral ones, because the commercial case is
strong enough on its own.

---

## Pick a model and declare it

**Premium** ... one payment, everything included. No currency purchases at all. An in-game store may
exist and run on earned currency only. Cleanest product, hardest mobile discovery.

**F2P** ... soft + hard currency, ads, a pass. Real revenue, real obligations.

⚠ **Never mix them.** A premium game with a hard-currency shop is the worst of both, and players spot
it instantly and review it accordingly.

---

## The two currencies

**Soft** (coins/scrap/goo) ... earned only, spent constantly, generously granted. Its job is to make the
core loop feel rewarding.

**Hard** (gems/thread/glint) ... earned slowly OR bought. Its job is to sell time and expression.

⚠ **Hard currency buys cosmetics, convenience and content. It never buys power.** In anything with a
competitive or comparative surface, purchasable power is not a monetisation choice ... it ends the game
for everyone who did not pay, and they are the people who make it worth playing.

---

## What sells well and is honest

1. **Cosmetics.** The whole industry's best-performing category. Zero balance impact.
2. **Convenience.** Extra slots, faster crafting, more inventory. ⚠ Sells time, not advantage.
3. **Content.** Expansions, new modes. Straightforwardly a product for money.
4. **A season pass.** Fixed price, fixed rewards, ⚠ **published in full before purchase.**
5. **Remove-the-grind.** One-time unlock. Converts extremely well and generates goodwill, because it
   is honest about what the free version is.

## What to refuse, and why it also costs you money

| pattern | why it is refused | the commercial reason |
|---|---|---|
| **Energy timers** | punishes playing | caps session length, which caps everything downstream |
| **Loot boxes without odds** | is gambling | illegal in several markets; delisting risk |
| **Purchasable power in PvP** | ends the game for non-payers | kills the population the payers are paying to beat |
| **Offers at loss moments** | preys on frustration | converts *worse* than offers at win moments |
| **Fake countdowns** | is a lie | one screenshot on Reddit and it is your permanent reputation |
| **Cancellation mazes** | traps people | app store policy violation, chargebacks, one-star reviews |
| **Streak punishment** | punishes having a life | the strongest predictor of a rage uninstall |

⚠ **Published odds are non-negotiable**, on the purchase screen, in the same font size as the price.
Not in a submenu, not in the EULA.

---

## Implementation

**⚠ The client is not the source of truth for anything you sold.** Local saves are editable. Fine for
progression. Not fine for premium currency.

- **Verify receipts server-side.** Both stores provide a validation API. Without it, a trivial
  client patch gives away everything you sell.
- **Grant idempotently.** Network drops mid-purchase. Users get double-charged or nothing. Use a
  transaction id and make replaying it a no-op.
- **Restore purchases must work.** Store policy, and the top support complaint when broken.
- **Log every grant.** When someone says "I paid and got nothing", you need the row.

```ts
async function grant(txId: string, sku: string, userId: string) {
  if (await alreadyGranted(txId)) return                 // idempotent
  const ok = await verifyWithStore(txId, sku)            // ⚠ server-side, always
  if (!ok) { log.warn('[IAP] failed verification', { txId, sku, userId }); return }
  await applyEntitlement(userId, sku)
  await recordGrant(txId, sku, userId)                   // the receipt row
}
```

---

## Tuning without exploiting

**Measure:** day-1/7/30 retention, session length, conversion rate, ARPDAU, and ⚠ **refund rate and
review sentiment** ... the two everyone omits and the two that catch a predatory economy early.

**The test to apply to any change:** *would I be comfortable if a player could see exactly why this
offer appeared right now?*

⚠ If the honest explanation is "because you just lost three times and we think you are frustrated",
the mechanic is the problem. If it is "because you just cleared a hard level and you are enjoying
yourself", it is fine ... and it converts better anyway.

---

## Checklist

- [ ] Model declared, not mixed
- [ ] Hard currency never buys power
- [ ] Odds published at the point of purchase
- [ ] Server-side receipt verification
- [ ] Idempotent grants, working restore
- [ ] No energy, no streak punishment, no fake timers
- [ ] Cancellation is as easy as subscribing
- [ ] Refund rate and review sentiment on the dashboard
