---
tags: [legacy-cycle, decisions]
updated: 2026-11-06
---

# Decisions

These are the settled calls, newest last. The team repo's `3 ✅ Tracking/Decisions.md` is the shared copy; this note adds the ones made in build sessions. Back to [[The Legacy Cycle]].

## Core rules (2026-11-05/06)

- One character per player: no heirs and no retirement.
- No permadeath and no hardcore mode. When you die you respawn at the last shrine. Your gear loses durability, and you drop loot that you can go back and recover.
- Health is its own stat. You raise it with stat points you spend on level-up; it does not come from your level.
- Trading comes later, through a server-held escrow (Phase 5 of the adventuring roadmap).

## Stats and leveling (2026-11-06)

- Max level is 100, at 3 points per level, so 300 points in total. There is no cap per stat; the player decides.
- What each point gives:
  - Vitality: +3 HP.
  - Strength, Endurance and Ember: +0.5 each.
  - Ember stays hidden until Echoes.
- Each region has a fixed enemy level.
- **The Shrine of Embers only gives the Ember** (user, 2026-11-09): it has no stat reset and no appearance change. This replaces "respec is free at a shrine". There is no respec anywhere for now; the old code is in StatService's history. Looks change from the main menu.
- XP curve: about 100 hours to reach level 100.
  - `xpToNext(L) = round5(4.5 * L^0.78 * (10 + 2(L-1)))`
  - Enemy XP is `10 + 2(level-1)`.
  - The 4.5 is the pace knob to tune in playtests.
- Region levels:

| Region | Levels |
|---|---|
| Havenbrook | 1–15 |
| Ethereal Forest | 15–30 |
| Frostveil | 30–45 |
| Ashen Wastes | 45–60 |
| Shadowlands | 60–80 |
| Forgotten Realm | 80–100 |

- The team repo's docs don't define stats anywhere. The GDD v0.2 .docx is still unread, so check it before treating this spec as final.

## Hero creation (2026-11-06)

- Heroes are built from presets only:
  - 8 skin tones.
  - 4 FaceControls expressions.
  - 5 hairstyles, including Shorn (bald), with 7 tintable colours.
  - 4 outfit palettes.
- The name shown is the Roblox **username**, not the display name, and there are no typed names.
- The creator opens on your first join. You can change your look later at a Shrine of Embers (the V prompt).
- Catalog hair from the Avatar Marketplace could work by asset id through HumanoidDescription; it is moderated and has no scripts. The catch is that it can't be tinted by our hair colours, and the preview would have to be built on the server. We are waiting for asset ids the user picks; no ids get invented. See [[Character Creator]].
- **Creator Store hair models are banned.** Three free "No scripts" hair models hid a malicious Studio overlay script (see [[Studio Gotchas]]). We use our own `generate_mesh` hair instead.
- **Faces and hairs come from the user's own packs** (user, 2026-11-06). This replaces the 4 FaceControls expressions and the 4 generated hairs above: 61 face decals and 33 hairs plus Shorn. Both packs were audited (no scripts). Old saves keep their indices, so a returning hero's face and hair change to whatever now sits at that index.
- **A secondary hair** (user, 2026-11-06) is worn on top of the main one, from the same list and in the same colour. It is the last field of a look, so older saves load with none.

## Main menu and settings (2026-11-06)

- A main menu comes before the loading screen. It has Play, Character Customization and Settings, and it shows **no username** (the user asked for it to be removed). See [[Join Flow]].
- Settings has Music and Sound effects volume sliders, a Camera shake toggle and a Low specs mode toggle. They are saved per player. See [[Settings]].
- There is no real music track yet, so the Music slider controls the World.Ambience background loop and any track added to the Music sound group later.

- **The loading screen has no Continue button** (user, 2026-11-06). Once loaded, a click or tap anywhere carries on, as do Enter, Space and gamepad A.
- **Returning to the main menu asks first** (user, 2026-11-06). A parchment note on the book's right page offers Return or Stay.

## Controls (2026-11-09)

