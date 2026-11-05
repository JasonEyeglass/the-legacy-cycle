---
created: 2026-11-03
owner: unassigned
tags: [research, competitors, legacy, design]
---

# Competitor Baseline: Rogue Lineage and Deepwoken

Baseline for [[Home|The Legacy Cycle]]. It checks the comparison table in [[Legacy System Spec]] and follows up concern #2 in [[GDD v0.2 Review]].

> Based on web research (wikis, guides, news) on 2026-11-03, not hands-on play. Numbers change with updates. Someone on the team should play each game for a few hours before we lock the design.

## At a glance
| | Rogue Lineage | Deepwoken |
|---|---|---|
| Released | March 2019 | December 2021 |
| Access | 200 Robux entry fee | 400 Robux entry fee |
| Reach | 378M+ visits | ~1.52B visits, won Best New Experience at the 2022 Roblox Innovation Awards |
| Lives | 3 per character, no normal way to get them back | 3 death stages: first death leaves a lasting HP penalty until the next level-up; second sends you to the Depths; third is permanent death |
| How a character ends | Lose the last life, which deletes the character | Die in the Depths, or wipe on purpose from the menu |
| Escape route | None | Escape the Depths (ring two bells, open the gate) to be restored, or pay Robux once at a statue |
| What carries over | **Edicts** (inherited abilities earned through quests, unlocked at 1, 5 and 10 wipes); **one banked artifact** (Banker NPC); some special artifacts such as an Heirloom | **Echoes**, a currency earned from the old character's achievements and spent on permanent upgrades; **2 to 6 items** through the Hippocampal Pool (legendary or enchanted items cost 40 Echoes) |
| Story delivery | Minimal. Mostly alignment quests and lore you find yourself | Environment, item descriptions, factions. "Explains almost nothing." |
| PvP | Core of the game. Orderly vs Chaotic alignment, jailing, player kills | Present. Griefing of new characters is a known problem |
| Same studio | Yes. Deepwoken's setting builds on Rogue Lineage's lore | |

## What they prove
- **Roblox players will pay for and stick with lineage and permadeath games.** Both charge an entry fee and still have huge visit counts.
- **The metagame keeps players going.** Losing a character hurts, but Echoes and Edicts make each run count for the next one.
- **Consequence creates stories.** Players remember the character they lost.

## Where they leave room for us
| Their weakness | Our opportunity |
|---|---|
| Hardcore and hostile. Losing a character can cost about 30 hours of progress | Retiring is a choice and death is not permanent, so it suits casual and mobile players |
| What carries over is **power** (Echoes, Edicts, items) | What carries over is **story**: NPCs and quests react to your family's choices |
| Story is hidden and players piece it together | A clear main story with quests, plus hidden lore for people who want to dig |
| PvP and griefing dominate the early game | PvE and co-op first, with no open-world PvP at launch |
| Paid entry | Free to play with cosmetic monetization (to be decided) |

## Changes to our spec
- The table in [[Legacy System Spec]] holds up. One correction: Deepwoken also lets players **wipe on purpose**, so "we let you retire by choice" is not new by itself. What sets us apart is **what** carries over (story and reputation) and **why** you retire (see the Embers in [[Story Foundation]]).
- Deepwoken's 40-Echo cost for passing down legendary items is a useful model. Passing on a strong heirloom should cost the player something.
- Rogue Lineage unlocks Edicts at 1, 5 and 10 wipes. Our bloodline traits could also unlock at generation milestones.
- Both games are hard to learn and new players get griefed. Our first 5 minutes need to be welcoming, so they're part of the story (see [[Story Foundation]]).

## Sources
- [Deepwoken: Inside Roblox's Most Brutal Hardcore RPG (RoWatcher)](https://rowatcher.com/news/deepwoken-inside-roblox-s-most-brutal-hardcore-rpg)
- [What happens when you die in Deepwoken (Destructoid)](https://www.destructoid.com/what-happens-when-you-die-in-deepwoken/)
- [Rogue Lineage Guide (roguelineage.wiki)](https://www.roguelineage.wiki/guide/rogue-lineage-guide)
- [Rogue Lineage Edicts (roguelineage.wiki)](https://www.roguelineage.wiki/systems/rogue-lineage-edicts)
- [Rogue Lineage Lives](https://rogue-lineage.fandom.com/wiki/Lives), [Banker](https://rogue-lineage.fandom.com/wiki/Banker), [Artifacts](https://rogue-lineage.fandom.com/wiki/Artifacts) (Fandom wiki)
- [Rogue Lineage on RoTrends](https://www.rotrends.com/game/1087859240/Rogue-Lineage)
