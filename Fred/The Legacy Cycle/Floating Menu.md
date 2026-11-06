---
tags: [legacy-cycle, system, ui]
updated: 2026-11-06
---

# Floating Menu

`StarterPlayerScripts.CombatClient.FloatMenu` is an iron-and-gold panel hanging in the world beside the character, opened with **M**. It is drawn on a SurfaceGui in PlayerGui, attached to a part under the camera's world-UI folder. Back to [[The Legacy Cycle]]. The decisions behind it are in [[Decisions]].

## Pages

| Page | Size (px at 60 px/stud) | Contents |
|---|---|---|
| Main | 360×340 | Inventory, Stats, Embers, Back to Main Menu, "M to close" |
| Inventory | 680×430 | Hero preview with equipment slots, and a 12-slot pack |
| Stats | 460×410 | Level, XP and points; + spends a point (StatAction) |
| Embers | 460×380 | The skill tree. A placeholder for now: an unlit emblem in a rune circle |

Switching pages morphs the panel to the new size while the contents cross-fade.

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
  - Taller pages grow downward from the top edge of the Main page.
  - **Floor check (2026-11-06):** a ray down from the panel lifts any page whose bottom edge would come within 0.6 studs of the ground. The big Inventory page used to clip through the ground.
- **First person (2026-11-06):** when the camera is within 3 studs of the head, the panel moves straight ahead of the eyes.
  - It sits far enough away to fit on screen (with a 1.4 margin) and faces the eye.
  - It keeps the direction it had when the view went close. In first person the cursor is the centre of the screen, so you aim at buttons by looking around.

## Light (2026-11-06)

- **Cool light (2026-11-06, the user asked for it cooler and lighting the environment):**
  - A PointLight on the panel part: cold arcane blue-white (170,205,255), brightness 2.4, range 20.
  - Shadows are on for your own panel (off in low specs) and always off for other players' ghosts.
  - It has a slow shimmer rather than a fire flicker, and it lights the ground, walls and nearby characters.
  - The Embers page turns it ember red (240,92,48), but only once the player has absorbed their Ember. See [[Shrine VFX]].
- **World tint:** while the menu is open, `Lighting.FloatMenuTint` (a ColorCorrection on this client only) cools the whole view: tint (226,236,255), saturation -0.12, brightness -0.03. It fades with the panel.
- **Glowing outline (2026-11-06, the user asked for a lighting outline):** a glowing edge traces the panel in the light's colour, so it is blue-white, or ember red on a lit Embers page.
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
