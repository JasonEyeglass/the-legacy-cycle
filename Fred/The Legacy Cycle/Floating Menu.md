---
tags: [legacy-cycle, system, ui]
updated: 2026-11-06
---

# Floating Menu

`StarterPlayerScripts.CombatClient.FloatMenu` is an **open grimoire** hanging in the world beside the character, opened with **M**. It is drawn on a SurfaceGui in PlayerGui, attached to a part under the camera's world-UI folder. Back to [[The Legacy Cycle]]. The decisions behind it are in [[Decisions]].

## Layout (redesigned 2026-11-06, direction D)

The design is direction **D, "Codex"**, from the Claude Design canvas https://claude.ai/artifact/4wrPSPaMRBqVdkPurpwu4T. The user first picked B, "Grimoire" (over A, the obsidian tab rail), after calling the old stacked-button panel slop. They then asked to combine B with C, a modern tabbed inventory taken from a reference image. D is that combination.

- **One fixed spread:** 760×440 canvas px (12.7 × 7.3 studs at 60 px/stud). The panel doesn't morph between pages.
  - Umber leather around two pages with a stitched gold spine.
  - Fonts: Fondamento for titles, Merriweather for body and bold numbers, Josefin Sans for labels.
  - Icons (door, close, katana) are drawn from thin frames, so no images need uploading.
- **Top strip, on the leather:**
  - Tabs **I Overview, II Inventory, III Stats, IV Embers**. The open chapter's tab is gilt-rimmed. Stats shows a gold pill with the number of unspent points, and Embers shows a "Lit" pill once the Ember is absorbed.
  - A level box (diamond, "Level N · xp / need" and the XP bar), then **Return to main menu** (door icon) and close.
- **Left page:** what you pick from, as cards. The picked card gets a gold rim, a warm wash and a soft gold glow.
- **Right page:** the picked thing's detail. It has a title, a subtitle, a gold rule, ATTRIBUTES rows with dotted leaders, one solid gold action, and a hint under it.
- Switching chapters cross-fades both pages.

| Chapter (page key) | Left page | Right page |
|---|---|---|
| I Overview (`Main`) | The live hero portrait: a script-free clone playing the katana idle | "Your hero", the level and XP. Equipped, Carried (0 of 12), Points to spend, Ember (Burning / Not yet found). **Spend N points** (or View stats) goes to Stats. Hint: "M opens and closes this book" |
| II Inventory | **Equipped 1 / 6** and **Pack 0 / 12** sub-tabs. Equipped is the six slot cards (3×2), with the Katana drawn on the Weapon card; Pack is a 4×3 grid with "Your pack is empty" | The picked slot: the item, or "Helmet · nothing equipped". Slot, Strength bonus (Weapon only), Status. **Unequip / Equip** is disabled, with the hint "Equipment changes come with the item system". On Pack: "Your pack", Carried, Space left |
| III Stats | "N points to spend", then a card per stat (Vitality, Strength, Endurance) with its effect and the points spent | The picked stat: "Stat · raises your damage", and so on. Points spent, Effect now, Points left. **Spend a point** (StatAction) is disabled at 0 points. Hint: "Reset your points at a Shrine of Embers" |
| IV Embers | The turning rune circle with the Ember diamond (ember-lit once absorbed) | "The Ember": Status, Skills (None yet), Found at (Shrine of Embers). There is no action; the hint says what to do next |

- The page keys stay `Main`, `Inventory`, `Stats` and `Embers`, because MenuPresence checks them.
- Everything redraws from player attributes (`Level`, `XP`, `XPToNext`, `StatPoints`, `Stat_*`, `HasEmber`) through one `refresh()` on `AttributeChanged`.
- Gamepad: opening a chapter selects its first control: the Overview action, the Weapon card, the first stat card, or the Embers tab.
- **Interaction feedback (2026-11-06, the user said it lacked button states and a cursor):** the code is `control()` with the `LOOKS` table in FloatMenu. The looks are `tab`, `card`, `subtab`, `filled`, `secondary` and `close`. The canvas's "B · Control states" sheet still shows the older B controls.
  - Every control has rest, hover, pressed, gamepad-selected and disabled (or current) looks: a gold wash, the rim's opacity and colour, the ink colour and a scale. A current tab or picked card holds its look under hover.
  - Hover grows the control by 2%. A press sinks it to 97% and it springs back on release. Keyboard and gamepad activations also give a 0.09 s dip.
  - **Cursor:** `UserInputService.MouseIcon` shows the system pointing hand over an enabled control and the forbidden sign over a + with no points. It clears on leave and when the book closes.
  - **Gamepad:** a gold ring (`SelectionImageObject`) replaces Roblox's blue selection box.
  - **Sound and effects:** a faint tick on hover (at most one every 0.06 s). Close turns blood red on hover. Spending a point flares the stat number gold and pops it.
