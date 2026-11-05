---
created: 2026-11-05
owner: JasonEyeglass
tags: [plan, design, merge, proposal]
status: proposal
---

# Merge Proposal: The Legacy Cycle + Action MMO plan (draft v0.1)

A draft of the "possible merge" from [[Chat Import Hub - Roblox Action MMO]]. It keeps the Legacy Cycle story and world ([[Story Foundation]], [[Legacy System Spec]], [[Prologue Script]], [[Act 1 Quest Outline]]) and adds the parts of [[Game Plan v0.1 - Action MMO]] that fill its gaps. **Nothing here is decided.** If the team adopts it, log one line in [[Decisions|Decisions Log]].

## The merge in one line
**Every fallen hero lives on as an Echo in your Ember.** Beat a boss and you carry their power and their memory. Carry too many, for too long, and you fall like they did, so you pass the Ember on.

## What we keep, take, and drop

| Topic | Keep from Legacy Cycle | Take from the Action MMO plan | Drop |
| --- | --- | --- | --- |
| Story and world | The Cycle, the Void, the Ember, the Keeper, Ser Caelan, Havenbrook, the 6 regions, the 3 seals | The "forgetting" theme, the chapter formula, the mystery ledger, the Masked Figure | Shattered Sky, floating islands, the Hush, the Wakeling |
| Powers | The Ember skill, the boss memories | Echoes (4 moves + 1 ultimate), weapon styles, mastery, awakened forms | "Weapons and skills" as a separate system |
| Legacy | Retirement, family flags, heirlooms, bloodline traits, reputation | — | — |
| Content | Prologue, Act 1, the Old Gate dungeon, the knight choice | Arena (1v1, free-for-all), raid, daily missions, 6-week chapters | Open-world PvP (both plans already say no) |
| Tone | Dark fantasy, "Your Choices. Their Tomorrow." | Aim for the **Mild** rating: no blood, enemies break into ash and embers | Gore |
| Platform | Parry tested at 150–250 ms ping | Phone first, 8 buttons max, soft lock-on, VFX budget | — |
| Money | One currency, no premium currency, cosmetics only, purchases never lost ([[Economy and Monetization]]) | A battle pass per chapter with fixed rewards | — |
| Tools | — | Rojo + git from the start, a Roblox group owns the game | "Team Create first, Rojo later" |
| Team | 5 people ([[Team and Roles]]) | An integrator, the ownership map, weekly play-tests | The 4-role split |

## The story, merged

**Why the Cycle repeats: the world forgets.** When a hero falls to the Void, the world forgets who they were. Their names leave songs, their statues crumble, and their families stop remembering them. That's why each new generation makes the same mistake. Only an Ember can hold what the Void erased.

This takes the strongest idea from the Shattered Sky draft (you can remember what everyone else forgets) and gives it a reason inside the Legacy Cycle story:

- **Echoes are the forgotten heroes**, held in your Ember. Each one gives you their fighting style and, as you master it, their memory.
- **Boss memories become Echo memories.** The Fallen Knight's memory from Act 1 is the first full Echo memory. The two plans already had this idea; now it's one system.
- **The Ember's corruption has a cause.** Every Echo you carry is a hero who held on too long, and carrying them weighs on your Ember. This answers the open question in [[Story Foundation]] about what corruption does, and gives the story a reason to retire.
- **The family chronicle becomes "the family remembers."** Your bloodline is the one family the Void can't make forget. The heir inherits the Ember, so the heir remembers what the ancestor saw.
- **The saga mystery:** *why can an Ember remember what the Void erases, and who was the first hero the world forgot?* This fits the existing question about whether the Void Sovereign is the first hero.

### The Masked Figure (optional)
The Action MMO plan's best cliffhanger device fits here too: a masked stranger who appears at Shrines of Embers and takes Echoes from fallen bearers. Suggested truth (for the private story bible only): a hero from an earlier Cycle who never retired and never fell, because they feed on other heroes' Echoes. They are the opposite of the legacy system, holding on forever by taking from others. Payoff at a season finale.

### What changes in the Prologue and Act 1
Very little. The [[Prologue Script]] already does the job of the Shattered Sky cold open.

| Beat | Today | With the merge |
| --- | --- | --- |
| Prologue end | Caelan's Ember escapes and finds the player | Same. The Keeper's line can add: "When he falls, the world will forget his name." |
| Act 1.1 at the Shrine | The player learns about the Ember | The player picks **1 of 3 starter Echoes**: minor heroes the world forgot, already in this Ember |
| Act 1 villagers | Nobody knows who Caelan was | Make this a beat: the town has forgotten its own champion. Only the player knows. |
| The knight choice | Save Bryn, take his sword, or leave him | Unchanged. If he's left and becomes the mini-boss, his Echo drops, and taking it is a dark choice that sets a family flag. |
| Fallen Knight | Boss + memory | Boss + memory + **Caelan's Echo** (guaranteed). The Keeper remembers his name again. |
| Act 1 end | The road to the Ethereal Forest | Same, plus the Masked Figure watching from the gate (if adopted) |

Chapter 1 - Lanternfall's emotional beat (a grandmother forgetting her granddaughter) can be reused in Havenbrook as a side quest if the team likes it. Its cast and island are dropped.

## Powers: Echoes plus weapon styles, inside the Ember

