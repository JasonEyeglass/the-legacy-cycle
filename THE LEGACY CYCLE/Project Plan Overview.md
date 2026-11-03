---
created: 2026-11-03
owner: JasonEyeglass
tags: [hub, plan, overview]
---

# Project Plan Overview

Everything planned so far for [[The Legacy Cycle]] on one page. Start here if you are new to the project. Each section links to the full note. Status of all of this: **proposals**, until the team logs them in [[Decisions Log]].

## 1. The game
Roblox open-world dark-fantasy action RPG. Tagline: *"Your Choices. Their Tomorrow."* Source: `The_Legacy_Cycle_Updated_Open_World_GDD_v0.2.docx` in the repo root.

- Six regions, each a separate Roblox place: Havenbrook, Ethereal Forest, Frostveil Mountains, Ashen Wastes, Shadowlands, Forgotten Realm.
- Private dungeons for 1–4 players.
- Action combat: attacks, dodge, parry, weapon skills.
- Signature mechanic: choices carry over to the next generation of your family.

## 2. Review of the GDD
Full note: [[GDD v0.2 Review]]
- **Strong:** scope control (milestones A–E), honest technical risks, per-player choices instead of a shared changing world, combat first.
- **Main concern:** the legacy system that names the game was deferred. Fixed by the spec below.
- **Other concerns:** consequences in shared regions, parry over network lag, first playable build too big for the team.
- **Missing:** reasons to come back, death penalty, economy, monetization, first 5 minutes, numbers.

## 3. Competitor baseline
Full note: [[Competitor Baseline - Rogue Lineage and Deepwoken]]
- Rogue Lineage and Deepwoken prove Roblox players love lineage games, but both are hardcore, paid, hard to learn, pass down **power**, and hide their story.
- **Our gap:** friendly to newcomers, free to play, passes down **story and reputation**, with a clear main quest.

## 4. Legacy system
Full note: [[Legacy System Spec]]
- **Core idea:** your character's choices become your family's reputation.
- **New generation starts by retiring,** not by dying. Retire at the Shrine of Embers after a region boss.
- **The heir inherits:** family flags (NPCs remember), 50% reputation, revealed map, 1 heirloom, 1 bloodline trait. Level, inventory and skills reset.
- **Death:** respawn at the last shrine, lose gear durability, recoverable loot drop. No permadeath (maybe an opt-in hardcore mode later).
- **Multiplayer:** all legacy data is per player. Legacy-driven content only appears in private instances or as NPCs only that player sees.
- **Do now (milestone D):** save the knight choice in `bloodline.familyFlags`, not on the character.

## 5. Story
Full notes: [[Story Foundation]], [[Prologue Script]] and [[Act 1 Quest Outline]]
- **Premise:** every hundred years the Void wakes, a hero rises, and the hero falls and becomes a monster. That's the Cycle. Every region boss is a fallen hero from an earlier Cycle.
- **The Ember:** heroes carry an Ember that slowly corrupts. Past heroes fell because they held on too long. Retiring means passing your Ember on, which makes the legacy mechanic the lesson of the story.
- **Long-term goal:** the final boss needs three seals, each from a different generation of your family.
- **Prologue (first 5 minutes):** you play Ser Caelan, the last champion. He refuses to pass on his Ember and becomes the Fallen Knight, the first boss. His Ember escapes and finds your character a hundred years later.
- **First playable build needs:** the prologue and Act 1 only.
- **Act 1:** 6 main quests in Havenbrook. The wounded knight (Ser Bryn Hale) is dying the way Caelan did. Save him with your Ember, take his sword, or leave him; leaving him makes him the dungeon mini-boss. The Old Gate dungeon ends with the Fallen Knight. First playable cut is ~15 minutes.

## 6. Build plan (milestones from the GDD, with our changes)
| Milestone | Exit criteria | Changes from our planning |
|---|---|---|
| A. Combat sandbox | Attacks, dodge, parry, one enemy feel good | Test at 150–250 ms ping. Client-side prediction with server checks and timing tolerance. |
| B. Regional slice | Havenbrook + stable travel | Suggest Havenbrook + dungeon only; move the forest gateway later. Build the prologue zone inside Havenbrook. |
| C. Co-op dungeon | Party can enter, finish, claim rewards, return | No change |
| D. Legacy proof | One choice changes a later quest or NPC | Save the knight choice as a family flag. Add the Fallen Knight boss memory. |
| E. Alpha readiness | Performance, onboarding, data integrity | The prologue is the onboarding |

**After alpha:** retirement shrine and ceremony, heirlooms, bloodline traits, reputation, the heir prologue variant.

## 6b. Economy and monetization
Full note: [[Economy and Monetization]]
- **One currency (gold) plus region materials.** No premium currency; cosmetics priced directly in Robux.
- **Gold mostly from quests and dungeons,** little from killing enemies. Main sinks: upgrades, repairs, heirloom engraving, and retirement (90% of gold left behind).
- **No player trading at launch.**
- **Free to play, cosmetics only.** Skins, Ember colors, family crests that last across generations, emotes, private servers. Purchases belong to the account and are never lost on retirement.
- **Never sold:** power, boosts, revives, loot boxes, legacy perks.

## 7. How the team works
Full notes: [[Team and Roles]], [[Current Status]], [[Decisions Log]]
- **Repo:** https://github.com/JasonEyeglass/the-legacy-cycle (private). The Obsidian vault inside it is the team's shared memory; every member's Claude reads it.
- **Rules (in `CLAUDE.md`):** pull before work, push after. Every note has an `owner:`. Only edit your own notes; suggest changes to others under `## Proposals`. Log decisions. Never force-push.
- **Roblox Studio:** use Team Create with script drafts. Rojo later if scripts should live in Git.

## 8. Open questions for the team
**Team setup**
- [ ] Fill in the 5 names, roles and note owners in [[Team and Roles]].

**Legacy system**
- [ ] Retirement before the first boss, or only after?
- [ ] One character per player, or several heirs?
- [ ] Heir as child or apprentice?

**Story**
- [ ] Is the Void Sovereign the first hero?
- [ ] Does corruption affect gameplay or only visuals and dialogue?
- [ ] Voice acting or text only?
- [ ] Character creation before or after the prologue?
- [ ] Is the tone right for the Roblox audience?

**Economy and monetization** (drafted in [[Economy and Monetization]])
- [ ] Robux prices, gold inheritance %, and gray areas (inventory space, character slots) listed there.

**Still missing from the plan**
- [ ] Reasons to come back (daily or weekly activities).
- [ ] Numbers: level ranges, region length, boss health and party scaling.

**Research**
- [ ] Someone plays Rogue Lineage and Deepwoken for a few hours.
