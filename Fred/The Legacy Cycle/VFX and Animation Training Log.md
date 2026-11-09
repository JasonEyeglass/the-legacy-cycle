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
- **Correction from study round 2** ([[Roblox Dev Study - Round 2]], Sakurai's hit-stop techniques). This drill's freeze is the bare minimum. Smash also:
  - blends the victim *into* the damage pose over about 4 frames during the stop, rather than holding one pose;
  - shakes the victim's visual (not the hurtbox) more than the attacker's: sideways on the ground, decaying across the stop;
  - lets the attacker creep at a sub-frame speed;
  - scales the stop by damage × a per-move coefficient.
  The shake was skipped here, so that is the next iteration.
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

## Drill 6: two-handed katana M1 on an R15 rig, in Roblox Studio (30 fps, 37 frames)

**Setup:**
- `workspace.R15Rig` in The Legacy Cycle's place, with the game's katana (`ServerStorage.Combat.Katana`, grip motor C0 `(0, -0.15, 0)` from the hand, rotation (180°, 0, -90°); blade tip 6.1 studs up the handle).
- The rig's joints are **AnimationConstraints** (Avatar Joint Upgrade), not Motor6Ds. A Pose's CFrame is the joint Transform, and every attachment has identity rotation.
- Toolkit `ServerStorage.AnimTraining.R15Swing` (copy in `VFX Training/2026-11-09/r15/R15Swing.lua`), built in four stages:
  1. **Keys describe the weapon, not the joints:** hilt point, blade direction and edge direction, plus pelvis, chest, both feet and a look target, all in HumanoidRootPart space.
  2. **Every frame is solved, then baked.** Two-bone IK puts both hands on the hilt (right at the guard, left 0.75 studs toward the pommel) and plants both feet. The bake writes one Keyframe per frame.
  3. **The torso fits itself.** When an arm can't reach, the chest takes the smallest yaw/pitch offset that fixes it.
  4. **The off-hand can let go.** An `lgrip` weight (0–1) releases the left hand to the hip.
- Output: `R15Rig.AnimSaves.KatanaM1_Train1`, which the clip editor loads, with a `Hit` marker on frame 15 (contact). The bake script is `r15/BakeKatanaM1.lua`.

**Beats:**

| Beat | Frames | Pose |
|---|---|---|
| Idle | 0 | Guard, blade forward-up |
| Anticipation | 7, held to 11 | Load over the right shoulder, chest twisted 30° right |
| Action | 13 in-between, 15 contact | Tip up, then the cut |
| Follow-through | 18 strike end, 25 settle | Left hand released |
| Recovery | 36 | Guard, regrip |

| Version | What was wrong | Fix |
|---|---|---|
| v1 | Strike crammed into 3 frames: blade turn **87°/frame**, tip 9.5 studs/frame. The 6-stud blade pointed 45° down in follow-through, putting the **tip 0.7 studs under the floor**. Both arms over-reached in follow-through | Add a tip-up in-between key and spread the strike over 4 frames. Keep the blade slope ≥ −0.3 at hip height |
| v2 | Peak turn 55°, the tip stays above the floor, but the arms over-reach on 14 frames. The chest twisted 25° left into the follow-through, which pulls the left shoulder back away from the pommel | **Hips rotate, shoulders stay square** (pelvis yaw to 18°, chest about 0–5°). Add the auto torso fit as a safety net |
| v3 | Recovery swept the blade flat through forward-left. That puts the left hand (on the pommel) 0.65 studs right of the right hand, and the fit needed up to a **40° chest twist, jumping 25°/frame**. A tip-up recovery in-between was no better | **Release the off-hand** after the follow-through (lgrip 1→0, frames 18→25) and regrip on the way to guard |
| v4 | Load peak was 66% of the strike (eased "out" over 6 frames) | Make the windup ease-in-out over 7 frames: 46% |
| v5 | **Passes the measured gates** (row below) | n/a |

**v5 measured:**
- Strike peak 5.69 studs/frame (**171 studs/s**) on frame 16, the frame after contact.
- Max blade turn 55°/frame; tip at least 0.83 studs above the floor.
- No arm or leg over-reach; chest fit ≤6°; the planted foot drifts 0.000 studs.
- On the baked clip, replaying it through FK matches the solver (error 0.00000) and both grip points sit on the hilt (gap 0.000).
- Statue captures: `r15/katana_m1_statues_row.jpg` and `r15/katana_m1_statues_closeup.jpg`.

**v6: limb clearance (after review: "limbs are too close to each other, it's clipping")**
- v5 passed every motion gate but was never checked for clipping. An oriented-box overlap test (separating axis) between unjoined parts, plus the blade against the body, found **all 37 frames clipping**: forearms 0.7–1.3 studs inside the chest, the two forearms up to 1.1 studs inside each other, and the thighs touching.
- **With the arms attached at the shoulder, block R15 has no clip-free two-hand grip.** A scan of more than 900 reachable hilt placements, at three blade angles and three chest twists, found the best still clipping 0.41 studs. The left forearm has to cross the 2-stud torso to reach a centred hilt.
- **Limb detachment fixes it.** Pose CFrames carry translation, so slide both shoulder joints forward (0.3, or 0.5 forward and 0.15 down). Combined with hands 1.0 stud apart (block hands are 1 stud deep, so 0.75 made them overlap) and the hilt 1.6–2.0 studs out front, contact drops to about 0.1–0.2.
- **Hip detachment:** drop the thigh sockets 0.2 and spread them 0.1 (`HIP_DETACH`). The block thigh top otherwise pokes 0.19 into the LowerTorso, and in a crouch it tilts up into the chest. At −0.2 the thigh sits flush with the LowerTorso's bottom face, so no gap shows.
- **Elbows point out, not down.** The out-pole won every search; down-and-back poles drive the forearms into the chest.
- **A blade pointing to the character's left crosses the forearms** (the right hand ends up left of the left hand), so the strike end clipped 0.67 whatever the placement. Release the off-hand right after contact instead. The release path arcs outward, because a straight line from hilt to hip runs through the torso.
- **Keep the torso turning through the follow-through** (chest 0 → 55 → 50 → 50). Unwinding it straight after contact snapped 40°.
- **An out-of-reach IK target must not pull the hand off the wrist.** The solver now pins the hand to the arm's end (gap 0.0000).
- **Tooling:**
  - `M.clipping` measures the overlap.
  - A per-key search over hilt offset, chest twist, elbow poles and shoulder detachment places the keys.
  - `M.plan` picks per-frame poles, detachment and small chest/hilt nudges with a Viterbi smoothness pass, so choices don't flicker.
  - `M.bakeFrames` bakes the result.
- **Result:** `KatanaM1_Train2` (Train1 is kept for comparison).
  - Worst limb contact 0.23 studs, from 1.34. Nothing over 0.25; 9 frames between 0.15 and 0.23, all at the two-hand grip.
  - Blade turn ≤50°/frame, strike 168 studs/s, load 50%, tip at least 0.83 studs above the floor.
  - Hands never leave the wrists; the right grip gap is 0.000.
  - Replaying the baked clip matches the planned frames (error 0.00000).
  - Leftovers: the chest turns 28° in one frame into contact, and the left hand trails its release path by 0.3 on frame 16.
  - Files: `r15/BakeKatanaM1_v2.lua` (reproduces the clip to 0.003) and `r15/katana_m1_v2_clearance.jpg`.

**v7: the same swing rebuilt in Blender (after "try it in Blender")**
- **Setup:** `r15/r15_blender.py` builds its own scene, `R15_Katana`, in the training .blend. A copy is saved as `r15/r15_katana_m1.blend`.
  - It is a block R15 with the exact `R15Rig` part sizes and joint pivots, read from Studio. One bone per part, with the bone head on the joint.
  - Axes map Roblox (x, y, z) to Blender (x, −z, y).
  - Same keys and sampler as Train2, but **Blender's own IK** solves the limbs: two-bone IK with pole targets, elbows and knees hinge-locked to local X (elbows bend 0–160° forward, knees backward).
  - Hands and feet copy the rotation of keyed target empties. The head uses a Track To constraint.
  - Limb detachment is a keyed bone location on the upper arm and thigh.
  - The same oriented-box clipping test is ported to Python.
- **Gotcha: a straight IK chain never bends.** Block limbs rest dead straight. Blender's solver starts from the pose and can't pick a bend direction, so the hand stopped up to 0.6 short. A 30° seed bend in the lower-limb rotation fixes it, with a miss of 0.000.
- **Gotcha: calibrate pole angles numerically.** Sweep `pole_angle` and keep the value that puts the elbow on the pole's side (right arm −80°, left arm −110°, knees 90°). Don't guess from bone roll.
- **Gotcha: a bone-parented object hangs off the bone's tail.** Setting `matrix_world` straight after parenting gave garbage. Set `matrix_basis = (bone.matrix_local @ T(0, length, 0))⁻¹ @ target` instead.
- **Fixed controls aren't enough:** shoulders 0.3 forward and elbows out everywhere gave 0.59 worst. What fixed it, roughly in order of payoff:
  1. **The released off-hand rests and arcs in chest space, not root space.** Once the chest turns 50°, a root-space rest spot is 0.3 out of reach and the arc points the wrong way. The UpperTorso-space arc (−0.9, 0.2, −0.5) cleared the release and the regrip.
  2. **Key the rig controls.** Shoulder detachment and elbow pole directions are keyed per frame and picked by a 1.3-second Viterbi scan (64 combos × 37 frames). A change cost stops the controls popping. The right elbow pole 'out and up' (1, 0.5, 0) won almost every frame.
  3. **Fix the path, not the pose.** Two frames still clipped between good keys:
     - At f9 the Catmull-Rom hilt path bulged 0.1 back toward the chest between the load keys.
     - At f3 the guard→load path ran close to the body.
     In-between keys with the hilt 0.2–0.3 further out fixed both. Ease them as in/out pairs (into the in-between `inn`, out of it `out`), so an in-between never becomes a stop.
  4. The guard hilt moved out 1.9 (from 1.75), so the left hand clears the chest.
- **Result:** worst limb contact **0.228** (Train2: 0.23). Blade turn ≤49.8°/frame, strike 168 studs/s on frame 15, hand and foot IK miss 0.000. Every check on the renders came back clean.
  - Files: `r15/katana_m1_blender_beats.jpg` (5 beats × game/side/front cameras), `r15/katana_m1_blender_closeup.jpg`, `r15/katana_m1_blender.gif` (half speed, game and side views).
- **Blender vs Studio:** the clipping scores are equal. Blender pays for itself in iteration and inspection:
  - A fresh evaluation is about 1 ms, so a full scan takes 1–3 s, against minutes over Studio MCP.
  - Real IK constraints and keyable rig controls.
  - Renders from any camera, without `screen_capture` timeouts.
  - The cost is a transfer step back to Studio: per-frame joint transforms through a Luau builder, since Studio can't reach localhost.

**Drill 7: running attack, a one-handed lunging dash cut (Blender, 2026-11-09)**
- **Brief:** "do a running attack… use the katana running animation as the starting point of running".
  - It fits `Config.RunningHeavy`'s contract: 0.87 s (26 frames), blade across the front on frame 10 (0.33 s), the 46 studs/s lunge on a gliding stance.
  - It's a different move from the game's spin slash: a right-to-left horizontal cut.
  - Keys and tooling: `RUN_KEYS` and `RUN_CFG` in `r15/r15_blender.py`. Files: `r15/r15_katana_run_attack.blend`, `r15/katana_run_attack_beats.jpg`, `r15/katana_run_attack.gif`.
- **Five beats:**
  1. **Idle (f0):** the game's Katana Run, keyframe 11 of 41 (t = 0.167 s): left foot mid-stance, right foot swinging through. The right foot plants next, which is the lunge step.
  2. **Anticipation (f4–6):** step into the lunge, chest winds 55° right, blade cocked over the right shoulder, off-hand guiding forward.
  3. **Action (f8–10):** the whip accelerates through an in-between key, and the off-hand pulls back to the hip to power the cut.
  4. **Follow-through (f13–18):** the blade carries to the left-back and the torso keeps turning to 45°. The off-hand counter-balances out to the left.
  5. **Recovery (f26):** rise into a low-forward ready carry, and the game blends back into the run.
- **Starting from an existing clip:** read its poses in Studio (read-only) and FK them on `R15Rig`. Then build the first key from them:
  - the exact hand, feet and torso;
  - full foot and off-hand matrices, so foot pitch survives;
  - the clip's own per-side joint translations and elbow/knee directions as the starting rig controls (`pin_start`), blended into the planned controls by frame 6.
  - Frame 0 matches the run within 0.003 studs, except the upper arms (0.4). Hinge-locked IK twists a block upper arm differently from the clip, because its pivot sits on an inner corner. The 0.1 s crossfade hides it.
- **The game's Katana Run clips on every frame:** thighs into the 42°-leaning chest, 0.05–0.64, and 0.32 at the frame used. It also uses large non-mirrored joint translations (left hip socket 1.17 studs, shoulders 0.49).
  - So the attack inherits 0.47 on frame 0, and the hips blend out of the run's detachment by frame 1, faster than the arms.
- **One-handed tooling:**
  - **Chest-space hilt keys (`hiltUT`), interpolated in chest space.** Interpolating them in root space dragged the hand straight through the chest while the torso unwound (1.05 contact). In chest space the hand rides the twist.
  - **Keyed off-hand positions (`lfree`, chest space).**
  - **Per-key foot lift.** A planted foot gliding with the lunge mustn't hop.
- **Fixes, in order of payoff:**
  - **Don't loop the recovery back to a trailing carry.** The blade slerps the short way, through the body (0.47). End in a ready pose and let the game blend.
  - **Widen hand paths past the torso sides.** A 1-stud hand centred at x 1.3 already overlaps a 2-stud chest.
  - **Point the knees out in a deep lunge** (pole (1, 0, -0.5)). Halve the pelvis pitch, drop the hip sockets 0.25 and spread them 0.15. A forward knee otherwise rises into the LowerTorso.
  - **Keep the follow-through blade above the floor.** A one-handed blade 6.1 studs long, pointing 25° down from hip height, reaches the ground.
- **Result:**
  - Frames 3–26: worst limb contact **0.18** (better than M1 Train2's 0.23). Frames 0–2 come from the run pose: 0.47, 0.25, 0.35.
  - Blade turn ≤56.6°/frame, tip about 210 studs/s at contact, tip at least 0.51 above the floor, hand/foot IK miss ≤0.08.

**Lessons:**
- **For a two-handed weapon, bake from the weapon every frame.** Interpolating joint rotations between sparse keys slides the hands off the hilt. With per-frame IK the grip holds exactly, but the clip editor shows 37 keys, so edit the weapon keys and re-bake rather than hand-tweaking frames.
- **R15 arm reach is shorter than its parts add up to.** Max shoulder-to-wrist is **1.56 studs**, not the 1.64 that the two segment lengths sum to, because the shoulder joint sits 0.5 studs off the upper arm's centre. Keep the hilt about 1.2–1.5 studs from **both** shoulders.
- **Two hands on the hilt keep the shoulders square.** Turn the hips, not the chest. Twisting the chest toward the follow-through pulls the off-hand's shoulder away.
- **A blade pointing across the body is the hardest two-hand reach,** because the pommel hand ends up on the far side. Release the off-hand after the cut and regrip in recovery. This is common in action-game katanas, and it also chambers the off-hand.
- **Speed budget for a long blade:** at 60°/frame of turn, a 6.1-stud blade's tip tops out at about **190 studs/s** from rotation alone (6.1 × 1.05 rad × 30). The game's existing katana attacks peak at 200–260 studs/s, so the rest must come from hilt travel of about 0.3+ studs/frame (a step-in or lunge). This M1 has only about 0.4 studs/frame of hilt travel and no step, so at 171 studs/s it sits at the light end.
- **An in-between that eases to a stop mid-recovery makes a hitch** (speed 1.8 → 0.75 → 0.49 → 1.2 studs/frame). Pair "in" then "out" eases, or drop the key.
- **Large auto-fit offsets mean the key is wrong.** Offsets over about 6° jitter 13–25° per frame. Move the key instead.
- **Off-axis screenshots lie:** perspective made a blade pointing straight at the camera look sideways. Check the HumanoidRootPart-space tip position before "fixing" a pose.
- **Keep tuned keys in a file, not only in `_G`:** one `screen_capture` timed out and Studio briefly dropped off MCP.

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
- **Interpolating joint rotations between sparse keys for a two-handed weapon:** the hands slide off the hilt. Bake per frame from the weapon.
- **Twisting the chest into a two-handed follow-through:** the off-hand over-reaches. Turn the hips and keep the shoulders square.
- **Pointing a 6-stud blade 45° down at hip height:** the tip goes through the floor.
- **Judging a pose from an off-axis screenshot:** check the tip position in HumanoidRootPart space.
- **Shipping a pose without a clipping check:** block R15 limbs are 1 stud thick, and a hilt 1 stud in front of the chest buries both forearms.
- **Keeping the arms attached for a two-hand grip on block R15:** no placement is clip-free. Detach the shoulders forward 0.3–0.5.
- **Moving the released hand in a straight line to the hip:** it passes through the torso. Arc it outward.
- **Leaving a Blender IK chain dead straight at rest:** it never bends. Seed a small hinge bend in the pose.
- **Putting the released off-hand's rest spot in root space:** once the chest twists it's out of reach. Make it chest-relative.
- **Interpolating chest-relative hand keys in root space:** while the torso twists, the hand cuts a straight line through the chest. Interpolate in chest space.
- **Looping a recovery back to the start pose when the blade must turn about 180°:** the slerp takes the short way, through the body. End in a ready pose and let the game blend.
- **Assuming an existing clip is clean:** measure it. The game's Katana Run clips 0.05–0.64 on every frame.

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
- [ ] Two-handed weapon: baked per frame with IK. On the baked clip, grip gap = 0, the planted foot's drift = 0, no over-reach, and the torso auto-fit is ≤6°.
- [ ] Long blade: tip speed budget = blade length × 60°/frame of turn; anything above that comes from hilt travel (step-in).
- [ ] Clipping check on every frame (oriented-box overlap of unjoined parts plus the blade): worst contact ≤0.25 studs, and only at the grip.
- [ ] Two-hand grip on block R15: shoulders detached forward, hands 1.0 apart, hilt 1.6–2.0 studs out, elbows out. Hip sockets dropped 0.2.

## Next drills

- Play `KatanaM1_Train1` from the clip editor and judge it by eye. Then make a step-in version (front foot steps about 0.6 studs on the strike) for 200+ studs/s, and fire hitstop and the layered slash from its `Hit` marker.
- Judge the hitstop videos (0/3/6 frames) by eye, then in Studio with `AdjustSpeed(0)` on both tracks.
- Export the five-beat swing and the hit reaction to Roblox through the Luau pose-builder route. Fire the layered slash and the impact burst from a `Hit` marker on frame 20, the measured contact frame.
- Make a separate dust sheet for the burst; judge the layered slash in Studio at game distance.
