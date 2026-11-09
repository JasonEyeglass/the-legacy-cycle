---
tags: [legacy-cycle, checkpoint, act-1]
created: 2026-11-09
status: in progress
---

# Act 1 Slice: the next checkpoint

Chosen 2026-11-09 (user: "Act 1 slice first"). This is the first playable build from the team's plan: a new player plays the opening of the game from start to finish in about 15–20 minutes. It covers milestones B (the Havenbrook slice), D (one choice changes a later quest) and most of C (the co-op dungeon). Back to [[The Legacy Cycle]].

**Sources:** the team's `Act 1 Quest Outline` (its "First playable cut" section), `Prologue Script` and `Story Foundation`, all in the repo under `THE LEGACY CYCLE/4 📐 Design/`.

## Done when

A brand-new player can:
- join, forge a hero, play the prologue, then play Act 1's cut, which is 1.1, 1.3, 1.5 and 1.6;
- make the wounded-knight choice and see it change Orla's line and the dungeon's mini-boss;
- clear the Old Gate alone or with up to 3 friends, then see the Fallen Knight's memory.

And when they rejoin, their choice, gear and progress are still there. The whole run has been played on desktop and in the phone emulator, with the console clean.

## Rule while working on it

Polish only goes in if it's on the slice's path. Anything else goes to "Later" below.

## Build order

Each step ends in something playable. The prologue is built last, even though it plays first: its burning old Havenbrook is the same place as the dungeon's buried old Havenbrook, and its Caelan becomes the Fallen Knight boss. Building the dungeon first means that art and that boss are made once.

| # | Step | What's in it | Reuses |
|---|---|---|---|
| 1 | **Story state and gear** | `storyFlags`, `questState`, `gold` and `inv` in the save (DATA_VERSION 4; old saves load empty). An Items registry with the starter sword, *Hale's Oath* and the tarnished *Hale's Oath*. An inventory service (add, remove, has, equip) whose equipped weapon's damage feeds combat. The floating menu's Inventory page shows real items. | PlayerData, CombatCore/Rules, FloatMenu's slot plates |
| 2 | **1.1 A Spark in Havenbrook** | After absorbing the Ember, the Keeper sends you to the guild. Orla Venn doubts you until your Ember flares at the old lantern. Reward: starter sword and 100 gold. | Shrine of Embers and the absorb cinematic (this is the prologue's Scene 6 spark), Townsfolk Orla, DialogueClient, QuestService |
| 3 | **1.3 The Wounded Knight** | Bryn Hale below the watchtower outside the gate (Havenbrook has one), with Void veins. Only players whose state matches see him. Three-way choice saved as `knight_fate` (saved / took / left): saving drains the Ember, taking gives *Hale's Oath*. Orla's next line changes with it. | The Havenbrook Knight rig and armor (as Bryn), dialogue choices |
| 4 | **Enemies** | Hollowed soldier (a Hollow Squire reskin in old Havenbrook armor), void hound (fast, low health, packs), void brute (the mini-boss when Bryn is saved or taken). The archer waits for later. | EnemyAI, the Squire |
| 5 | **1.5 The Old Gate** | A dungeon instance per party (1–4 players) under the town, in the same place, so there's no teleport. Groups are formed by standing in the gate circle when the leader starts. Order: encounters, then the mini-boss (Hollowed Bryn if the leader left him, otherwise a void brute), then the Fallen Knight at the old gate in 3 phases, with the Bryn assist if he was saved. Checkpoints sit before the mini-boss and before the boss. | Combat, EnemyAI, the knight armor (blackened for Caelan) |
| 6 | **1.6 What He Would Not Let Go** | The Fallen Knight's memory as a cutscene, then the Keeper's last lines. Rewards: 150 gold and the title "Ember-Bearer". Sets `act1_complete`. | ShrineFx.Cinematic, the roblox-cutscene kit |
| 7 | **Prologue** | Cold open, burning old Havenbrook, the gate fight (attack, heavy, dodge), the Brute (parry), the Void (Ember Strike), the fall, then present day. Built as a hidden zone in this place. Its enemies run on the client and give no rewards. Sets `prologue_seen`. | The dungeon's old-Havenbrook art, the Fallen Knight model, Hollow Squires |
| 8 | **Slice pass** | Play the whole run end to end: desktop, phone emulator, and a 200 ms ping test (the parry window). Fix what breaks. | |

## Assets this needs

Some can come from what exists, some are new:
- **The Keeper:** new.
- **Orla:** an existing townsperson.
- **Bryn:** the armored knight.
- **Fallen Caelan:** the armor blackened, with a purple glow.
- **Void hound and void brute:** new models.
- **Old Havenbrook (buried and burning):** a damaged copy of the town blockout.
- **Ember lantern and gate circle:** new.
- **Void tendrils and Ember Strike VFX:** new (PhaseVFX first).

## Open questions (my default in brackets)

- **Captain Aldric and The Missing Caravan** aren't in the team's Act 1. [Aldric stays as the gate warden who points players north; The Missing Caravan becomes side content outside the slice.]
- **Ember Strike**, the prologue's skill and the game's "one magic ability", doesn't exist yet. [Build it in step 7, and let the hero use it after absorbing the Ember.]
- **Gold** has no use in the slice, because Tobin's upgrade (1.4) is cut. [Save it and show it anyway, so the rewards are real.]
- **1.1's "starter sword" reward:** since step 1, every hero already carries the Wanderer's Katana. [1.1 gives the 100 gold only.] Settled 2026-11-09: 100 gold and 60 XP.
- **The Missing Caravan vs 1.1:** only one quest runs at a time, so a player who took Aldric's quest first gets 1.1 once it ends. [Make the Caravan a side quest that can run beside the story, when the quest log allows more than one.] Eased 2026-11-09: abandoning the caravan lets 1.1 start.

## Later (not in the slice)

1.2 and 1.4, the brazier puzzle, the archer, the side quests and the bounty board. Mining and forging (roadmap Phase 1's crafting loop). The Ethereal Forest. A model of its own for each weapon (Hale's Oath looks like the katana for now). Armor items for the other equipment slots. A guild hall building. The lantern's own "Done" line once 1.1 is over (it reads as cold again).

## Progress

- [x] 1 Story state and gear (2026-11-09, waiting on the user's playtest)
- [x] 2 A Spark in Havenbrook (2026-11-09, waiting on the user's playtest)
- [x] 3 The Wounded Knight (2026-11-09, waiting on the user's playtest)
- [x] 4 Enemies (2026-11-09, stand-in models for the hound and brute; waiting on the user's playtest)
- [ ] 5 The Old Gate
- [ ] 6 What He Would Not Let Go
- [ ] 7 Prologue
- [ ] 8 Slice pass
