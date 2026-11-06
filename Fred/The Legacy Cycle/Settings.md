---
tags: [legacy-cycle, system, ui]
updated: 2026-11-06
---

# Settings

The player's settings are saved in PlayerData and replicated as `Setting_*` player attributes. Back to [[The Legacy Cycle]]. The decisions behind them are in [[Decisions]].

| Setting | Values | What it does |
|---|---|---|
| Music | 0–100% in 10% steps | Sets the `SoundService.Music` SoundGroup volume (currently the World.Ambience loop) |
| Sound effects | 0–100% in 10% steps | Sets the `SoundService.Effects` volume: combat sounds (`Feedback.sound` routes through it) and the fountain |
| Camera shake | On / Off | When Off, `Feedback.shake` (trauma) and `CombatCamera.kick` do nothing |
| Low specs mode | On / Off | Turns off GlobalShadows, Bloom, SunRays, DepthOfField and Blur, and halves combat sparks and drops their flash light. Turning it off restores what the place originally had |

## Code

- **`ReplicatedStorage.Settings`:**
  - `Defaults` is music 1, effects 1, shake on, low specs off.
  - `sanitize` always returns a full set, falling back to the default for any bad field. Volumes are rounded to tenths and clamped, and NaN is rejected.
  - Also: `publish`, `read`, `ready`, and `watch`. `watch` runs on the client, connects only once, and applies everything.
  - There is a self-check.
- **`ServerScriptService.Progression.SettingsService`:**
  - It publishes the settings when a profile loads.
  - `SettingsAction:FireServer("Save", values)` is sanitized, rate limited to 0.5 s, stored in PlayerData `settings` and published again.
- **UI:** the overlay in the [[Join Flow|main menu]].
  - The sliders take a drag or a tap, and the toggles are buttons.
  - A change applies locally right away. It is then sent to the server 0.6 s after the last change, which keeps sends under the server's rate limit.

## Not done yet

- There is no real music track.
- Low specs doesn't yet cover the movement speed-lines or the UI embers.
- The menus have no gamepad focus.
