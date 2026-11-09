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
