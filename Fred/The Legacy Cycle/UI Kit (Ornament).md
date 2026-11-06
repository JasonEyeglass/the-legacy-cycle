---
tags: [legacy-cycle, ui]
updated: 2026-11-06
---

# UI Kit (Ornament)

The fantasy trim that every screen uses ([[Join Flow]], [[Character Creator]], [[Settings]], [[Floating Menu]]). The colours and fonts come from `ReplicatedStorage.Combat.Theme`: Iron, IronLight, Slot, Gold, GoldDim, Amber, Bone, BoneDim and Ember, with Fondamento for titles and Merriweather for body text. Back to [[The Legacy Cycle]].

## `ReplicatedStorage.Ornament`

| Function | What it does |
|---|---|
| `tween(inst, sec, props, style, dir, repeats, reverses)` | Shorthand for TweenService |
| `diamond(parent, size, pos, fill, z)` | A gold-stroked diamond |
| `shimmer(stroke, period)` | Sweeps a moving gold gradient along a stroke |
| `gild(label, glint)` | A gold text gradient, plus an optional glint every ~5 s |
| `corners(frame, size, inset)` | L-shaped corner brackets with studs |
| `innerRim(frame, inset)` | An inner rim line |
| `divider(parent, pos, width, anchor, z)` | A line with diamonds |
| `embers(parent, count, z) → stop` | Drifting ember motes (RenderStepped) |
| `runeCircle(parent, pos, size, z)` | Rings and counter-rotating squares, spinning once every 40 s |
| `breathe(stroke, from, to, period)` | A pulsing stroke thickness; sets the `Breathing` attribute |
| `button(btn)` | Hover grows it 1.04×, press shrinks it to 0.95×, plus an ember flash |
| `pop(gui)` / `reveal(gui, offset, delay)` | Entrance animations |

## `ReplicatedStorage.UiFocus`

`hold(gui) → release()` can be nested. While held, it:
- Hides every other ScreenGui, including ones added later.
- Hides the PlayerList, Backpack and EmotesMenu core GUIs. It leaves Health alone, because CombatClient turns that off on purpose.
- Blocks the combat keys through a ContextActionService action at priority High+100.
- Anchors the character.

## Gotchas this works around

- A UIGradient on a TextButton also tints its label, so buttons use solid fills.
- A UIListLayout also lays out decoration children, so keep ornaments off list frames.
- A second UICorner is ignored, so pass the radius to the helper instead.
- Fondamento's arrow glyphs render as boxes, so chevrons, plus signs and minus signs are drawn from bars.
- UIGradient has no radial mode, so a soft glow is made of stacked faint discs.