- **Shift lock is on Ctrl, not Shift** (user, 2026-11-09), because Shift sprints. Roblox's built-in shift lock stays off. Slide moved to C and R1 only.
- **The shift lock indicator is minimal:** a small dot and a thin gilt ring at the centre of the screen, with no label.
- **The controls strip hides itself** after 12 s on screen, and Caps Lock toggles it (user, 2026-11-09).
- **The ground impact and shockwave only come from a drop from a great height** (user, 2026-11-09): a fall speed of 110 studs/s or more, about a 30-stud drop (`Config.Traversal.ImpactLanding`). Ordinary jumps and double jumps still thump (shake and sound) but don't break the ground.
- **The ground impact is rocks** (user, 2026-11-09): slabs of the ground's own material burst up in a crater rim round the landing, chunks fly out, then everything sinks back. This replaces the dark cracked star.
- **The impact depends on the ground** (user, 2026-11-09). Stone and anything unlisted gives tilted slabs and blocky chunks; grass gives turf and dirt clods; mud slumps and spatters; sand and snow give a low lip and a cloud; ice and glass give clear shards; wood gives planks and splinters; metal gives sparks only; water gives a splash. A big drop into water now counts as a landing.
- **Wall climbing is a cliff or ladder climb** (user, 2026-11-09): you hang on the wall and move up, down and sideways with the move keys. Moving costs stamina (12 a second); hanging still costs none, but stamina doesn't recover while you're on the wall. At 0 stamina you can only climb down.
- **Jump shockwaves and the double-jump burst are wind, not fire** (user, 2026-11-09, "shouldn't have orange, it should be like wind"): pale white gust arcs that swirl as they spread, plus a puff of air. This replaces the orange ring with its dark under-ring and chips (the field-guide shockwave above) and the ember sparks and orange flash on double jumps and wall kicks. Dodges went to wind as well (user, same day): a slipstream of gusts replaces the flash and speed lines, and the afterimages stay.
- **Cleanup code uses Trove** (user, 2026-11-09). Trove is a cleanup library with nothing on screen; the visible rock impact the user had in mind is the `Rocks` effect above. Sleitnick's Trove sits at `ReplicatedStorage.Packages.Trove`, copied from its official GitHub source and read through before adding. New code that tracks connections or temporary instances uses it; existing code is only switched over when it's being changed anyway.
- **Shift lock eases instead of snapping** (user, 2026-11-09): the hero turns in over up to 0.45 s, and the view glides after the mouse. The hero faces exactly where the crosshair points.

## Embers and the dev space (2026-11-09)

- **Embers are skills** (user, 2026-11-09). Absorbing one plays a short cinematic in two shots: lighting the embers, then collecting its power. After it, the Embers page pops open in the floating menu with the skill tree.
- **The tree's shape is a placeholder:** three paths of two sealed skills out of the Ember. No skill names, costs or effects are invented; the nodes stay sealed until the skills are designed.
- **The dev space has a Hollow Squire and three Training Dummies** (user, 2026-11-09). Dummies never give XP or quest credit.
- **The dev space has a replayable Ember shrine** (user, 2026-11-09) for testing the cinematic. A `DevReplay` shrine never saves the Ember, so it must stay out of the real world.
- **The absorb cinematic is a bigger 5-beat cutscene** (user, 2026-11-09, "gotta be cooler"): orbiting shots, a graded picture, a pillar and ring of fire, spiralling motes, the hero lifted off the ground and an impact flash. See [[Shrine VFX]].
- **The skill tree's nodes are clickable** (user, 2026-11-09): a sealed node shows which path and step it is; the Ember core goes back.
- **VFX follow the field guide's curve rules** (user, 2026-11-09, picks 1, 3 and 4 of [[Roblox VFX Techniques]]): impacts pop with a 3-frame star, sizes ease out, transparency holds then fades, and shockwaves get a dark under-layer. Built-in particle textures only until the user uploads their own; ground winds wait for a wind texture.
- **Landed hits shake the camera harder** (user, 2026-11-09): up to 1.6 times the usual maximum angle, for your own hits only.

