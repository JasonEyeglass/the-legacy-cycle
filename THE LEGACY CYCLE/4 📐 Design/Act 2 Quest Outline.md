---
created: 2026-11-06
owner: JasonEyeglass
tags: [story, quests, act-2, draft]
status: draft
---

# Act 2 Quest Outline: Ethereal Forest and Frostveil (draft v0.1)

Act 2 of [[Home|The Legacy Cycle]]. Picks up after [[Act 1 Quest Outline]] and follows the arc in [[Story Foundation]]: *follow the trail of the earlier heroes and learn that heroes fall when they hold on.* Built on the adopted [[Merge Proposal - Legacy Cycle and Action MMO]] (Echoes, one region per chapter). Level ranges come from Fred's build notes (`Fred/The Legacy Cycle/Decisions.md`). Gold and materials follow [[Economy and Monetization]].

Act 2 is **two chapters**, matching the release plan: Chapter 2 (Ethereal Forest) and Chapter 3 (Frostveil, the Season 1 finale).

## The act in one line
The player follows the Keeper's trail to two more fallen heroes, a druid who gave herself to the forest and a king who froze his own people, and learns from their memories that the Keeper was there when each of them chose to hold on.

## At a glance
| | Chapter 2: Ethereal Forest | Chapter 3: Frostveil Mountains |
|---|---|---|
| Levels | 15–30 | 30–45 |
| Main quests | 5 | 6 |
| Side quests | 3 | 3 |
| Big choice | The Guardian's heartwood (return / keep) | The frozen city (thaw / keep it frozen) |
| Dungeon | **The Rootdeep**: private, 1–4 players, ends with the Hollow Guardian | **The Glacier Throne**: private, 1–4 players, ends with the Frostbound King |
| Echoes | The Hollow Guardian (control/support) + 1 hidden | The Frostbound King (tank) |
| Seal | — | **First seal** |
| Material | Forest moonwood | Frostveil ice-crystal |
| Length target | ~60 min per chapter. Not measured. | |
| Ends with | Her memory and the mountain road | The King's memory, the first seal, and the Season 1 reveal |

## New characters
| Character | Role |
|---|---|
| **Warden Ilsa Thorne** | Leads the last forest rangers. Calm, tired, has buried too many people to the rot. Gives Chapter 2 main quests. |
| **Pip** | A small moss sprite who follows the player through the forest. Comic relief and the hint system for the Rootdeep puzzle. |
| **Maren of the Grove** | The druid who became the **Hollow Guardian**. Seen only in her memory and as the boss. |
| **Wren Hale** | Bryn's older sister, a scout in Frostveil. How she treats you depends on the knight choice. |
| **Old Gunnar** | Keeper of the last mountain inn. Remembers songs about the king that no one else can finish (the "world forgets" theme). Gives Chapter 3 main quests. |
| **King Halvard** | The **Frostbound King**. Seen in his memory and as the boss. |
| **The Keeper** | Still at the Shrine of Embers, reachable by fast travel. Her story turns in Chapter 3. |

## Enemies
| Enemy | Region | Role in combat |
|---|---|---|
| **Rotwood stalker** | Forest | Melee ambusher that hides as a tree. Teaches watching the environment. |
| **Spore-caster** | Forest | Ranged, leaves slowing clouds on the ground. Teaches positioning. |
| **Thornling swarm** | Forest | Many weak enemies. Tests the area moves of your Echo. |
| **Frost-hollowed guard** | Frostveil | The King's frozen soldiers. Shield up from the front; teaches guard breaks and flanking. |
| **Ice wraith** | Frostveil | Ranged caster that blinks away when you close in. Teaches dash timing. |
| **Avalanche brute** | Frostveil | Slow heavy with big wind-ups. A parry check, tuned for 150–250 ms ping. |

All of them break into ash and embers on death (Mild rating, from the merge).

---

## Chapter 2: Ethereal Forest (levels 15–30)

### 2.1 The Road Into Green
**Goal:** reach the rangers' camp at the forest edge.
- The forest looks beautiful from far away. Up close, half of it is grey and hollow.
- Ilsa doesn't trust Ember-bearers: *"The last one who came through here promised to save us. Then she became the forest."*
- **Knight choice payoff:** if Bryn was **saved**, he's at the camp as a weapon trainer (Act 1 promise). He offers a free weapon-style lesson.
- **Teaches:** fast travel back to Havenbrook, the new material (forest moonwood).
- **Reward:** 200 gold, forest moonwood.

