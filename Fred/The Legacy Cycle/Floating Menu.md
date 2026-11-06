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

- A PointLight on the panel part: warm firelight (255,170,96), brightness 1.6, range 12, no shadows. It turns ember red (240,92,48) on the Embers page.
- It fades in on open (0.4 s) and out on close (0.18 s), with a slow `math.noise` flicker on top.

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