## Townsfolk (2026-11-09)

- **Havenbrook gets about 12 townsfolk** (user, 2026-11-09): pedestrians with simple routines between the market, fountain, doors, board, keep and gate. A few are named from the Act 1 outline (Orla Venn, Tobin); the rest are new and count as proposals.
- **Smart talk is a hybrid** (user): written lines picked by context (Ember, quest state, blade drawn, met before), plus answers to the player's chosen topic from Roblox's built-in `TextGenerator`, held to each person's persona and the world's lore. The written line is the fallback whenever the AI fails, is slow or is over budget.
- **The player only picks reply choices** (user): no typed chat, so it works the same on gamepad and phone.
- **Townsfolk wear the hero presets** (user): the hero's own body, dressed through `Appearance.apply`.
- **AI answers are off** (user, 2026-11-09). The user asked to remove the "AI-powered conversation" notice. Roblox requires that notice whenever replies may come from AI, so the AI was switched off instead (`UseAI = false`). Townsfolk now answer from written lines only, and the notice no longer shows. This replaces the hybrid above for now; the AI code stays and the rules in the next bullet apply again if it is turned back on.
- **Townsfolk react when bumped or hit** (user, 2026-11-09): a shove, a turn and a written line. These are never AI lines.
- **Townsfolk speech uses our own bubble** (user, 2026-11-09): minimalist, with no background (italic text over a thin gold rule), instead of Roblox's chat bubble.
- **No quests, prices or rewards from the AI.** The prompt forbids them, and its replies go only to the player talking.
- **Roblox's AI rules are followed** (2026-11-09): an AI notice shows for the whole conversation. AI memory lasts one conversation and is never saved, and each person gives at most 6 AI answers per player per 15 minutes. Cross-session AI memory or unlimited AI chat would count as "extended AI interactions", which force the Restricted (18+) label, so they stay out. Answer the Content Maturity questionnaire's AI questions before publishing.

## Inventory and menu (2026-11-06)

- Body armour is split into **Chestplate** (upper body) and **Leggings** (lower body). The full equipment slot set is Helmet, Chestplate, Leggings, Gloves, Weapon and Boots. See [[Floating Menu]].
- **Embers** is the skill tree, opened from the floating menu. Its skills aren't designed yet, so the page is an unlit placeholder. (2026-11-09: it now shows a sealed tree once the Ember is absorbed; see Embers and the dev space.)
- The floating menu has **Back to Main Menu**, which reopens the main menu mid-game.
- **The floating menu is an open grimoire** (user, 2026-11-06). The user called the old stacked-button panel slop. Of two Claude Design directions (A: obsidian slab with a tab rail; B: grimoire), they picked **B**. It is a fixed 760×440 spread: the left page has the level and chapters I–IV, the right page the open chapter. The Main page became the **Overview** ledger. Canvas: https://claude.ai/artifact/4wrPSPaMRBqVdkPurpwu4T
- **Then direction D, "Codex": the grimoire with a modern layout** (user, 2026-11-06). After seeing C, a modern tabbed inventory taken from a reference image, the user asked to combine it with B.
  - D keeps the book's leather, gilt and fonts.
  - The chapters move to tabs across the top.
  - The left page becomes cards to pick from (equipment, stats). The right page shows the picked thing's detail, with one solid gold action.
  - Overview shows the hero portrait.
  - Return to main menu is now a door icon next to close.
- **Then direction E, "Illuminated": painted art from the user's own asset sheets** (user, 2026-11-06). The user said D still looked slop and generated a parchment-grimoire mockup from my brief, then three clean asset sheets. They approved uploading them as four atlases. D's structure stays (chapters, a picker page and a detail page with one action), drawn with that art: ribbon bookmarks for the chapters, a wax-seal close, slot plates round an ink knight, drop-cap titles, ink drawings and a ribbon-seal action. See [[Floating Menu]].
- **The glowing outline is removed** (user, 2026-11-06). The painted book's edge frames it now, and the outline's 18 px margin went with it.

## HUD and main menu art (2026-11-06)

