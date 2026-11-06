---
created: 2026-11-03
owner: JasonEyeglass
tags: [hub, plan, overview]
---

# Project Plan Overview

Everything planned so far for [[Home|The Legacy Cycle]] on one page. Start here if you are new to the project. Each section links to the full note. Status of all of this: **proposals**, until the team logs them in [[Decisions|Decisions Log]].

## 1. The game
Roblox open-world dark-fantasy action RPG. Tagline: *"Your Choices. Their Tomorrow."* Source: `The_Legacy_Cycle_Updated_Open_World_GDD_v0.2.docx` in the repo root.

- Six regions, each a separate Roblox place: Havenbrook, Ethereal Forest, Frostveil Mountains, Ashen Wastes, Shadowlands, Forgotten Realm.
- Private dungeons for 1–4 players.
- Action combat: attacks, dodge, parry, weapon skills.
- Signature mechanic: your choices change the world and its people. One character per player (heirs cut 2026-11-05, see section 4).

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

## 4. Choices and legacy (changed 2026-11-05)
Full notes: [[Merge Proposal - Legacy Cycle and Action MMO]]; the old [[Legacy System Spec]] is kept for history.
- **No heirs.** Retirement, heirs, heirlooms, bloodline traits and reputation carry-over are cut: too complicated for younger players. Each player keeps one character.
- **"Legacy" now means the heroes before you.** You play them in short flashbacks (boss and Echo memories), like Caelan in the prologue.
- **Choices still matter:** story flags change NPCs, quests and the mini-boss.
- **Death:** respawn at the last shrine, lose gear durability, recoverable loot drop. No permadeath.
- **Multiplayer:** all story flags are per player. Flag-driven content only appears in private instances or as NPCs only that player sees.
- **Do now (milestone D):** save the knight choice in `character.storyFlags`.

## 5. Story
Full notes: [[Story Foundation]], [[Prologue Script]] and [[Act 1 Quest Outline]]
- **Premise:** every hundred years the Void wakes, a hero rises, and the hero falls and becomes a monster. That's the Cycle. Every region boss is a fallen hero from an earlier Cycle.
- **The Ember:** heroes carry an Ember that slowly corrupts. Past heroes fell because they held on too long. The lesson lands in the final choice: let go, take control, or join the Void.
- **Long-term goal:** the final boss needs three seals, earned from region bosses.
- **Prologue (first 5 minutes):** you play Ser Caelan, the last champion. He refuses to pass on his Ember and becomes the Fallen Knight, the first boss. His Ember escapes and finds your character a hundred years later.
- **First playable build needs:** the prologue and Act 1 only.
- **Act 1:** 6 main quests in Havenbrook. The wounded knight (Ser Bryn Hale) is dying the way Caelan did. Save him with your Ember, take his sword, or leave him; leaving him makes him the dungeon mini-boss. The Old Gate dungeon ends with the Fallen Knight. First playable cut is ~15 minutes.

## 6. Build plan (milestones from the GDD, with our changes)
| Milestone | Exit criteria | Changes from our planning |
|---|---|---|
| A. Combat sandbox | Attacks, dodge, parry, one enemy feel good | Test at 150–250 ms ping. Client-side prediction with server checks and timing tolerance. |
| B. Regional slice | Havenbrook + stable travel | Suggest Havenbrook + dungeon only; move the forest gateway later. Build the prologue zone inside Havenbrook. |
| C. Co-op dungeon | Party can enter, finish, claim rewards, return | No change |
| D. Choice proof | One choice changes a later quest or NPC | Save the knight choice as a story flag. Add the Fallen Knight boss memory. |
| E. Alpha readiness | Performance, onboarding, data integrity | The prologue is the onboarding |

**After alpha:** reputation, more boss memories, the Memory Journal.

## 6b. Economy and monetization
Full note: [[Economy and Monetization]]
- **One currency (gold) plus region materials.** No premium currency; cosmetics priced directly in Robux.
- **Gold mostly from quests and dungeons,** little from killing enemies. Main sinks: upgrades, repairs, crafting, and Echo awakenings (if adopted).
- **No player trading at launch.**
- **Free to play, cosmetics only.** Skins, Ember colors, crests, emotes, private servers. Purchases belong to the account and are never lost.
- **Never sold:** power, boosts, revives, loot boxes.

## 7. How the team works
Full notes: [[Team and Roles]], [[Current Status]], [[Decisions|Decisions Log]]
- **Repo:** https://github.com/JasonEyeglass/the-legacy-cycle (private). The Obsidian vault inside it is the team's shared memory; every member's Claude reads it.
- **Rules (in `CLAUDE.md`):** pull before work, push after. Every note has an `owner:`. Only edit your own notes; suggest changes to others under `## Proposals`. Log decisions. Never force-push.
- **Roblox Studio:** use Team Create with script drafts. Rojo later if scripts should live in Git.

## 8. Open questions for the team
**Team setup**
- [ ] Fill in the 5 names, roles and note owners in [[Team and Roles]].

**Legacy** (heirs cut 2026-11-05)
- [ ] Keep the tagline "Your Choices. Their Tomorrow."?

**Story**
- [ ] Is the Void Sovereign the first hero?
- [ ] Does corruption affect gameplay or only visuals and dialogue?
- [ ] Voice acting or text only?
- [x] Character creation before or after the prologue? **Before** (on first join), 2026-11-06.
- [ ] Is the tone right for the Roblox audience?

**Economy and monetization** (drafted in [[Economy and Monetization]])
- [ ] Robux prices and gray areas (inventory space, character slots) listed there.

**Still missing from the plan**
- [ ] Reasons to come back (daily or weekly activities).
- [ ] Numbers: region length, boss health and party scaling. (Level ranges and the XP curve are set in Fred's `Decisions.md`.)

**Research**
- [ ] Someone plays Rogue Lineage and Deepwoken for a few hours.