- The back face is the book's leather cover with a gold sigil. Other players' ghosts show the same book, with the page title and blank chapter lines.

## Equipment slots (2026-11-06)

| Left (head to legs) | Right |
|---|---|
| Helmet | Gloves |
| Chestplate (upper body) | Weapon (Katana) |
| Leggings (lower body) | Boots |

There is no item system yet. `EQUIPPED` is fixed at `{Weapon = "Katana"}`, and nothing is saved.

## Placement

- **Third person:**
  - The panel sits up and ahead of the character, shifted to the right by half its width. It eases after the character, sways, bobs and faces a point behind the character.
  - **Floor check (2026-11-06):** a ray down from the panel lifts the book if its bottom edge would come within 0.6 studs of the ground. The old big Inventory page used to clip through the ground.
- **First person (2026-11-06):** when the camera is within 3 studs of the head, the panel moves straight ahead of the eyes.
  - It sits far enough away to fit on screen (with a 1.4 margin) and faces the eye.
  - It keeps the direction it had when the view went close. In first person the cursor is the centre of the screen, so you aim at buttons by looking around.

## Light (2026-11-06)

- **Warm light (2026-11-06):** the user first asked for a cooler light that lights the environment. Later the same day they asked for a colour that suits the book instead of the blueish one.
  - A PointLight on the panel part: warm candle gold (255,196,122), brightness 2.4, range 20.
  - Shadows are on for your own panel (off in low specs) and always off for other players' ghosts.
  - It has a slow shimmer rather than a fire flicker, and it lights the ground, walls and nearby characters.
  - The Embers page turns it ember red (240,92,48), but only once the player has absorbed their Ember. See [[Shrine VFX]].
- **World tint:** while the menu is open, `Lighting.FloatMenuTint` (a ColorCorrection on this client only) gives the whole view a faint warm cast: tint (255,241,222), saturation -0.06, brightness -0.02. It fades with the panel.
- **Glowing outline (2026-11-06, the user asked for a lighting outline):** a glowing edge traces the panel in the light's colour, so it is warm gold, or ember red on a lit Embers page.
  - Five stacked UIStrokes run from a bright core line (the light colour pushed toward white) out to a faint halo 16 px wide.
  - A brighter band on the core line turns round the rim at 40°/s.
  - It is drawn on both faces and on other players' ghosts, and it shimmers, pops and fades with the light.
  - The SurfaceGui part is 18 px (`PAD`) larger on every side than the page so the halo has room. The page itself is inset by the same amount, so the layout and the floor check are unchanged.
- It fades in on open (0.4 s) and out on close (0.18 s).
- **Embers page:** "You have not found your Ember" / "Seek the Shrine of Embers in Havenbrook" until the `HasEmber` attribute is true, then "Your Ember burns", with a lit ember diamond and halo.

## Seen by other players (2026-11-06)

- The client fires `ReplicatedStorage.MenuAction` with the open page, or nil on close. `ServerScriptService.MenuPresence` checks that the page name is real, merges bursts into one update every 0.15 s, and sets the player's `MenuPage` attribute.
- Every other client draws a **ghost** of that panel beside the player. It has the same size, morph and placement (`besideCFrame`), the page title, blank rows and the light, but **not the contents**, so the inventory and stats stay private.
- Low specs turns off other players' panel lights, but keeps your own.

## Back to Main Menu

- It closes the panel and runs `CombatClient.ReturnToMenu` (2026-11-06):
  1. Over 1.6 s the camera rises 16 studs and pulls back 14 studs, looking down at the character, while the screen darkens and embers drift. The HUD is hidden.
  2. `ReplicatedFirst.MainMenu.run()` opens (see [[Join Flow]]).
  3. On Play, the camera goes back to Custom on the Humanoid.
- **Risk:** the world keeps running behind the menu, and enemies could hit a player who opens it mid-fight.