- **The HUD and main menu use the same illuminated art as the book** (user, 2026-11-06). The user generated a mockup and three clean sheets (HUD pieces, main-menu pieces, ink icons) from my prompts. They approved uploading them as three more atlases (GrimoireArt atlases 5–7).
- **The HP, Stamina and Ember gauges stay curved** (user, 2026-11-06, "we need to keep the curved UI"). They are not the straight bars from my prompt. The arcs carry the painted fill textures, a gilt rim and gold star ends.
- **The arc rims are true circles, not rotated rectangles** (2026-11-06). The user saw jagged edges, because Roblox doesn't antialias rotated frames. The rim and groove are now `UIStroke`s on round frames, clipped to the arc; see [[Studio Gotchas]].

- **Locking on shows the target's name and health** on the gilt lock-on bar at the top centre (2026-11-06). The sheet's lock bar had no use in the old HUD, which showed only the reticle and letterbox bars.

## Shrine of Embers (2026-11-06)

- The user's obsidian-brazier mesh replaces the old blocky shrine in Havenbrook. See [[Shrine VFX]].
- Its fire is an **ember, not a bonfire**: a weak smoulder over glowing coals.
- The user compared it with a Blender Mantaflow flipbook and picked the **native Roblox VFX** version (built-in fire and spark textures). The flipbook copy was removed.
- You absorb the Ember **once per player, and it is saved** (user, 2026-11-06). Afterwards the shrine stays dark for that player only.
- ~~The floating menu's light is a cool blue-white that lights the surroundings and slightly cools the world's tint while open.~~ **Replaced 2026-11-06:** the user asked for a colour that suits the book instead of the blueish light. The light, its glowing outline and the world tint are now **warm candle gold** (255,196,122), with a faint warm tint. The Embers page still goes ember red once the Ember is lit.

## Havenbrook Knight (2026-11-09)

- **Guards pick their fidgets at random** (user, 2026-11-09, "use all those randomly for the knight on guard"). The Idle loop runs all the time. A fidget plays every 5–11 s, never the same one twice in a row.
- **Fidgets play at Movement priority, above the Idle-priority loop.** Two tracks at the same priority average instead of one overriding the other, so a fidget at Idle priority would only half-play. Each fidget starts and ends on the stance, so the short 0.35 s fades don't pop.
- **Knights are tagged `KnightGuard`, not found by name**, so more guards just need the tag.
- **The armor is Blender meshes, uploaded as one Model** (user picked "Blender meshes"). The pieces are split by body part, so each can be welded to its part and follow every animation with no skinning. They are split by material too (steel, tabard, gold, leather, slit), so the colours come from MeshPart Color and Material with no textures to upload. Each piece is named `<BodyPart>__<Material>` and grouped by the game's slots: Helmet (Head), Chestplate (torso and upper arms), Gloves (lower arms and hands), Leggings (legs) and Boots (feet).
- **Armor pieces are welded, not skinned or Accessories.** Each MeshPart is joined to its body part with a Weld (`ArmorWeld`), whose C0 is the piece's part-local centre turned 180° about Y. The FBX importer faces meshes backwards, so the turn undoes that; a later re-import needs the same turn.
- **Under the plate, the knight's body is dark padding** (34,31,33), so the joints between plates read as cloth, not skin.

## Names and attacking (2026-11-09)

- **Only our nameplates; Roblox's name display is off everywhere** (user, 2026-11-09). Every Humanoid's DisplayDistanceType is None. Players, guards, townsfolk and Captain Aldric all wear the same plate: the name in Fondamento over a title in Merriweather. A player's title is their level, and their own plate is hidden from them.
- **Light and heavy swings slow you to a crawl** (user, 2026-11-09: first "they shouldn't be able to move and attack at the same time", then "more like make them really slow"). WalkSpeed is 4 (walking is 20) for 0.45 s after a light and 0.9 s after a heavy. A full stop felt too much. A dodge cancels it, so you can still evade out of a swing. The running heavy keeps its lunge, which is its whole point.

## Next checkpoint: the Act 1 slice (2026-11-09)

