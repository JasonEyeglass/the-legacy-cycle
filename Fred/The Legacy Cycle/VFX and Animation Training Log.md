---
tags: [legacy-cycle, vfx, animation, training, blender]
updated: 2026-11-09
---

# VFX and Animation Training Log

Practice drills that turn the study notes into measured skill. Back to [[The Legacy Cycle]]. Built on [[Roblox VFX Techniques]].

## 2026-11-09

Two Blender practice drills run on 2026-11-09 (Blender 5.2). Each was critiqued against [[Roblox VFX Techniques]]-style rules and iterated until it passed. Every finding below was **Measured** in these drills, not taken from a tutorial. Files are in `Lihfolk_Vault\VFX Training\2026-11-09\`:
- `fx_anim_training.blend`: scenes `FX_Training` and `Anim_Training`;
- `slash_crescent_4x4_v3.png`: a usable 1024² OneShot sheet;
- `sword_slash_5beat.mp4`: the 3/4 view and the game camera side by side;
- arc-trace and game-camera stills.

## Drill 1: slash crescent flipbook (4x4, OneShot, white with alpha)

**Setup:**
- A crescent mesh with UVs: u runs along the arc, v across it.
- One emission material. Its alpha is the head sweep × tail clip × soft inner edge × noise erosion.
- Three animated Value nodes: Head, Tail and Erode.
- An ortho camera, 16 frames at 256² in EEVEE with a transparent film, tiled with `ffmpeg tile=4x4`.

| Version | What was wrong | Fix |
|---|---|---|
| v1 | Frame 1 empty (dead time in OneShot); frames 4–9 frozen; flat values; soft blob head; the dissolve looked like a wipe and only showed on the last 2 frames | Start the sweep at 40% on frame 1; drift scale 1→1.05 and rotation 10° over the whole life; streaks plus a hot rim; skewed taper `u^1.6·(1−u)^0.6` |
| v2 | Drift pushed the arc out of the tile (a hard edge in Roblox); ease-in on the erosion hid it until frame 14 | Add a margin (ortho 3.3 for a radius-1.3 arc); linear erosion over frames 6→16; shorter hold |
| v3 | **Passes:** pointed head, clean margins, sweep on frames 1–4, a short drifting hold, a streaky tail-first dissolve over frames 12–16 | n/a |

**Lessons:**
- **The first frame of a OneShot sheet must already show the effect.** Start a sweep part-way (a smear) rather than from nothing.
- **A hold must keep moving.** A few percent of scale plus a few degrees of rotation over the life is enough.
- **Leave a margin of at least 10% of the tile.** Animate the drift in the preview before choosing the camera scale.
- **Eased thresholds hide the dissolve.** A noise threshold with an ease-in stays invisible for most of its range, so drive it linearly.
- **A greyscale sheet tinted in Roblox never reaches white.** The rim-to-body gradient gives the shape, but the white-hot core needs a *second*, smaller emitter. That's the layering rule.

## Drill 2: five-beat sword slash on an R6 block rig (30 fps, 50 frames)

**Setup:**
- An armature with R6 proportions: Root, Torso, Head, arms and legs. Block parts are bone-parented.
- A sword in the right fist, with an empty on its tip.
- Ortho front, side and top cameras, a 3/4 camera and a **game camera** (behind and above).

**Final timing:**

| Frame | Beat |
|---|---|
| 1 | Idle |
| 12 | Anticipation key (furthest reach-back) |
| 17 | Held anticipation: the torso starts unwinding, the arm stays back |
| 19 | The torso reaches the strike pose; the arm lags |
| 21 | Contact: the arm catches up |
| 29 | Follow-through |
| 38 | Settle |
| 50 | Recovery |

| Version | What was wrong | Fix |
|---|---|---|
| v1 | Arm Euler angles chosen by hand: the blade pointed straight up at the strike (an R6 grip puts the blade on the arm's local −Z, so an arm held out forward points the blade up) | Pose by **world directions**: give each key an arm direction and a blade direction, build the bone matrix (Y = arm, Z = −blade, Gram-Schmidt), set `pose_bone.matrix`, use quaternions with a hemisphere fix |
| v2 | Spacing chart: wind-up peak 2.8 vs strike 5.4 studs per frame, so the load was too fast. A hitch at contact: 1.35 → 0.39 → 0.57 | Load stretched to 12 frames (peak 2.05 vs 5.85, about 1:3); **unclamped (AUTO) handles on pass-through keys** |
| v3 | Front view: the tip dipped to its lowest point at contact, then rose again in the follow-through (distovi's "dip and rise") | Lower the follow-through blade direction; z now descends from 6.9 to 1.04 |
| v4 | A small re-acceleration in the follow-through (0.24 → 0.47) from an extra key at frame 25 | Delete the key: spacing decays 5.85, 2.38, 1.55, 1.41, 0.97, 0.58, 0.34, 0.23 … |
| v5 | **Game camera:** the blade was hidden behind the torso at contact | Widen the strike arc to the left, out of the silhouette |
| v6 | The off-hand arm still hid the blade at frame 21 | **Chamber the off-hand into the chest at the strike.** Passes: the blade is visible on every strike frame |

**Lessons:**
- **Pose by silhouette and direction, not by Euler numbers.** Solving from where the arm points and where the blade points worked first time. Hand-typed angles never did.
- **Measure the motion.** Trace the weapon tip every frame and read three numbers:
  - spacing (studs per frame): a slow load and a fast strike, at about 1:3;
  - the height profile after contact: it must keep descending, with no dip and rise;
  - occlusion from the game camera.
- **Clamped handles on a pass-through key make a dead stop.** Clamp only true holds and extremes.
- **Every extra key in a follow-through is a chance for a hitch.** Let the contact key ease straight into the end pose.
- **Overlap is cheap:** the torso reaches the strike pose 2 frames before the arm.
- **Always judge from the gameplay camera.** A forward cut is occluded at contact by the attacker's own torso and off-hand. Fix it with a wider arc and a chambered off-hand, and let the slash VFX carry the read.

```python
# Spacing chart for a Blender animation: studs per frame of an object (e.g. a sword-tip empty).
import bpy
sc, tip = bpy.context.scene, bpy.data.objects["SwordTip"]
pts = []
for f in range(sc.frame_start, sc.frame_end + 1):
    sc.frame_set(f)
    pts.append(tip.matrix_world.translation.copy())
