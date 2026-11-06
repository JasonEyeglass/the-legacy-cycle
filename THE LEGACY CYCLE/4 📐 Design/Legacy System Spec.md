---
created: 2026-11-03
owner: unassigned
tags: [spec, legacy, design, draft]
status: superseded
updated: 2026-11-06
---

# Legacy System Spec (draft v0.1)

> [!warning] Superseded 2026-11-06: no heirs, no permadeath
> Each player keeps **one character** for the whole game. **There is no permadeath and no hardcore mode.** Retirement, the retirement ceremony, heirs, heirlooms, bloodline traits, reputation carry-over and the ancestor hall are cut. See [[Decisions|Decisions Log]] (2026-11-05 "No heirs").
> **Still valid:** per-player story flags (the knight choice), the death penalty below, and the multiplayer rules below. Everything about retirement and heirs further down is kept only for history.

One-page spec for the signature mechanic of [[Home|The Legacy Cycle]]. It answers concern #1 in [[GDD v0.2 Review]]: decide what carries between generations now, even if it is built later. Everything here is a proposal for the team to accept, change or reject.

## The pitch in one line
**Your character's choices become your family's reputation.** The world remembers your bloodline, not just your character.

## How it differs from Rogue Lineage and Deepwoken
| | Rogue Lineage / Deepwoken | The Legacy Cycle (proposed) |
|---|---|---|
| What starts a new generation | Death (lives run out or permadeath) | **The player chooses to retire** a character, usually after a story milestone |
| What carries over | Mostly power: stats, traits, items, unlocks | **Story first:** NPCs, factions and quests react to what your ancestors did |
| How it feels | Loss, risk, grinding back | Choosing what to pass on and what your heir must live with |

Death is not the trigger. This keeps the game friendly for casual and mobile players, and it is the clearest difference from both games. Checked against web research in [[Competitor Baseline - Rogue Lineage and Deepwoken]]. One correction: Deepwoken also lets players wipe by choice, so what sets us apart is what carries over and the story reason for retiring (see [[Story Foundation]]). Hands-on play is still to do.

## The loop
1. Play a character: quests, choices, gear, renown.
2. Reach a **retirement point**. Proposed: after defeating any region boss, at a shrine in Havenbrook. It is never forced.
3. **Retirement ceremony:** the player sees a summary of the character's deeds and picks what to pass on.
4. The heir starts at level 1 in Havenbrook, but the world is different for them.

## What carries over
| Category | What the heir gets | Example |
|---|---|---|
| **Family flags** (automatic) | Key choices become bloodline facts that NPCs and quests read | Ancestor saved the knight, so the knight trains the heir. Ancestor left him, so the corrupted knight hunts the family. |
| **Reputation** (automatic) | Faction standing carries over at 50% | The guild trusts your name, and the Shadowlands cult remembers it |
| **Heirloom** (pick 1) | One item passes down and scales to the heir's level, with an engraved history | "Sword of Aldric, who spared the Frostbound King" |
| **Bloodline trait** (pick 1) | Small passive trait unlocked by the ancestor's deeds | Ancestor parried 500 attacks, so the heir gets +5% parry window |
| **Map knowledge** (automatic) | Discovered routes and shrines stay revealed | Faster travel early on |

**What does not carry over:** level, inventory, currency (except a small inheritance), and skill points. Power comes back through play, not through retiring.

Balance rule: an heir should feel different, not stronger. Gate traits and heirlooms behind deeds, not playtime, so retiring early gives no advantage.

## Death penalty (current character)
Death is never permanent. On death: respawn at the last shrine, gear durability loss and an unclaimed-loot drop the player can recover. **No permadeath and no hardcore mode** (cut 2026-11-05).

## Multiplayer rules
- All legacy data lives on the player's profile. Shared regions never change permanently. This follows GDD section 6.
- Family-flag consequences show up in dialogue, private quests and story instances only. Example: the corrupted knight fight is a private instance, or an NPC that only that player can see. This resolves concern #3 in the review: **anything that depends on legacy flags appears only in instances or as per-player visible NPCs.**
- In a party, the dungeon uses the host's story state. Each member keeps their own flags and rewards.

## Data model (sketch)
**Current (2026-11-06):** no `bloodline`. Story flags live on the one character:
```
Profile (versioned, DataStoreService)
  character
    level, xp, inventory, currency, questState
    storyFlags: { [flagId]: value }   -- e.g. knight_fate = "saved"
    reputation: { [factionId]: number }
```
Full profile: [[Merge Proposal - Legacy Cycle and Action MMO]].

Old heir version, kept for history:
```
Profile (versioned, DataStoreService)
  bloodline
    generation: number
    familyFlags: { [flagId]: value }   -- e.g. knight_fate = "saved"
    reputation: { [factionId]: number }
    traits: { traitId }
    heirlooms: { itemId, history }
    deeds: { [deedId]: count }         -- feeds trait unlocks
    ancestors: { name, generation, summary }
  character
    level, xp, inventory, currency, questState, localFlags
```

**Important for milestone D:** store the knight choice in `bloodline.familyFlags` from day one, not on the character. Retirement can then be added later without migrating save data. Retirement itself is one atomic profile update: archive `character` into `ancestors`, apply the picks, then create a fresh `character`.

## Build order
1. **Milestone D (no change in scope):** knight choice saved in `character.storyFlags`, read by one later NPC.
2. ~~After alpha: deeds counter, reputation, retirement shrine, ceremony UI, one heirloom and three traits.~~ Cut.
3. ~~Later: ancestor hall, more traits, hardcore mode.~~ Cut.

## Open questions
- [ ] Should retirement be possible before the first boss, or only after it?
- [ ] One active character per player, or several heirs side by side?
- [ ] Does the heir play as a direct child or as a chosen apprentice? This affects character customization.
- [ ] Is 50% reputation carry-over the right amount? It needs playtesting.
- [ ] Is there a monetization link (cosmetic ancestor portraits, heirloom skins) that stays non-pay-to-win?

## Proposals
**JasonEyeglass, 2026-11-05: cut heirs and retirement.** ✅ Applied 2026-11-06 (banner, death penalty, data model, build order). Logged in [[Decisions|Decisions Log]]. The heir loop is too complicated for younger players, so each player keeps one character for the whole game. If the owner agrees, mark this spec as superseded:
- Cut: retirement, the retirement ceremony, heirs, heirlooms, bloodline traits, reputation carry-over, the ancestor hall, hardcore permadeath.
- Keep: per-player story flags (the knight choice), the death penalty, and the multiplayer rule that flag-driven content shows only in private instances or per-player NPCs.
- Data model: drop `bloodline`; move `familyFlags` to `character.storyFlags`. Full profile in [[Merge Proposal - Legacy Cycle and Action MMO]].
- "Legacy" now means the heroes before you, told through playable boss and Echo memories.