- **The next checkpoint is the Act 1 slice** (user, 2026-11-09, "Act 1 slice first", over finishing mining and forging first). That means the prologue plus Act 1's first playable cut (1.1, 1.3, 1.5, 1.6) from the team's outline, about 15–20 minutes, solo or with up to 3 friends. The plan and progress are in [[Act 1 Slice]].
- **Only slice work until it's done.** Polish that isn't on the slice's path waits.
- **The dungeon is built before the prologue.** Their old-Havenbrook art and the Fallen Knight / Caelan model are the same thing, so they get made once.
- **Only the part of roadmap Phase 1 the story needs is pulled forward:** items, inventory, equip and gold. Mining, smelting and forging come after the slice.

## Story state and gear (2026-11-09)

- **Every hero carries the Wanderer's Katana, and the Weapon slot is never empty.** You can swap blades but not unequip one, because the katana on your back is always drawn from the same model.
- **A weapon's power multiplies damage on top of Strength.** The Wanderer's Katana is ×1, the tarnished Hale's Oath ×1.1 and Hale's Oath ×1.25. These numbers are placeholders until the slice pass.
- **Story flags are short words with a short string, number or true/false value**, saved per player and replicated as `Story_<flag>` attributes so dialogue can branch on them. `knight_fate` is "saved", "took" or "left".
- **Quest state stays where it was** (the profile's `quest` and `done`). The plan's "questState" didn't need a new field.
- **Gold is saved and shown, but nothing spends it yet** (the open-question default in [[Act 1 Slice]]).

## Quest 1.1, A Spark in Havenbrook (2026-11-09)

- **The Keeper sends you without a model, for now.** Their line comes as a toast after the absorb cinematic. A Keeper NPC is still on the slice's asset list, for 1.6.
- **Story quests start by themselves** (`startsWithEmber`), never by talking to someone. If another quest is underway, 1.1 waits until that one ends.
- **The guild is Orla at the quest board.** No guild hall is built for the slice. The "old Ember lantern" stands beside the board.
- **1.1 pays 100 gold and 60 XP, not a sword.** Every hero already carries the Wanderer's Katana (the open question's default).
- **Talking to a townsperson counts for quest steps after the greeting is picked**, so Orla's greeting always answers the step you came for.

## Quest 1.3, The Wounded Knight (2026-11-09)

- **The watchtower stands outside the town's only gate, east of the road.** The outline says "the north gate", but Havenbrook has one gate.
- **Story quests are never offered by talking to someone.** They start by themselves, in order (`after`), once you have your Ember and nothing else is underway.
- **Bryn is one shared model, hidden on every client that isn't on 1.3.** One world copy is simpler than spawning one per player. The server still checks every choice, so a hidden Bryn can't be used.
- **The choice is made once.** `knight_fate` can't be changed after it's set.
- **Saving Bryn drains the Ember only for the rest of 1.3,** and only on the Ember arc, since nothing spends the Ember yet. Once Ember Strike exists (step 7), saving him should cost it properly.
- **Taking the sword gives the true Hale's Oath (×1.25).** The tarnished one is kept for a later beat (the dungeon's Hollowed Bryn).
- **Reply choices fit the quest:** a townsperson's quest topics come first while they apply.

## Enemies for the slice (2026-11-09)

- **Every placement so far is a placeholder** (user, 2026-11-09). The watchtower, Bryn and the dev-space spawns move once the real map and world design are done.
- **The hound and brute are stand-ins on the Squire rig.** The hound is a small, hunched humanoid and the brute a scaled-up soldier, so the gameplay can be built now. Real models (a four-legged hound with its own animations, a brute model) replace them later without code changes, as long as they keep the template names.
- **The brute has poise instead of more health.** Light hits can't interrupt its slam, so the fight is about reading the telegraph and parrying or dodging. A parry still stuns it, which is the opening.
- **Enemy kinds are data:** a template in `EnemyTemplate` plus an optional `Config.Enemy.Kinds` entry, picked by a spawn part's `Enemy` attribute. The dungeon (step 5) places spawn parts the same way.