### 2.2 Spores and Silence
**Goal:** clear the rot from three ranger outposts.
- Open-world fighting with all three forest enemies.
- At each outpost, a ranger has forgotten their own partner's name. The rot takes memories first. Only the player's Ember can bring a name back (a short interaction at a glowing tree).
- **Teaches:** Echo swapping at a Shrine of Embers, choosing an Echo for the enemy type.
- **Reward:** 220 gold, a forest Echo skin (cosmetic, looks only).

### 2.3 The Druid's Path
**Goal:** follow Maren's old waymarkers to find out what she did.
- Pip appears and joins the player. He was Maren's companion and is the only one who still remembers her.
- The waymarkers show that Maren bound herself to the heart of the forest to stop the rot. It worked, for a while. Then she refused to let go, even when it began to hollow her out.
- The Keeper (by message at the next shrine): *"I knew her. She was kind. Kindness is not always enough."*
- **Reward:** 250 gold.

### 2.4 The Rootdeep (dungeon)
**Goal:** go under the oldest tree and reach the forest's heart.
Private dungeon, 1–4 players, 15–25 minutes.

| Part | What happens |
|---|---|
| Exploration | Huge glowing roots, a grey-and-gold cave, Maren's old camp |
| Encounters | Mixed forest groups; thornling swarms in tight tunnels |
| Puzzle | Grow root bridges by carrying light from Ember flowers. Pip points at the next flower if players get stuck. In co-op, one player holds a bridge open while others cross. |
| Mini-boss | **The Rot Mother**, a giant spore-caster that fills the room with clouds |
| Final boss | **The Hollow Guardian** at the forest's heart |
| Rewards | Gear, moonwood, 250 gold each (+200 first clear each week), lore |

**Party rule:** like Act 1, the dungeon uses the party leader's story state. Everyone keeps their own choices and rewards.

### 2.5 What She Would Not Let Go
**Goal:** return the Guardian's heartwood to Ilsa.
- After the boss, the player sees **Maren's memory** (playable, ~45 s): you are Maren, binding herself to the tree to save a dying ranger. The Keeper is there and says: *"Hold on. Only you can keep them safe."*
- That line is the first hint of the Season 1 reveal. Most players won't catch it yet.
- **The heartwood choice** (see below).
- **Reward:** **the Hollow Guardian's Echo** (guaranteed), 300 gold, the mountain road opens.

### The heartwood choice
Saved as `character.storyFlags.grove_fate` = `returned` / `kept`.

| | **Return it to the forest** | **Keep it** |
|---|---|---|
| How | Plant it in the forest's heart. | Take it with you. |
| What it means | Letting go. What Maren couldn't do. | Holding on to power, the way the heroes always have |
| Right away | The grey spreads back slowly. Rangers remember lost names. Pip stays in the forest, happy. | A rare upgrade material for one weapon. Pip stays, but is sad and won't talk to you. |
| Later | Ilsa joins the guild in Havenbrook. A hidden grove opens with a side quest. | The heartwood whispers. Corruption rises a little (visuals and dialogue only). |

As in Act 1, neither option wins on rewards. *Keep* is stronger now; *Return* gives the better story and a side quest.

### Boss: the Hollow Guardian
| Phase | Health | Moves |
|---|---|---|
| 1. The protector | 100–50% | Root walls that split the party, healing pulses from the trees around the arena |
| 2. The hollowing | 50–20% | Her bark cracks to show the Void inside. Grey thorn rain, she summons rotwood stalkers. Destroy the tree "anchors" to stop her healing. |
| 3. Holding on | 20–0% | She wraps herself around the heart tree and won't let go. Fast thorn sweeps in a shrinking ring. |

- **Party scaling** (placeholder): same as Act 1, health ×1 / ×1.6 / ×2.2 / ×2.8.
- **Phone rule:** the anchors must be big and clear to tap-target with soft lock-on.

---

## Chapter 3: Frostveil Mountains (levels 30–45), Season 1 finale

