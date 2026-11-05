---
created: 2026-11-03
owner: JasonEyeglass
tags: [economy, monetization, design, draft]
status: draft
---

# Economy and Monetization (draft v0.1)

How players earn and spend in [[Home|The Legacy Cycle]], and how the game makes money. Fills the gap flagged in [[GDD v0.2 Review]]. Builds on the [[Legacy System Spec]] (what survives retirement) and [[Competitor Baseline - Rogue Lineage and Deepwoken]] (both charge an entry fee; we don't).

**Two rules everything below follows:**
1. **Nothing you can buy makes you stronger.** No paid stats, gear, XP boosts or revives. The GDD already says cosmetics over pay-to-win.
2. **Nothing you buy is ever lost.** Purchases belong to the account, not the character, so they survive death and retirement. This makes buying feel safe in a game built on starting over.

---

# Part 1: The economy

## Currencies
Keep it to **one currency plus materials**. More currencies confuse new players, especially on mobile.

| Resource | What it is | Kept on retirement? |
|---|---|---|
| **Gold** | The only currency. Earned in play, never sold for Robux. | 10% passes to the heir as an inheritance (spec says "a small inheritance") |
| **Materials** | Region-specific: e.g. Havenbrook iron, Forest moonwood, Frostveil ice-crystal, Ashen ore. Used for upgrades and crafting. | No |
| **Renown** | Not spent. Reputation with factions; it carries over at 50% per the spec. | 50% |

There is **no premium currency** (no "gems"). Cosmetics are priced directly in Robux. Players and parents always see the real price, and there's no leftover-currency trick.

## Where gold comes from (sources)
| Source | Share of income (target) | Why |
|---|---|---|
| Quests (main and side) | ~40% | Finite, so it can't be farmed endlessly. Rewards playing the story. |
| Dungeon chests | ~25% | Rewards co-op. First clear per week pays a bonus. |
| World events and world bosses | ~15% | Rewards showing up and joining others |
| Selling materials and junk to NPCs | ~15% | Rewards exploration |
| Enemy drops | ~5% | Kept small on purpose so grinding mobs isn't the best strategy (GDD: avoid level grinding) |

## Where gold goes (sinks)
| Sink | Notes |
|---|---|
| **Gear upgrades** | Main sink. Gold + region materials. Materials from later regions make players explore. |
| **Repairs** | Connects to the death penalty in the spec (durability loss). Cheap, but constant. |
| **Crafting** | Consumables (potions, Ember charges) and gear. |
| **Heirloom engraving** | Passing on an heirloom at retirement costs gold, scaling with the item's power. Same idea as Deepwoken's 40-Echo cost for legendary items. |
| **Retirement** | The biggest sink: 90% of gold is left behind. Spending gold before retiring (upgrading the heirloom, buying cosmetics in gold shops) is a natural last goal for a character. |
| **Gold cosmetics** | Some dyes and simple outfits sell for gold, so non-paying players get cosmetics too. |

**No player trading at launch.** The GDD defers trading. That's also the strongest protection against item duplication bugs and selling items for real money. Revisit after alpha.

## Starting numbers (placeholders for the first playable build)
These are guesses to start tuning from, not balanced values.

| Item | Value |
|---|---|
| Main quest reward | 100–150 gold |
| Side quest | 40–80 gold |
| Dungeon clear (each player) | 120 gold + materials; first clear each week +100 |
| Enemy drop | 1–5 gold |
| First equipment upgrade | 200 gold + 10 Havenbrook iron |
| Repair after a death | ~10% of the item's upgrade cost |
| Target | A new player affords the first upgrade in about 30–40 minutes |

---

# Part 2: Monetization

## Free to play
No entry fee. Rogue Lineage (200 Robux) and Deepwoken (400 Robux) can charge because they are established. A new game needs players first. Free entry also means more players in shared regions, which the world events need.

## What we sell
| Product | Roblox type | Example price | Notes |
|---|---|---|---|
| **Armor and weapon skins (appearance changes)** | Developer product, saved as owned on the account | 99–399 Robux | Changes looks only. Main product line. One set per region theme. |
| **Ember colors** | Developer product | 149 Robux | Changes the color of the Ember VFX (gold by default). Fits the story and is visible in every fight. |
| **Family crest and banner** | Developer product | 199 Robux for crest designs, 99 for colors | **Bloodline-themed: the crest stays across all generations.** Shows on capes, shields, the ancestor hall. Best fit for the legacy theme. |
| **Heirloom skins** | Developer product | 249 Robux | A look for heirloom weapons, kept forever as the family weapon. |
| **Emotes** | Developer product | 49–99 Robux | Cheap first purchase. |
| **Private servers** | Built-in Roblox feature | ~100 Robux/month | Friends play shared regions together. Little work to add. |
| **Seasonal "Chronicle" pass** | Developer product | ~399 Robux per season | **Later, not at launch.** Free track + paid track of cosmetics only, unlocked by playing. |

## Free cosmetics too
Some of the best looks are **earned only through play**: boss-kill armor sets, a crest border for each generation completed, an Ember color for breaking the Cycle. That way a cosmetic means "I did this", not only "I paid".

## Other income
- **Premium Payouts:** Roblox pays experiences based on how long Roblox Premium subscribers spend in them. A good main story and coming-back reasons increase this with no extra work.

## What we will never sell
| Never | Why |
|---|---|
| Stats, gear, XP or gold boosts | Pay-to-win breaks co-op fairness and the "not stronger" rule |
| Paid revives or skipping the death penalty | Deepwoken sells a 1,000-Robux revive; we keep death fair for everyone |
| Loot boxes or paid random rewards | Our audience is young. Roblox also has rules about paid random items; every price stays fixed and visible. |
| Extra legacy perks (traits, heirlooms) | The legacy system must be earned through deeds |
| "Limited time, buy now!" pressure | Seasonal items can rotate, but no countdown pressure aimed at kids |

## Gray areas for the team to decide
- [ ] **Inventory or wardrobe space.** Convenience, not power, but can feel like pressure. Suggest: generous free space, extra wardrobe slots for cosmetics only.
- [ ] **Extra character slots.** Only matters if the team allows several heirs side by side (open question in the spec).
- [ ] **Rewarded video ads.** Roblox supports opt-in ads with a small reward. Probably skip: it doesn't fit the tone.

## Launch set (first public release)
Keep it small: **5–8 cosmetic items, private servers, Premium Payouts.** That's enough to test what sells without slowing down the game. Add crests, heirloom skins and the Chronicle pass once the legacy system ships.

---

## Implementation notes (for the gameplay programmer)
- **Profile changes:** add an `account` section next to `bloodline` and `character` in the spec's data model: `account.ownedCosmetics`, `account.processedPurchases`, `account.crest`. Retirement never touches `account`.
- **Purchases must be processed exactly once.** Roblox's `MarketplaceService.ProcessReceipt` can be called more than once for the same purchase. Save the `PurchaseId` in `account.processedPurchases` and only return `PurchaseGranted` after the save succeeds. Getting this wrong means players pay and get nothing, or get items twice.
- **Server only.** Gold, materials and ownership change only on the server, validated like everything else (GDD section 9).
- **Logging:** log every gold source and sink from day one, so balancing uses real numbers instead of guesses.

## Open questions
- [ ] Are the example Robux prices right? Check prices of similar cosmetics in top Roblox RPGs before launch.
- [ ] 10% gold inheritance: too much or too little?
- [ ] Should gold shops sell some outfits, or only dyes?
- [ ] When does player trading come back, and with what limits?
