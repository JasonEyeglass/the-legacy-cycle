---
created: 2026-11-03
owner: JasonEyeglass
tags: [story, quests, act-1, draft]
status: draft
---

# Act 1 Quest Outline: Havenbrook (draft v0.1)

Act 1 of [[Home|The Legacy Cycle]]. Picks up straight after the [[Prologue Script]] and follows the arc in [[Story Foundation]]. Uses the wounded knight choice from the GDD, saved the way the [[Legacy System Spec]] says, and gold amounts from [[Economy and Monetization]].

## The act in one line
The player's Ember brings them to the guild, they meet a knight dying the way Caelan did, and they go beneath Havenbrook to face what Caelan became.

## At a glance
| | |
|---|---|
| Region | Havenbrook village, its fields, and the ruins of old Havenbrook beneath it |
| Main quests | 6 |
| Side quests | 3 |
| Big choice | The wounded knight (save / take his weapon / leave him) |
| Dungeon | **The Old Gate**: private, 1–4 players, ends with the Fallen Knight |
| Length target | Full act ~60 min. First playable cut ~15 min (see below). Not measured. |
| Ends with | The Fallen Knight's memory and the road to the Ethereal Forest |

## New characters
| Character | Role |
|---|---|
| **Guildmaster Orla Venn** | Runs the adventurers' guild. Practical, skeptical of "Ember nonsense" until she sees it. Gives main quests. |
| **Ser Bryn Hale** | The wounded knight. A young guild knight, proud like Caelan. The choice is about him. |
| **Tobin the smith** | Blacksmith. Teaches upgrades. Comic relief. |
| **The Keeper** | From the prologue. Stays at the Shrine of Embers and explains what the player has found. |

## Enemies (the GDD's three standard archetypes)
| Enemy | Role in combat |
|---|---|
| **Hollowed soldier** | Melee, slow, readable (from the prologue) |
| **Hollowed archer** | Ranged, forces the player to close distance or dodge |
| **Void hound** | Fast, low health, attacks in packs, teaches crowd control |

---

## Main quests

### 1.1 A Spark in Havenbrook
**Goal:** get to the guild hall and join the guild.
- The player walks through Havenbrook for the first time. It's the same town from the prologue, rebuilt. Players may recognize the square and the north gate.
- Orla doesn't believe the Keeper sent them, until the player's Ember flares when they touch the guild's old Ember lantern.
- **Teaches:** map, quest tracker, talking to NPCs.
- **Reward:** starter sword, 100 gold.

> Orla: "The Keeper hasn't sent anyone in my lifetime. Or my grandmother's."

### 1.2 Trouble on the North Road
**Goal:** find out what's attacking farms outside the north gate.
- First open-world fighting: void hounds in the fields, then hollowed soldiers near a collapsed watchtower.
- Hollowed soldiers wear **old Havenbrook armor**, a hundred years old. Something is digging up the past.
- **Teaches:** fighting in the open world, picking up materials.
- **Reward:** 120 gold, Havenbrook iron.

### 1.3 The Wounded Knight (the choice)
**Goal:** find Ser Bryn Hale, who went to scout the watchtower and didn't come back.
- The player finds Bryn below the watchtower. He's badly wounded and the Void is spreading through the wound: purple veins, the same glow Caelan had.
- Bryn won't go back: *"I can hold them here. Just give me a minute."* The player should hear Caelan in this.
- **The choice** (see the table below).

### 1.4 What Lies Beneath
**Goal:** find out where the hollowed are coming from.
- The trail leads to a crack in the ground near the north gate. **Old Havenbrook is buried underneath the new one.**
- The Keeper explains: the old north gate is where Caelan fell. Something down there is still "holding the line", and it's waking up.
- Tobin the smith says the player won't survive down there with a starter sword: **first upgrade** (200 gold + 10 Havenbrook iron).
- **Teaches:** upgrading gear, forming a party, the dungeon entrance.
- **Dialogue changes** depending on the knight choice (see table).

### 1.5 The Old Gate (dungeon)
**Goal:** go down into old Havenbrook and stop what's behind the gate.
Private dungeon, 1–4 players, 15–25 minutes (GDD target). Flow follows GDD section 4:

| Part | What happens |
|---|---|
| Exploration | The burned streets from the prologue, now buried and dark. Players walk past the places they fought as Caelan. |
| Encounters | Mixed groups of all three enemy types |
| Puzzle | Light three Ember braziers to open the way. In co-op, players split up to light them at the same time. Solo: braziers stay lit longer. |
| Mini-boss | **Depends on the knight choice** (see table) |
| Final boss | **The Fallen Knight** at the old north gate |
| Rewards | Gear, materials, 120 gold each (+100 first clear each week), lore |

**Party rule** (from the spec): the dungeon uses the **party leader's** story state for the mini-boss. Everyone keeps their own choices and rewards.

### 1.6 What He Would Not Let Go
**Goal:** return to the Keeper.
- After the boss, the player sees the **Fallen Knight's memory**: Scene 4 of the prologue, replayed from the Keeper's view. Now the player knows they played him.
- The Keeper: *"He held the line for a hundred years. Nobody told him he could stop."*
- She reveals the other fallen heroes are waking too. The nearest is in the Ethereal Forest.
- **Reward:** 150 gold, title "Ember-Bearer", the road to the forest opens (Act 2).
- Seed for the legacy system: she shows the player the empty stone ring in the shrine. *"One day you'll stand here and hand your Ember on. Not today."*