spacing = [round((pts[i] - pts[i - 1]).length, 2) for i in range(1, len(pts))]
heights = [round(p.z, 2) for p in pts]
print(spacing, heights)
```

## Common mistakes

- **An empty first frame in a OneShot flipbook:** it delays the effect by one frame.
- **A frozen hold:** the effect looks dead between the sweep and the dissolve.
- **No tile margin when the effect drifts:** the shape gets clipped into a hard edge.
- **An eased-in dissolve threshold:** the dissolve only shows in the last frames.
- **Expecting a tinted greyscale sheet to glow white:** use a second emitter for the hot core.
- **Typing Euler angles for a weapon pose:** the grip axis flips the blade.
- **Clamped handles on a pass-through key:** a dead stop, then re-acceleration.
- **Extra keys in the follow-through:** hitches.
- **Judging a combat animation from the front:** players watch it from behind, where the cut is occluded.

## Checklist

- [ ] The flipbook shows the effect from frame 1, keeps moving through the hold and dissolves tail first.
- [ ] Tile margin ≥10%; checked on a tinted preview over a dark background.
- [ ] Weapon poses are built from arm and blade directions.
- [ ] Tip spacing: load peak about ⅓ of the strike peak; the strike is 2–4 frames; smooth decay after contact.
- [ ] The tip's height keeps descending after contact.
- [ ] Contact is visible from the game camera; the off-hand is chambered.
- [ ] The arm lags the torso by about 2 frames.

## Next drills

- Study round 2 is pending: YouTube blocked transcript requests from this IP (HTTP 429, then IpBlocked). The queued videos are Blender VFX for Roblox (MaskedFaces), the anime sword slash (BlenderBuilder), hitstop (SugarPunch), sprite sheets from 3D (CoderNunk) and procedural cartoon flames (Blender Box).
- Drill 3: a dark under-layer plus a hot-core companion sheet for the v3 crescent, judged in Studio at game distance.
- Drill 4: export the five-beat swing to Roblox through the Luau pose-builder route, and fire the slash VFX from a `Hit` marker on frame 21.
