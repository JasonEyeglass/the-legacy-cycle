---
tags: [legacy-cycle, vfx, animation, training, blender]
updated: 2026-11-09
---

# VFX and Animation Training Log

Practice drills that turn the study notes into measured skill. Back to [[The Legacy Cycle]]. Built on [[Roblox VFX Techniques]].

## 2026-11-09

Five Blender practice drills run on 2026-11-09 (Blender 5.2). Each was critiqued against [[Roblox VFX Techniques]]-style rules and iterated until it passed. Every finding below was **Measured** in these drills, not taken from a tutorial. Files are in `Lihfolk_Vault\VFX Training\2026-11-09\`:
- `fx_anim_training.blend`: scenes `FX_Training` and `Anim_Training`;
- `slash_crescent_4x4_v3.png`: a usable 1024² OneShot sheet;
- `sword_slash_5beat.mp4`: the 3/4 view and the game camera side by side;
- arc-trace and game-camera stills.
- drills 3–5: `slash_layer_*_4x4.png`, `slash_layered_comp_dark.png`, `impact_burst_4x4_v4.png` and its preview, `hit_reaction_hitstop_*.mp4`, `hit_reaction_frames_18_27.png`. The `.blend` now also has the `FX_Burst` scene, plus `RIG_Target` and `CAM_Shoulder` in `Anim_Training`.

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

## Drill 3: layered slash (dark under-layer, main crescent, hot core)

**Setup:** three 4x4 sheets built on the drill 1 v3 crescent, meant to play as three emitters on the same attachment:
- **under:** dark and wide;
- **main:** the v3 crescent, tinted ember;
- **core:** thin and white-hot.

They are composited over light and dark backgrounds for judging. Files: `slash_layer_under_4x4.png`, `slash_layer_main_4x4.png`, `slash_layer_core_4x4.png`, `slash_layered_comp_dark.png`.

| Version | What was wrong | Fix |
|---|---|---|
| v1 | The under-layer sat outside the hot edge and read as a hard dark **outline** (a sticker look) | Move it **inside and behind** the arc: radius 1.27 (vs 1.3), width ×1.7, a soft inner edge (0.7), lagging the main sweep by a frame |
| v2 | **Passes:** the dark layer gives contrast on light ground without outlining, and the core reads white-hot, then cools to ember | n/a |

**Lessons:**
- **A dark under-layer is a shadow, not an outline.** Keep it inside the arc, soft and lagging. If it pokes out past the bright edge, it becomes a cartoon outline.
- **The hot core is short and thin:** width ×0.35 and gone by frame 9 of 16. White-hot first, ember after, gives the read of something cooling. A core that lasts as long as the body just looks flat white.

## Drill 4: impact burst flipbook (4x4, OneShot)

**Setup:** a polar-UV disc shader with:
- a flash;
- an expanding ring;
- wedge shards;
- noise breakup.

It renders 16 frames, which are tiled into a sheet and previewed with an ember tint. Files: `impact_burst_4x4_v4.png`, `impact_burst_v4_preview.png`.

| Version | What was wrong | Fix |
|---|---|---|
| v1 | Frames 13–16 empty; the ring froze after frame 3 (EXPO ease-out); beaded breakup; thin spoke shards; a muddy frame-2 flash | Use all 16 frames; QUAD ease-out on the ring; the flash only on frames 1–2 |
| v2 | Shard gaps looked like clock ticks; the dust read as a donut | Wider wedge shards whose width shrinks with distance, leading the ring |
| v3 | A hard **seam at 9 o'clock**, and hard-edged gaps | The angle from `atan2` jumps from +π to −π there, so 1D noise on the angle breaks. Drive 3D noise with `(cos a, sin a)·freq` instead; soften thresholds to 0.28–0.44 |
| v4 | **Passes** (dust is still donut-ish) | Dust belongs on its own sheet/emitter |

**Lessons:**
- **Any noise on a polar angle seams where atan2 wraps.** Sample noise with the direction vector `(cos a, sin a)` instead, so it is continuous all the way round.
- **EXPO ease-out freezes a ring.** It covers almost all the distance in the first frames, then barely moves. QUAD keeps it expanding through the life.
- **The flash is 1–2 frames only.** A longer flash goes muddy as it fades.
- **Shards should lead the ring and taper outward.** Thin spokes read as lines, not debris.
- **One sheet, one job.** Dust baked into the burst sheet fights the ring, so give it a separate emitter.

## Drill 5: hit reaction with hitstop (R6 target takes the drill 2 slash)

**Setup:**
- `RIG_Target` is a copy of the R6 rig without the sword. It is tinted red and faces the attacker.
- It is keyed with the 5 beats: guard (1–19), impact (20, the contact frame), extreme (24), stumble (32) and recovered guard (50). Every pose is built from armature-space rotations.
- Hitstop is a **playback remap**, not keys: the contact frame is shown H extra times, for H = 0, 3 and 6 frames at 30 fps (0/100/200 ms).
- It is judged from `CAM_Game` and from a shift-lock-style `CAM_Shoulder`: 12.5 studs, 25° pitch, offset 1.75 studs to the character's right.

Files: `hit_reaction_hitstop_shouldercam.mp4`, `hit_reaction_hitstop_gamecam.mp4` (H = 0/3/6 side by side) and `hit_reaction_frames_18_27.png`.

| Version | What was wrong | Fix |
|---|---|---|
| v1 | Head path dead-stopped at frame 26 (0.025 studs/frame) and re-accelerated into the stumble; the snap was mostly forward (dot 0.79) though the blade travels mostly down | A deeper drop (−0.75); make the extreme a **pass-through (AUTO) key** |
| v2 | Direction good (dot 0.86), but **the target was invisible from the game camera.** Ray casts from the camera hit the attacker's own head and torso: 11–33% of the target's upper body was visible, even with a shoulder offset and a 35° pitch | Move the target to where the blade really lands: the attacker's left, at x 2.2 (blade passes 0.2 studs from its axis). Contact is frame 20, not 21, so key the impact on 20 |
| v3 | **Passes:** dot 0.90 with the blade direction. Head spacing 0.57 → **0.61 peak** the frame after impact → decays to 0.03 at the bottom of the sink → a recovery bump of 0.19 (under ⅓ of the hit). 43% visible from CAM_Game, 67% from CAM_Shoulder | n/a |

**Lessons:**
- **At melee range your own character hides the target.** In a chase camera the target stands directly behind the attacker's head and torso. Place the hit (the hitbox centre and the contact point of the swing) to the side of the attacker's silhouette, and judge with ray-cast visibility, not by eye in a 3/4 view.
- **A shift-lock style offset to the right revealed a target on the left better** than the centred camera (67% vs 43%). Test the camera you will actually ship.
- **Down-moving reactions sink into the occluded zone.** The biggest visible motion should be sideways or at head height. The hit VFX, highlight and camera shake carry the read where the body can't.
- **Snap direction follows the blade.** Check `dot(blade velocity at contact, head displacement over the snap) > 0.7`.
- **The impact pose shows on the contact frame and is what hitstop freezes.** A freeze on the guard pose shows nothing.
- **A reaction's spacing peaks right after impact, then decays.** The turn-around from sink to recovery is a near-zero frame by nature. What matters is that the recovery peak stays well under the hit peak, or it reads as a second hit.
- **Hitstop is a runtime time remap.** Bake no hold keys: each client pauses both rigs' tracks (`AnimationTrack:AdjustSpeed(0)`) for H frames, then restores the speed. The 3f (100 ms) and 6f (200 ms) values are a rule of thumb (**Unverified**). The videos are built, but the right value is not yet judged by eye or in Studio.
- **Drill 5 changed drill 2's strike** (strike arm direction `(0.3, −0.95, −0.1)`, blade `(0.75, −0.55, −0.3)`) so the blade reaches a target at melee range. From the shoulder camera, the blade is now behind the attacker's head on frames 20–22. That is acceptable only because the slash VFX covers contact.

```python
# Visibility of a target from the gameplay camera: fraction of sample points not blocked by other objects.
import bpy
from mathutils import Vector
def visible_fraction(scene, cam, target_rig, bones=("Head", "Torso"), prefix="T_"):
    dg = bpy.context.evaluated_depsgraph_get(); o = cam.matrix_world.translation; v = n = 0
    for nm in bones:
        for dz in (0.2, 0.8, 1.4):
            for dx in (-0.6, 0, 0.6):
                p = target_rig.matrix_world @ (target_rig.pose.bones[nm].head + Vector((dx, 0, dz))); d = p - o
                hit, loc, nrm, i, ob, m = scene.ray_cast(dg, o, d.normalized(), distance=d.length)
                n += 1; v += (not hit) or ob.name.startswith(prefix)
    return v / n
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
- **A dark under-layer outside the bright edge:** it reads as a sticker outline.
- **Noise driven by a polar angle:** a seam where atan2 wraps.
- **EXPO ease-out on an expanding ring:** it freezes after a few frames.
- **Judging a hit reaction in a 3/4 view:** from the game camera, the attacker's own body hides the target at melee range.
- **Freezing the guard pose in hitstop:** key the impact pose on the contact frame.