### 3.1 The Pass
**Goal:** cross the snowy pass to the last inn.
- Wind, deep snow and avalanche brutes. The mountain road from the forest opens straight into the storm.
- **Knight choice payoff:** Wren Hale, Bryn's sister, scouts the pass.
  - **Saved:** she guides you through: *"Bryn wrote me about you."*
  - **Took:** she refuses to help until you return or show her *Hale's Oath* (side quest 3.S2).
  - **Left:** she asks if you saw her brother. The player can tell the truth or lie (flavor only, sets a small flag).
- **Reward:** 350 gold, ice-crystal.

### 3.2 Songs Without Endings
**Goal:** find out who the Frostbound King was.
- At the inn, Old Gunnar sings songs about a king who "held back the winter", but nobody, not even him, can remember the last verse or the king's name.
- The player finds pieces of the song across the region (open-world exploration). Each one restores a line.
- **Teaches:** the Memory Journal as a quest tool. Restored songs appear in it.
- **Reward:** 380 gold, a cosmetic banner.

### 3.3 The Frozen City
**Goal:** reach Halvard's capital, frozen inside a glacier for 200 years.
- The city is full of people frozen mid-step. The Void is trapped in the ice with them.
- The player learns what King Halvard did: when the Void came, he froze his own kingdom to stop it spreading. He saved the world, and lost his people.
- **Teaches:** the region's hazard: cold builds up away from fires and braziers; standing near an Ember brazier warms you.
- **Reward:** 400 gold.

### 3.4 The Hale Shield
**Goal:** help Wren hold the city gate while the player finds a way in.
- A defense fight: waves of frost-hollowed guards. Wren fights alongside you (in every knight-choice version; her lines change).
- If the player **left** Bryn and later took his Echo in Act 1, Wren recognizes Bryn's moves when you use them. A short, quiet scene. No penalty, just weight.
- **Reward:** 400 gold, ice-crystal.

### 3.5 The Glacier Throne (dungeon)
**Goal:** reach the king in the heart of the glacier.
Private dungeon, 1–4 players, 20–25 minutes. The Season 1 finale, so it is the biggest dungeon so far.

| Part | What happens |
|---|---|
| Exploration | Frozen streets, a throne room inside the ice, the king's guard frozen in rows |
| Encounters | Frost-hollowed guards and ice wraiths |
| Puzzle | Carry warmth: move between braziers, melting ice walls on the way. In co-op, players pass a lantern between them. |
| Mini-boss | **The First Shield**, the captain of the King's guard: a frost-hollowed guard with a huge shield |
| Final boss | **The Frostbound King** |
| Rewards | Gear, ice-crystal, 300 gold each (+250 first clear each week), lore |

### 3.6 What He Would Not Let Go
**Goal:** take the first seal and return to the Keeper.
- **The King's memory** (playable, ~60 s): you are Halvard as the Void reaches the city. The Keeper stands beside him, *exactly as she looks today, 200 years ago*, and tells him: *"Hold on. Freeze it. Only you can keep them safe."*
- **The frozen city choice** (see below).
- **Reward:** **the Frostbound King's Echo** (guaranteed), **the first seal**, 500 gold, title "Seal-Bearer".

### The frozen city choice
Saved as `character.storyFlags.frostveil_fate` = `thawed` / `frozen`.

| | **Thaw the city** | **Keep it frozen** |
|---|---|---|
| How | Use the seal's power to melt the glacier. | Leave the ice as Halvard left it. |
| What it means | Letting go of the safe answer. The people get their lives back, and you take the risk. | Honoring the King's sacrifice. Safe, but nobody wakes up. |
| Right away | The people wake, confused. Some Void escapes: a world event (Void storms in Frostveil) appears for you. | The city stays silent. Gunnar finally sings the last verse, with the king's name. |
| Later | Thawed citizens open a market and quests in Frostveil. More Void enemies in Act 3. | A quiet memorial. The Ashen Wastes start a little easier. |

The thawed city must be **per-player visible** (spec rule): the shared region looks the same for everyone, and the woken citizens are per-player NPCs or a private instance.

### Boss: the Frostbound King
| Phase | Health | Moves |
|---|---|---|
| 1. The king | 100–60% | Greatsword combos, ice pillars that block line of sight |
| 2. The winter | 60–25% | The arena freezes over: slippery floor, cold build-up. Players must stand by braziers between attacks. He calls frozen guards. |
| 3. Holding the ice | 25–0% | He kneels and tries to freeze everything, including himself. A long glowing wind-up on the big attack: the parry check. Party members can warm each other by standing close. |