---

## The wounded knight choice
Saved as `bloodline.familyFlags.knight_fate` = `saved` / `took` / `left`. This is the milestone D proof: one choice that changes a later quest and NPC responses.

| | **Save him** | **Take his weapon** | **Leave him** |
|---|---|---|---|
| How | Use your Ember to burn the Void out of his wound. Your Ember bar drains to empty for the rest of the quest. | Take his sword, *Hale's Oath*. He's too weak to stop you. | Walk away. |
| What it means | **Giving up some of your power to save someone.** The thing the Keeper couldn't do for Caelan. | Power now, at someone else's cost | Not getting involved |
| Right away | Bryn survives and goes back to the guild | You get *Hale's Oath*, stronger than the starter sword | Nothing |
| In 1.4 | Orla is grateful. Bryn, bandaged, gives you a tip about the gate. | Orla is cold: guild reputation −10. *"That sword belongs to a Hale."* | Orla: *"Bryn never came back. Did you see him?"* |
| In the dungeon (mini-boss) | A void brute (standard) | A void brute (standard) | **Hollowed Bryn**: Bryn, taken by the Void. Uses guild knight moves. Drops a tarnished *Hale's Oath*. |
| In the boss fight | In the last phase, Bryn appears at the gate and holds it for 10 seconds, a scripted assist that gives the party a free hit | Nothing | Nothing |
| Later (Act 2+) | Bryn becomes a weapon trainer | Hale family members distrust you | The Keeper mentions Bryn |
| **Next generation** | The Hale family trains your heir: *"Your family saved ours."* | The Hales remember: *"Your family took our sword."* | Hollowed Bryn becomes a wandering threat in Havenbrook for the heir |

No choice is "the right one" in rewards. *Take* gets the best weapon early. *Save* gets the ally and the best long-term story. *Leave* gets the hardest and most memorable fight. That keeps it a real choice, not a test.

---

## Boss: the Fallen Knight
Brief for combat design. Everything he does should feel like Caelan from the prologue, but corrupted.

| Phase | Health | Moves |
|---|---|---|
| 1. The champion | 100–50% | **Caelan's own combo from the prologue,** heavy guard-breaking swing, a charge. Players who remember the prologue can read him. |
| 2. The corrupted Ember | 50–20% | **Corrupted Ember Strike:** the player's own skill, but purple and bigger. Void tendrils from the floor. Long glowing wind-up on the big attack: the parry check. |
| 3. Holding the line | 20–0% | He plants his sword at the gate and refuses to move: *"I... hold... the line..."* Fast, desperate attacks around the gate. If Bryn was saved, he appears here. |

- **Party scaling** (placeholder): health ×1 solo, ×1.6 for 2, ×2.2 for 3, ×2.8 for 4.
- **Network lag:** the big parry attack in phase 2 needs a generous timing window (see the lag concern in [[GDD v0.2 Review]]).
- **Model reuse:** the Fallen Caelan model from the prologue.

---

## Side quests
| Quest | Giver | What happens | Reward | Why it's there |
|---|---|---|---|---|
| **Iron for Tobin** | Tobin | Gather Havenbrook iron in the fields | Gold, a free repair | Teaches materials and the economy |
| **Ashes of Old Havenbrook** | The Keeper | Find 5 hidden Ember shards around the region, each showing a short scene from the old town | Lore, a cosmetic crest border | Exploration and lore, for players who want to dig |
| **The Bounty Board** | Guild hall | Repeatable daily bounties: clear a camp, kill a number of void hounds | Gold, materials | A daily reason to come back (a gap from the GDD review) |

---

## First playable cut (~15 minutes)
The GDD's minimum playable build is a 10–15 minute adventure. Cut Act 1 to:
1. **1.1** short: talk to Orla (2 min).
2. **1.3** the wounded knight, reached straight from the gate (4 min).
3. **1.5** the dungeon cut down to encounters, then the mini-boss, then the Fallen Knight (8 min).
4. **1.6** the memory and the Keeper's last line (1 min).

Cut for later: 1.2, 1.4 (upgrade can move to a guild NPC), the puzzle, and all side quests. This still proves milestone C (co-op dungeon) and milestone D (one choice changes the mini-boss).

## Implementation notes
- **Per-player visibility:** Bryn in the watchtower and in the guild exists only for players whose state matches. Use client-side visibility or a per-player NPC. Never one shared Bryn in the region (spec rule).
- **Flags:** `knight_fate` on `bloodline.familyFlags`; quest progress on `character.questState`; Act 1 complete as `bloodline.familyFlags.act1_complete`.
- **Disconnect in the dungeon:** restoring dungeon progress after a disconnect is a GDD risk. Simplest first version: checkpoint before the mini-boss and before the Fallen Knight.

## Open questions
- [ ] Should saving Bryn cost more than the Ember (e.g. failing a timer, losing the first upgrade)?
- [ ] Guild reputation −10 for taking the sword: too harsh?
- [ ] Is a daily bounty board enough of a "come back" reason for Act 1?
- [ ] Names: Orla Venn, Bryn Hale, Tobin, Hale's Oath. Keep or change?
