---
created: 2026-11-03
owner: unassigned
tags: [review, gdd, design]
source: The_Legacy_Cycle_Updated_Open_World_GDD_v0.2.docx
---

# GDD v0.2 Review

Review of the game design document v0.2 for [[Home|The Legacy Cycle]].

## What works
- **Scope discipline.** The minimum playable build, the milestones A–E and the "don't build six regions first" rule are the strongest part of the doc.
- **Honest technical risks.** Teleport reliability, data consistency across places and reconnects are the real Roblox problems, and the doc names them.
- **Multiplayer choice rule.** Keeping choices per player, using votes only for mission outcomes and not promising a shared changing world is the correct call for Roblox.
- **Combat-first milestone.** If combat doesn't feel good, nothing else matters, so it is right to build it first.

## Main concerns
1. **The hook is the thing being deferred.** The legacy/generation system gives the game its name, but the first build only proves "one choice changes one quest", which most RPGs already do. Without the generational part, this reads as a standard Roblox fantasy RPG in a crowded genre. Define what passes between generations now, even if it is built later.
2. **Compare against existing Roblox games.** Roblox already has RPGs built around lineage, permadeath and wipes, such as Rogue Lineage and Deepwoken. Study what they carry between characters and how this game differs. The doc's own section 13 asks for this; do it before building the legacy system.
3. **Per-player consequences in shared regions.** The "corrupted knight" outcome needs either a private story instance or per-player NPC visibility. Decide which consequences live in shared regions and which only in instances.
4. **Parry and dodge over network lag.** Server-authoritative combat with tight parry windows feels bad at mobile ping. Plan client-side prediction with server checks that allow some timing tolerance, and test at 150–250 ms ping in milestone A.
5. **The first playable build is still big for two people.** It includes two regions, teleporting, a dungeon, a boss, 3 enemy types, inventory and persistence. Possible cut: build Havenbrook plus the dungeon only, and add the forest gateway in milestone B. A risky teleport can still be tested early through the dungeon entry.

## Missing from the doc
- **Why players come back:** daily or weekly reasons to return, and what happens after the boss is beaten.
- **Death penalty:** what dying costs the player, which ties directly into the legacy system.
- **Economy:** currency sources and spending.
- **Monetization:** concrete cosmetic items.
- **First 5 minutes:** how a new player is introduced to the game.
- **Numbers:** level ranges, time to finish each region, boss health and how it scales with party size.

## Suggested next steps
- Write a one-page spec for the legacy system.
- Lock the scope of the first playable build.
- Start milestone A (the combat sandbox) with a network-lag test.


Drafted: [[Legacy System Spec]]