## Checklist

- [ ] The flipbook shows the effect from frame 1, keeps moving through the hold and dissolves tail first.
- [ ] Tile margin ≥10%; checked on a tinted preview over a dark background.
- [ ] Weapon poses are built from arm and blade directions.
- [ ] Tip spacing: load peak about ⅓ of the strike peak; the strike is 2–4 frames; smooth decay after contact.
- [ ] The tip's height keeps descending after contact.
- [ ] Contact is visible from the game camera; the off-hand is chambered.
- [ ] The arm lags the torso by about 2 frames.
- [ ] The under-layer sits inside and behind the arc; the hot core is thin and dies by about ½ of the life.
- [ ] Polar effects sample noise with `(cos a, sin a)`; the ring eases out with QUAD; the flash lasts 1–2 frames; dust is on its own emitter.
- [ ] Hit reaction: snap dot with the blade velocity > 0.7; head spacing peaks right after impact; recovery peak < ⅓ of the hit.
- [ ] The target is ≥40% visible (ray cast) from the shipped camera at contact; the contact point is outside the attacker's silhouette.
- [ ] Hitstop is a runtime pause of both tracks, with the impact pose on the frozen frame.

## Next drills

- Study round 2 is pending: YouTube blocked transcript requests from this IP (HTTP 429, then IpBlocked). The queued videos are Blender VFX for Roblox (MaskedFaces), the anime sword slash (BlenderBuilder), hitstop (SugarPunch), sprite sheets from 3D (CoderNunk) and procedural cartoon flames (Blender Box).
- Judge the hitstop videos (0/3/6 frames) by eye, then in Studio with `AdjustSpeed(0)` on both tracks.
- Export the five-beat swing and the hit reaction to Roblox through the Luau pose-builder route. Fire the layered slash and the impact burst from a `Hit` marker on frame 20, the measured contact frame.
- Make a separate dust sheet for the burst; judge the layered slash in Studio at game distance.