## Town title and abandoning quests (2026-11-09)

- **Place names show at the top centre on entering, not under the minimap** (user, 2026-11-09). Zones are circles round map landmarks for now, until the real map has region bounds.
- **Every quest can be abandoned, and a story quest starts over.** Every quest so far is a main quest (Aldric's caravan included), so allowing only side quests would leave nothing to abandon. A story quest comes back by itself, so the story can't be lost. Choices already made stay made.

## Katana slash VFX (2026-11-09)

- **The slash is measured from each animation, ahead of time.** Each swing's cuts (timing and the tip's circle) live in `Config.SlashFx.Cuts`, so the crescent appears as the blade starts and grows with it. Sampling at run time could only draw it after the blade had passed. The cost: after editing a swing animation, re-measure its cuts (see Studio Gotchas). An animation with no cuts entry shows only the trail.
- **Our own drawn sheets instead of PhaseVFX for this one**, because the user asked to re-create the katana slash and it's a signature effect that has to match our swings.
- **Enemy slashes are void purple**, so a player can tell at a glance whose blade is coming.

- **Damage lands when the blade crosses the front** (user, 2026-11-09). The hit times come from the measured animations, one per light combo step. They aren't kept short for snappiness. Heavies now land about 0.2 s later than before, which also gives more time to parry an enemy's heavy.
- **The running heavy stays one hit** for now, though its animation cuts three times. Multi-hit would be a balance change.
- **A cut can be posed for the camera, not only measured.** When a swing's real plane is edge-on to the play camera (Light 2), its crescent is rolled to follow how the swing reads on screen, along the hands. The measured timing and radius stay.

## Music and UI sounds (2026-11-09)

- **The user's sound plan:** BGM-1 on the main menu, BGM-1 quiet under loading, BGM-2 in gameplay. UI: Click, Hover, Open and Close (windows, floating menu), Select (dialogue choices, interacting), Mission (quest taken, objective changed, quest abandoned), Quest Complete, Quest Failed.
- **Button sounds are global, not per button.** One hook covers every button in PlayerGui, so new UI gets sounds without code. An attribute opts a button out or swaps its click for Select.
- **Quest sounds play when their toast shows**, so a quest accepted mid-conversation sounds when the conversation closes, together with its "Quest Started" toast.

## Running lunge (2026-11-10)

- **One-handed thrusts are baked per frame by IK, not as Euler keys.** The hand target moves as a shoulder-relative direction plus a length, so its path never folds through the shoulder. Interpolating IK-solved keys as Euler angles flipped the wrist up to 170° per frame.
- **The arms stop 0.025 studs short of straight, and a foot out of reach slides toward its hip.** A straight two-bone chain has no defined twist, so it flicks between frames, and a foot left at an unreachable target detaches from the leg.

## Open world (2026-11-10)

- **One continent, far from the dev area** (user: "Whole continent, but place it far from our current places"). It is centred at (16000, 0, 0). The dev spawn stays where it is (user).
- **The terrain is generated by a script, not sculpted by hand.** Every size is a constant in `ServerStorage.WorldGen`, and a rebuild takes 82 s. Hand sculpting is lost on a rebuild, so terrain fixes go into the script.
- **Buildings and nature are a Blender kit plus a placer.** The user imports the GLBs (user: "I'll import them"). The placer measures each imported piece against the manifest for scale, and uses the lamp post's arm to tell whether the importer turned the kit 180°, so neither needs fixing by hand. Until the import, it places blockout boxes of the right size.
- **The story layout comes from the Act 1 beats:** north gate, then the Old Gate crack just outside it, then the fields, then the collapsed watchtower with Bryn. The guild hall faces the square, and Tobin's smithy stands by the north gate.
- **Quest objects stay in the old town until the switch.** Anchors in the new town mark where each one goes, so the move can be done in one step later without breaking quests now.
- **Generated terrain is checked by raycast, not trusted** (2026-11-10). The drawn ground once sat 2 studs above the generator's heights and buried every placed piece. Builds now fail if the two differ by more than 0.6 studs.
