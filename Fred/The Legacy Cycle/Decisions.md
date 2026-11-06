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
- Each region has a fixed enemy level. Respec is free at a shrine, but not in combat or in dungeons.
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

## Main menu and settings (2026-11-06)

- A main menu comes before the loading screen. It has Play, Character Customization and Settings, and it shows **no username** (the user asked for it to be removed). See [[Join Flow]].
- Settings has Music and Sound effects volume sliders, a Camera shake toggle and a Low specs mode toggle. They are saved per player. See [[Settings]].
- There is no real music track yet, so the Music slider controls the World.Ambience background loop and any track added to the Music sound group later.

## Inventory and menu (2026-11-06)

- Body armour is split into **Chestplate** (upper body) and **Leggings** (lower body). The full equipment slot set is Helmet, Chestplate, Leggings, Gloves, Weapon and Boots. See [[Floating Menu]].
- **Embers** is the skill tree, opened from the floating menu. Its skills aren't designed yet, so the page is an unlit placeholder.
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

## Shrine of Embers (2026-11-06)

- The user's obsidian-brazier mesh replaces the old blocky shrine in Havenbrook. See [[Shrine VFX]].
- Its fire is an **ember, not a bonfire**: a weak smoulder over glowing coals.
- The user compared it with a Blender Mantaflow flipbook and picked the **native Roblox VFX** version (built-in fire and spark textures). The flipbook copy was removed.
- You absorb the Ember **once per player, and it is saved** (user, 2026-11-06). Afterwards the shrine stays dark for that player only.
- ~~The floating menu's light is a cool blue-white that lights the surroundings and slightly cools the world's tint while open.~~ **Replaced 2026-11-06:** the user asked for a colour that suits the book instead of the blueish light. The light, its glowing outline and the world tint are now **warm candle gold** (255,196,122), with a faint warm tint. The Embers page still goes ember red once the Ember is lit.