- **Party scaling** (placeholder): same as Act 1.
- **Network lag:** the big parry attack needs the same generous window as the Fallen Knight's.

### The Season 1 reveal
After the King's memory, the player goes back to the Shrine of Embers.
- The player now has two memories where the Keeper told a hero to *hold on*.
- The Keeper doesn't deny it: *"I told them all to hold on. Every one of them. And every one of them fell."*
- *"So with you, I'm trying something else."*
- She walks away from the shrine for the first time in the game. **Cliffhanger.** Season 2 (the Ashen Wastes) starts with finding her.
- If the Masked Figure is adopted, they appear at the frozen gate as the player leaves, holding something that glows like an Echo.

This reveal is a **proposal**. It answers why the heroes all held on, without deciding the bigger question of whether the Void Sovereign is the first hero. It needs the team's OK before anyone writes Act 3.

---

## Side quests
| Quest | Region | Giver | What happens | Reward | Why it's there |
|---|---|---|---|---|---|
| **Names in the Bark** | Forest | Ilsa | Find 5 carvings of rangers' names and restore them with your Ember | Lore, cosmetic crest | Exploration and the "forgetting" theme |
| **Pip's Lost Friends** | Forest | Pip | Find 3 other moss sprites hiding from the rot | A pet cosmetic (looks only) | A light, cute quest to balance the tone |
| **The Hidden Grove** | Forest | Unlocked by returning the heartwood | A short hidden area with the forest's **hidden Echo** | Hidden Echo, lore | Rewards the "let go" choice with story, not power |
| **Ice-Crystal for Tobin** | Frostveil | Tobin (by letter) | Bring ice-crystal for the next upgrade tier | Gold, upgrade | Teaches the new material sink |
| **Hale's Oath** | Frostveil | Wren (only if you took the sword) | Give the sword back, or tell her why you won't | Wren's trust, or a cold goodbye | Pays off the Act 1 choice |
| **The Last Verse** | Frostveil | Gunnar | Find all song fragments | Cosmetic banner, Memory Journal entry | Exploration, lore |

Daily missions and the bounty board continue in both regions (the reasons to come back from [[Economy and Monetization]]).

---

## First playable cut (~15 minutes per chapter)
For each chapter release, the smallest version that tells the story:
- **Chapter 2:** 2.1 short (camp), 2.3 short (2 waymarkers), the Rootdeep cut to encounters then the Guardian, 2.5 memory and the heartwood choice.
- **Chapter 3:** 3.1 short (Wren), 3.3 (the frozen city), the Glacier Throne cut to encounters then the King, 3.6 memory, choice and the reveal.

Cut for later: 2.2, 3.2, 3.4, the dungeon puzzles and all side quests.

## Implementation notes
- **Flags:** `grove_fate`, `frostveil_fate`, `wren_told_truth` and `act2_complete` on `character.storyFlags`. Quest progress on `character.questState`. The first seal goes in `character.seals`. Echoes go in `character.echoes`.
- **Per-player visibility:** Bryn at the forest camp, Wren's version, Pip, the thawed citizens and the Void storm event are per-player. Never one shared copy in a region.
- **Corruption:** keeping the heartwood raises `character.corruption`. Visuals and dialogue only.
- **Levels:** enemies use each region's fixed level (Fred's notes). Main quests are tuned for the start of each range; the dungeons for the middle.
- **Disconnect:** checkpoint before each mini-boss and boss, like Act 1.
- **Reuse:** the brazier warmth from 3.3 can share code with the Act 1 Ember braziers.

## Open questions
- [ ] Is the Keeper reveal (she told the heroes to hold on) the right Season 1 cliffhanger?
- [ ] Do the Keeper's lines in Maren's memory give the twist away too early?
- [ ] Is Thaw vs Frozen balanced? Thawing makes Act 3 harder but adds content.
- [ ] Is a planned Echo for the forest's hidden grove too much power for the "let go" option?
- [ ] Names: Ilsa Thorne, Pip, Maren, Wren Hale, Old Gunnar, King Halvard. Keep or change?
- [ ] Gold numbers are placeholders scaled up from Act 1. Tune with Fred's XP curve.