- **Weapon style** = your basic combo, dash, block and parry. Launch with 3 (sword, spear, gauntlets; scythe later).
- **Echo** = 4 moves + 1 ultimate, one role (damage, control, support, tank). Equip one at a time. The GDD's "one magic ability" (Ember Strike) stays as a shared move every Echo has.
- **Mastery** unlocks the moves, then the full memory, then the awakened form.
- **Where Echoes come from:** region bosses (guaranteed), mini-bosses, hidden quests, events. **Never sold.** No paid rolls.
- **Launch set (6):** 3 starters, the Fallen Knight (Caelan), Bryn's Echo (only if he falls), 1 hidden Echo in Havenbrook.
- **Phone rule:** M1, dash, block/parry, 4 Echo moves and 1 ultimate: 8 buttons.

### How Echoes work with the legacy system
This is the main design question the merge creates. Proposal, following the spec's rule that **an heir should feel different, not stronger**:

| | Passes to the heir? | Why |
| --- | --- | --- |
| Echoes collected | **Yes**, they live in the Ember | The story says the Ember carries them. Long-term collection goal for the bloodline. |
| Echo mastery | **No**, resets to 0 | Power comes back through play, as the spec says. The heir has to learn each style again. |
| Unlocked memories | **Yes**, they go in the family chronicle | Story carries over, not power. |
| Echo corruption | **No**, the heir starts with a clean Ember | This is the reward for retiring in time. |

**Data model addition** (to the sketch in [[Legacy System Spec]]):
```
bloodline
  echoes: { [echoId]: { memoriesUnlocked: number } }
character
  equippedEcho: echoId
  echoMastery: { [echoId]: number }
  corruption: number
```

## Content and release plan

The six GDD regions become the chapter unit instead of islands. Each region is one chapter: one boss, 1–2 Echoes, one battle pass.

| Release | Region | Notes |
| --- | --- | --- |
| Launch (v1.0) | Prologue + Havenbrook | Act 1, the Old Gate as the raid-sized dungeon, arena, shop, battle pass |
| Chapter 2 (~6 weeks later) | Ethereal Forest | The Hollow Guardian |
| Chapter 3, Season 1 finale | Frostveil Mountains | The Frostbound King. Big reveal. |
| Season 2 | Ashen Wastes, Shadowlands | First seal earned here. Retirement fully live. |
| Season 3 | Forgotten Realm | Three seals, three generations, the Void Sovereign |

**Arena:** uses the same combat and Echoes. Legacy flags don't touch it. Lore hook: the guild's training ring under Orla Venn.

**Daily missions** (story, combat, arena) fill the "reasons to come back" gap in [[GDD v0.2 Review]].

## Money
Keep everything in [[Economy and Monetization]]. Add one product:

- **Battle pass, one per chapter**, about 5 weeks plus about 1 week of rest. About 10 tiers, a free track, fixed rewards only, no penalty for buying late, completable at 80–85% of a 5-day-a-week player's XP.
- Rewards fit the legacy theme: Ember colors, family crests, heirloom skins, Echo skins (looks only).
- Like all purchases, pass rewards belong to the account and survive retirement.

## Roadmap: phases mapped onto milestones A–E

| Phase (Action MMO plan) | Milestone ([[Project Plan Overview]]) | Gate |
| --- | --- | --- |
| 1. Pre-production, weeks 1–3 | — | **Pitch locked:** this merge (or another option) agreed and logged |
| 2. Combat prototype, weeks 4–9 | A. Combat sandbox | **Fun test** on a phone, at 150–250 ms ping: sword + 2 Echoes + 1 enemy |
| 3. Vertical slice, weeks 10–17 | B. Regional slice + D. Legacy proof | Prologue + first 15 min of Act 1. Knight choice saved in `bloodline.familyFlags`. |
| 4. Launch build, weeks 18–28 | C. Co-op dungeon | Old Gate, Fallen Knight, arena, 3 weapon styles, 6 Echoes, shop, battle pass |
| 5. Closed beta, weeks 29–32 | E. Alpha readiness | Runs well on a mid-range phone, maturity questionnaire done |
| 6. Live | — | A chapter every 6 weeks. Retirement shrine ships before the first seal (Season 2). |

Timeline assumptions from [[Game Plan v0.1 - Action MMO]] still apply (about 15 hours a week each, starting Nov 9, 2026, launch late June 2027). With 5 people instead of 4 there's a bit more room, which the team can spend on the retirement shrine.

## Team: 5 roles with the Action MMO plan's working rules

| Role ([[Team and Roles]]) | Takes from the 4-role plan | Owns |
| --- | --- | --- |
| Lead / producer + **integrator** | A's integrator and release work, D's community and play-test lead | Scope, merges, weekly build, Discord, clips, analytics |
| Gameplay & server programmer | A's combat work | Combat, Echo move data format, saves, server checks |
| Systems & narrative design | B's story half | Story bible, mystery ledger, quests, legacy rules, battle pass tiers |
| Environment & level design | B's world half | Havenbrook, Old Gate, arena, enemy placement |
| Art | C, plus D's UI | Models, animation, VFX, mobile UI |

Audio has no owner yet. Free libraries at launch, or one person adds it.

[[Team Tasks - Weeks 1-9]] works with these 5 roles once the letters are re-mapped.

## Open questions for the team
- [ ] Adopt this merge, the Legacy Cycle plan as is, or the Shattered Sky plan?
- [ ] Art style: anime-styled dark fantasy, or keep a more grounded dark-fantasy look?
- [ ] Do collected Echoes pass to the heir (this draft says yes, mastery resets)?
- [ ] Does carrying Echoes add corruption, or does corruption come only from fighting the Void?
- [ ] Is Bryn's Echo (taken from a fallen ally) too dark for a Mild rating, or a good choice?
- [ ] Keep the Masked Figure?
- [ ] Re-use Chapter 1 - Lanternfall's grandmother story as a Havenbrook side quest?
- [ ] Switch to Rojo + git from the start (this reverses the "Team Create first" plan in [[Project Plan Overview]])?
