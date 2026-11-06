---
tags: [legacy-cycle, system, ui]
updated: 2026-11-06
---

# Join Flow

**Join → Main menu → Play → Loading pan → "Forge your hero" (first join) or "Continue" → game.** Back to [[The Legacy Cycle]]. The decisions behind it are in [[Decisions]].

## Main menu: `ReplicatedFirst.MainMenu` (ModuleScript)

- It is the first screen after joining, laid out at 844×390 (a landscape phone) and scaled with a UIScale clamped to 0.6–1.8. It uses DisplayOrder 45.
- The look: embers, a turning rune circle with a breathing ember diamond in the middle, a soft glow made of stacked discs, and a gilded "The Legacy Cycle" title.
- **Play** fades to black and returns.
- **Character Customization** opens the [[Character Creator]], with `firstTime = false` so a new player can still cancel. Until the character and the `HasLook` attribute exist, it reads "Preparing your hero…".
- **Settings** opens the [[Settings]] overlay. It stays dimmed until the saved settings have arrived (`Settings.ready`), so an early change can't be overwritten when the save loads.
- `MainMenu.run()` blocks until Play is pressed and returns a `close()` function. It returns nil if the menu is already open.
- `close()` releases [[UI Kit (Ornament)|UiFocus]] and fades the black cover off whatever is underneath it.
- The [[Floating Menu]]'s **Back to Main Menu** button calls `run()` again mid-game. Pressing Play from there drops the player straight back into the game.

## Loading screen: `ReplicatedFirst.LoadingScreen` (LocalScript)

- It calls `MainMenu.run()` at the top, before it builds its own UI. After it takes UiFocus, it calls `closeMenu()`. Both screens are black at the hand-off, so the swap doesn't show.
- **The pan:**
  - The camera glides through the shots in `Workspace.Cinematics.LoadingPan`. There is one folder per shot, holding parts A and B, with Location, Detail and Duration attributes.
  - It runs from `BindToRenderStep("LoadingPan", Camera+10)` with cosine easing and FOV 55.
  - It forces the camera to Scriptable every frame, because the default camera grabs it back when the character spawns.
- **Streaming:** `RequestStreamAroundAsync` only lasts a while, so the screen re-requests the current and next shot every 2 s. The shot markers live in a Persistent model.
- **Progress:**
  - The bar starts at 0.2. It adds 0.3 when the game has loaded, 0.2 when the character exists, 0.2 once `HasLook` is known, and 0.1 once the area has streamed in.
  - The screen stays up for at least 3 s.
- **Then:**
  - On a first join it shows "Forge your hero", which opens the creator with `firstTime = true`.
  - Otherwise it shows "Continue" with "as @username · Level N".
- `finish()` fades out, unbinds the pan, returns the camera to Custom on the humanoid, releases UiFocus and destroys the gui.

## Shots (2026-11-06)

1. "Havenbrook" / "Levels 1–15", 9 s.
2. "Havenbrook Keep", 8 s.
3. "Old Ruins", 8 s.
4. "Havenbrook Market", 8 s. It was reframed so it no longer sits inside a roof.

To reframe a shot, move parts A and B in Edit mode.

## Related

- CombatCamera skips its update while the camera is Scriptable, so it doesn't drift the shot. See [[Systems Map]].
