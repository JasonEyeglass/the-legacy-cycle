---
tags: [legacy-cycle, vfx, animation, research]
updated: 2026-11-09
---

# Roblox VFX Techniques

What an AI learned from watching top Roblox VFX and animation tutorials, and the repos that let it watch. Back to [[The Legacy Cycle]]. Related: [[Shrine VFX]], [[Studio Gotchas]].

Studied on 2026-11-09 from 13 high-view YouTube tutorials. I pulled their captions with `youtube-transcript-api` and sampled keyframes from three of them with `yt-dlp` and `ffmpeg`. Every technique below is summarized in my own words, with its source video cited. Labels:
- **Tutorial:** what the video does or claims.
- **Checked:** I verified it with maths or the Roblox docs.
- **Unverified:** a claim to test before relying on it.

## 1. How an AI "watches" a tutorial (the repos)

Star counts come from the GitHub API on 2026-11-09. They are ranked by fit for learning a *visual* skill, then by stars and activity.

| # | Repo | Stars | What it does | How it gets the content |
|---|---|---:|---|---|
| 1 | [bradautomates/claude-video](https://github.com/bradautomates/claude-video) | 20.3k | `/watch` skill/plugin for Claude Code (and Codex, other skill hosts): URL or file → timestamped frames + transcript handed to the agent | yt-dlp download, frames, **native captions first**, optional local WhisperX or Groq/OpenAI ASR |
| 2 | [sonpiaz/watch-cli](https://github.com/sonpiaz/watch-cli) | 260 | One `watch <url>` command plus an MCP server, and a prompt library (one prompt turns a tutorial into a cheat sheet) | yt-dlp, then ffmpeg for evenly spaced frames, then ASR. The default ASR backend is a paid API, about $0.05 per hour of video |
| 3 | [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) | 206k | Downloader: search (`ytsearchN:`), view counts, captions, **section-only downloads** | Video, audio, subtitles |
| 4 | [jdepoix/youtube-transcript-api](https://github.com/jdepoix/youtube-transcript-api) | 8.6k | Python API for a video's captions, no API key | Captions only |
| 5 | [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper) / [openai/whisper](https://github.com/openai/whisper) | 26.6k / 112k | Local speech-to-text for videos without captions | Audio |
| 6 | [Breakthrough/PySceneDetect](https://github.com/Breakthrough/PySceneDetect) | 5.4k | Finds scene cuts so you sample frames where the picture changes, not every N seconds | Frames |
| 7 | [danielmiessler/Fabric](https://github.com/danielmiessler/Fabric) | 44.6k | Prompt "patterns". Its `yt` helper pipes a transcript into a pattern such as `extract_wisdom` | Captions only |
| 8 | [HKUDS/VideoRAG](https://github.com/HKUDS/VideoRAG) | 3.5k | "Chat with your videos" over very long video, with the Vimo desktop app | Frames, captioned by a vision-language model, plus ASR, indexed for retrieval. Needs a GPU |
| 9 | [QwenLM/Qwen3-VL](https://github.com/QwenLM/Qwen3-VL) | 20.2k | Open vision-language model that takes video input directly | Frames (native video). Needs a GPU |
| 10 | [Leon1207/Video-RAG-master](https://github.com/Leon1207/Video-RAG-master) | 465 | NeurIPS 2025 research: training-free retrieval that feeds query-relevant text to a vision-language model | OCR, ASR and detected objects from frames |
| 11 | [anaisbetts/mcp-youtube](https://github.com/anaisbetts/mcp-youtube) | 552 | MCP server: "summarize this video" | Subtitles through yt-dlp |
| 12 | [martinopiaggi/summarize](https://github.com/martinopiaggi/summarize) | 233 | Multi-source summarizer (YouTube, X, TikTok…) with an optional vision mode | Transcript, optional frames |

Too small or too stale to recommend: [YueFan1014/VideoAgent](https://github.com/YueFan1014/VideoAgent) (337 stars, last push 2024-12), [chrisx599/Video-Browser](https://github.com/chrisx599/Video-Browser) (29), [ZeroPointRepo/youtube-mcp](https://github.com/ZeroPointRepo/youtube-mcp) (13, freemium) and [khou/watch-video](https://github.com/khou/watch-video) (7).

**Best pipeline for learning a visual skill:** search → captions → read them for *when* and *why* → pull a short clip only around the key moments → sample frames → a vision model reads the frames for *what is shown*. #1 does this end to end. The minimal Windows build, which is what this note was made with:

```bash
py -3 -m pip install --user yt-dlp youtube-transcript-api
py -3 -m yt_dlp --flat-playlist --print "%(view_count)s|%(id)s|%(channel)s|%(title)s" "ytsearch12:roblox vfx tutorial"
py -3 -m yt_dlp -f "bv*[height<=720][ext=mp4]" --download-sections "*5:00-6:40" -o clip.mp4 <url>
ffmpeg -i clip.mp4 -vf "fps=1/10,scale=960:-1,tile=3x3" -frames:v 1 sheet.jpg
```

ffmpeg was already installed. Captions come from `YouTubeTranscriptApi().fetch(video_id)`. For the agent route, install #1 as a Claude Code plugin instead.

**What it can't do (seen in this study):**
- **Captions miss everything set in popups.** One slash tutorial (Ampro) says outright that its curve popups didn't record, and its size and transparency graphs appear only on screen. Frames recovered one of them: an ease-out size curve.
- **Frames at 360p can't read the Properties panel.** The contact sheets showed the shape of each effect but not one property value. Use 720p or higher and crop to the panel when the numbers matter.
- **Fixed-interval frames miss 0.05 s flashes.** Impact flashes last 3 frames. Step through the hit moment frame by frame, or use scene detection (#6).
- **Tutors mumble numbers** ("0.01" vs "point one", "-190 -180"). Treat spoken values as starting points and confirm them in Studio.
- **Nothing here judges taste.** The models describe a frame, but whether the effect *reads* at game distance still takes a playtest.

## 2. Particle fundamentals

- **Two-layer fire:** inner and outer attachments.
  - Inner: a 4x4 flipbook set to OneShot, LightEmission ≈0.8, Brightness 5, Lifetime 0.5, Rate 2, Speed 0.001, a bright-to-dark orange gradient.
  - Outer: a duplicate of the inner with a darker colour and lower emission, a random lifetime, a higher rate, a bigger size, and transparency fading to 1 at the end.

  (Tutorial: Stoic, TrendyV2 #1)
- **`Speed` can't be 0 for perpendicular particles.** `VelocityPerpendicular` and `VelocityParallel` take their facing from the velocity, so use 0.001 to 0.01. (Tutorial: Twist, Ampro. Checked: the orientation follows the velocity vector.)
- **Brightness only does something once `LightInfluence` is below 1.** Set LightInfluence to 0 for VFX. (Tutorial: TrendyV2 #2)
- **With LightEmission 1, use Brightness as your opacity knob.** It's quicker than editing the transparency curve. (Tutorial: Twist aura)
- **Off-centre texture + random Rotation and RotSpeed** makes specks and fire wobble around a pivot instead of spinning in place. This is cheap organic motion. (Tutorial: Ampro, Twist aura)
- **Squash** turns round dots into ovals and stretches thick textures. It doesn't work on diagonal textures. (Tutorial: Ampro, Twist aura)
- **Negative LightEmission gives a dark outline look.** (Tutorial: Develuper. Unverified: test it in Studio first.)
- **Use white or greyscale textures and tint them in Roblox.** (Tutorial: BlenderBuilder)

### Flipbooks (Tutorial: Dogmath, "flipbooks in under 3 minutes")
- **Layouts:** 2x2 gives 4 frames of 512², 4x4 gives 16 of 256², and 8x8 gives 64 of 128², all on a 1024² sheet. Frames read left to right, then top to bottom.
- **Loop:** plays at `FlipbookFramerate` until the particle's lifetime ends. Use it for steady fire.
- **OneShot:** stretches the whole sheet over the lifetime (16 frames over 1 s ≈ 16 fps). Use it for explosions, slashes and smoke puffs. Most tutorials default to OneShot.
- **PingPong:** like Loop, but plays forward then back.
- **Random:** jumps between frames at random. Use it for lightning flicker.
- **`FlipbookStartRandom`:** starts each particle on a random frame. Use it to break up identical copies.

### Curves (seen on screen)
- **Size:** an ease-out, which grows fast in the first ~20% of the life and then levels off. The tutor drew it from 0 to its maximum (Ampro frame, 5:00–6:40). Twist aura also says "a bit less than linear, so it grows more at the start."
- **Transparency:**
  - Add an extra key so the particle *holds* at 0 and then fades. Dragging only the end point makes it start fading at once. (Tutorial: Twist slash)
  - For winds and crescents, start slightly transparent so they appear at the slash radius instead of visibly expanding. (Tutorial: Ampro)
  - The impact flash is the exception: no fade-in, it pops. (Tutorial: Twist)

## 3. Beams and trails

- **Trails** need both `Attachment0` and `Attachment1`, or nothing renders. For a sword trail, put the attachments at the base and tip of a thin part and use Lifetime ≈0.5. (Tutorial: TrendyV2 #2)
- **Layered energy beam:**
  - Beam A: LightEmission 1, TextureSpeed 4, then tune TextureLength. Use a texture that scrolls along the beam.
  - Beam B: a duplicate of A, wider and more transparent, which reads as glow.
  - At the source: a 4x4 OneShot flipbook particle (rate 1, speed 0.01).
  - Side sparks: spread 35–45°, speed 20, lifetime 0.5–0.7, Rotation −360..360, acceleration ≈−200 on the axis opposite the beam, and transparency starting at 1 so they fade in.
  - A PointLight in the beam colour. Darker colours need more Brightness.

  (Tutorial: Jam Pro)
- **Perfect semicircle beam:** put the two attachments one diameter *d* apart and face them outward. Set `CurveSize0` and `CurveSize1` to **(2/3)·d** (Twist used d = 19 → 12.67). (Checked: a cubic Bézier half-circle needs handles of 4/3·r = 2/3·d.) Rotate 90° so it lies flat, and duplicate it for a full ring.
- **FaceCamera on** for slash beams, so they don't go flat when seen from above. Raise **Segments** only when the beam actually curves. (Tutorial: Twist)
- **Beams draw correctly from one side only.** You can see the tail through the head from behind. It doesn't matter on fast slashes. (Tutorial: Twist)
- **Fade a beam by tweening Width0 and Width1 to 0.** Store the base widths as attributes, start at base × a multiplier (the beam shrinks immediately, so start bigger), then tween to 0. (Tutorial: Twist)

## 4. Slash recipe (Twist VFX 58 min + Ampro 18 min)

1. **Main crescent:**
   - Particle settings: Lifetime 0.2–0.4, `VelocityPerpendicular`, Speed 0.01, LightInfluence 0, EmitCount 1, an initial Rotation that points the crescent along the swing, and RotSpeed **above 360**. RotSpeed is in degrees per second, so a 0.3 s life needs a big number.
   - Stack 3–6 slash textures:
     - a **dark under-layer** (dark green, not black) for contrast;
     - the main colour;
     - a small, very bright highlight;
     - a near-transparent noisy layer for "glow".
   - `ZOffset` steps of 0.001 are enough to order the layers.
2. **Slash winds:** OneShot crescents that fade in, RotSpeed ≈500 with variation, Lifetime 0.4–1.2. Use two copies in different colours for layering.
3. **Beam layer** (optional): several curved beams under **one** attachment, so a single tween rotates them all. Tween the attachment's CFrame by **175°, not 180°**, because a 180° CFrame tween can go round the wrong way. Its duration must be shorter than the width fade's.
4. **Hit layer:**
   - Dots: squashed, FacingCamera, Speed 50, Drag 3, Lifetime 1–1.2, shape emitting in and out.
   - Shards: `VelocityParallel`, Rotation 90 and Spread 0, so they point along their travel, and LightEmission 0 to keep their colour.
   - One **big shard flash** with a very short lifetime, which reads as the slash breaking into shards.
5. **Impact flash:** a star texture, Lifetime ≈**0.05**, growing and fading. Add a second copy that shrinks instead, and a squashed one with a random rotation. Rule: *if you can make out its shape, it lasts too long.*
6. **Ground layer:**
   - A flat shockwave with a **Linear** size curve, emitted once.
   - Wind particles with LightEmission on (it hides the black lines in wind textures), Spread ≈25, rotated in opposite directions.
   - Faint stylized smoke emitted from a disc.
   - Neon ground-wind meshes tweened from size 0 to their final size, ending at Transparency 0.97. Neon needs a very high transparency to fade.
7. **Debris:** emit from the side as a ring, Speed 100, Drag 7, small sizes that shrink, Lifetime 0.5–1, and few of them. Find a reference artist before guessing.
8. **Character flash:** a `Highlight`, FillTransparency tweened to 0.6 and back (Circular easing, reverses on), outline off.
9. **Trigger** every burst on the animation's first swing frame.

```lua
local TweenService = game:GetService("TweenService")

-- Fire every emitter in an effect model once; per-emitter count lives in an EmitCount attribute.
local function burst(model: Instance)
	for _, d in model:GetDescendants() do
		if d:IsA("ParticleEmitter") then
			d:Emit(d:GetAttribute("EmitCount") or 1)
		end
	end
end

-- Shrink every beam from its stored width (times a head-start multiplier) to zero.
local function fadeBeams(model: Instance, duration: number, mult: number)
	local info = TweenInfo.new(duration, Enum.EasingStyle.Linear)
	for _, d in model:GetDescendants() do
		if d:IsA("Beam") then
			d.Width0 = (d:GetAttribute("Width0") or d.Width0) * mult
			d.Width1 = (d:GetAttribute("Width1") or d.Width1) * mult
			TweenService:Create(d, info, { Width0 = 0, Width1 = 0 }):Play()
		end
	end
end

return { burst = burst, fadeBeams = fadeBeams }
```

## 5. Aura and transformation (Twist aura 42 min, Develuper)

- **Build the effects on a rig template**, a rig that holds only attachments and emitters, organized in folders. On spawn, a script copies each part's attachments, emitters, beams and sounds onto the matching part of the character. (Tutorial: Develuper)
- **Ground effects go on a separate invisible part** that follows the root position. They aren't parented to the character, so they don't tilt or go through the floor. Keep one per character to avoid duplicates. (Tutorial: Develuper)
- **Looped auras use `Enabled`. Bursts use `:Emit()`.** For sound, play one burst on transform and then a looped bed while the aura is on.
- **Use the "zero envelope" in size graphs** to randomize thickness. Use stars more than lens-flare textures for flares. Add a ground glow decal and a slow ripple, both at low brightness. (Tutorial: Twist aura)
- **Where effects run:** Develuper says to run impacts on the server "so everyone sees them". The better pattern, and what The Legacy Cycle does, is: **the server validates, then tells all clients, and each client renders.** See the LihFolk vault note "Replicating VFX - Server Validates, Clients Render".
- **Fire VFX and SFX from an animation event** on the exact frame (`GetMarkerReachedSignal`). For a projectile, raycast from the arm on the server to get the real impact point, then spawn the impact effect there. (Tutorial: Develuper)
- **Upper-body attacks:** delete the leg keyframes, and Roblox blends the attack with the walk cycle. (Tutorial: Develuper)

## 6. Combat animation

**The 5 beats:** Idle, then Anticipation, then Strike, then Follow-through, then Recovery. distovi teaches exactly these, with a souls-like knight clip as reference (frames 0:00–0:40).

1. **Pick key poses first.** For each beat, take the most *exaggerated* frame, for example the furthest reach-back for Anticipation. Build those poses, then fill in between them. (distovi)
2. **Hold the anticipation.**
   - Copy the anticipation pose a few frames later. Just before the strike, rotate the **torso** a little further toward the strike. That creates the slow-in. (distovi)
   - Make the wind-up **slower** than the strike and add a brief pause before the release. (MonkeyDev punch)
3. **Follow-through:** copy the end-of-strike pose and push it further along the swing. (distovi)
4. **Keep the weapon's arc clean.** A straight thrust stays straight, with no dip and rise. (distovi)
5. **Overlap:** the arm ends a few frames *after* the torso. That lag sells the weapon's weight. (distovi)
6. **Animate the torso first, and the whole body moves.** A throw is not just an arm. Exaggerate past what looks real. (Nisky)
7. **Retime by dragging keys.** Make the strike 2–4 frames and keep the load longer. (MonkeyDev)
8. **Blender gimbal flips:** use Pose ▸ Flip Quats (Alt+F). (distovi)

**Moon Animator notes** (Nisky):
- R toggles move and rotate. Double-click a part to key the whole rig. Ctrl+R mirrors a pose.
- The timeline runs at 60 fps. 24 fps gives a choppier, anime look.
- B toggles onion skin, which helps with jump arcs.
- Camera FOV: 50 is the default "lens", and 50–80 is the usual range.
- M on a middle key smooths the path. 7 opens easing.

## 7. Cinematic camera (Ludius "Cutscenes Like a Pro")

- **Camera rig model:**
  - `HumanoidRootPart`, 1×1×1, set as the PrimaryPart.
  - `Cam`, 1×1×1, with a SurfaceGui to show which face is the front.
  - A `Motor6D` from HumanoidRootPart to Cam.
  - An `AnimationController` with an `Animator`.

  Animate the Cam part in Moon Animator together with the character.
- **At play time:**
  1. Clone the rig and `PivotTo` it onto the player's root CFrame.
  2. Weld the two roots.
  3. Set the camera to `Scriptable`.
  4. On `RenderStepped`, set `camera.CFrame = cam.CFrame`.
  5. Play both tracks with fade time **0**, so the camera snaps.
  6. Wait for the track to finish, then restore the camera type, WalkSpeed, JumpPower and AutoRotate.
- **Preload the animations.** Without that, the first play shows nothing. (`ContentProvider:PreloadAsync`)

## Worth applying to The Legacy Cycle first

1. **Ember cinematic, Action beat:**
   - Make the hit flash an impact star with Lifetime ≈0.05, one growing copy and one shrinking copy. Pair it with a `Highlight` pulse on the hero (FillTransparency to 0.6 and back, Circular, reverses on).
   - Fire them from the same clock tick as the FOV punch and the grade.
   - Today the flash is a grade and FOV change. A shape-less 3-frame burst on top of it will read as *impact*.
2. **Ring of fire and pillar:**
   - Add pairs of curved beams over the neon ring parts, with CurveSize = ⅔ of the attachment gap. Use two layers: a core, and a wider, more transparent glow.
   - Scroll them with TextureSpeed. Fade them by tweening Width0 and Width1 to 0, not by fading the part.
   - Rotate the beam group by 175° per spin.
   - The texture must be the user's own upload, made with PhaseVFX or the atlas pipeline. Never guess an id.
3. **Dash shockwave** (`CombatClient.Locomotion`):
   - Make the shockwave a flat, emit-once ring with a **Linear** size curve.
   - Add two counter-rotated ground-wind particles, and a small sideways debris ring (Speed 100, Drag 7, shrinking).
   - Put a dark under-layer beneath the bright ring. Dark fantasy needs that contrast more than extra brightness.
4. **Curve discipline on the motes and aura:**
   - Size eases out. Transparency holds and then fades. Winds fade in.
   - Use off-centre ember textures with a random Rotation and RotSpeed for flicker.
   - Set LightInfluence to 0 everywhere.
5. **Combat animations** (5 beats):
   - Hold the anticipation, with the torso turning a few degrees further toward the strike.
   - Strike in 2–4 frames. The arm settles 2–3 frames after the torso.
   - Keep the weapon arc clean. Upper-body attacks carry no leg keys, so they blend with the walk.
   - Fire `Feedback.shake`, the hit VFX and the SFX from the same `Hit` marker.

## Common mistakes

- **Speed 0 on a perpendicular or parallel particle:** it vanishes or faces the wrong way. Use 0.01.
- **Bursts left on `Enabled`:** use `:Emit()` from an animation marker instead. A looping emitter drifts out of sync with the swing.
- **Single-texture slashes:** they look flat. Layer a dark under-layer, the main colour, a bright highlight and a faint glow.
- **Impact flashes that last long enough to read:** cut them to about 0.05 s.
- **Tweening a rotation by exactly 180°:** the CFrame can take the short way round. Use 175°.
- **Dragging only the end of the transparency curve:** the particle starts fading at birth. Add a hold key.
- **Running VFX on the server "so everyone sees it":** the server should validate and broadcast, and each client renders.
- **Playing an animation before it's preloaded:** the first cutscene play does nothing.
- **Trusting spoken numbers or 360p frames for exact values:** confirm them in Studio, or read a 720p crop.

## Checklist

- [ ] Every burst is fired with `Emit` from a marker. Loops are only for auras.
- [ ] Each layer has a job: dark contrast, main colour, highlight, glow, debris, flash.
- [ ] Size curves ease out. Transparency holds, then fades. Winds fade in. Flashes pop.
- [ ] Beam arcs use CurveSize = 2/3 of the attachment gap. Beams fade by tweening their width.
- [ ] Animation: 5 beats from exaggerated key poses, a held anticipation with torso slow-in, a clean arc, the arm lagging the torso.
- [ ] Camera: rig animated alongside the character, Scriptable each frame, tracks preloaded.
- [ ] Texture and asset ids are your own uploads. Never guess them.

## Sources (videos studied)

| Video | Channel | Views (2026-11-09) | Used for |
|---|---|---:|---|
| [ROBLOX VFX Guide #1 - Particles](https://www.youtube.com/watch?v=xzZeP65SSlA) | TrendyV2 | 378k | Particle basics, two-layer fire |
| [ROBLOX VFX Guide #2 - Trails & Beams](https://www.youtube.com/watch?v=rIXyQ4RAwws) | TrendyV2 | 56k | Trails, beams, LightInfluence and Brightness |
| [How to Make Slash VFX \| Roblox Studio](https://www.youtube.com/watch?v=zlKdwujvP2A) | Twist VFX | 267k | Full slash, beam ring maths, width tweens, impact, debris, Highlight (frames 10:55–12:15) |
| [How to Learn Roblox Aura VFX in 1 Hour](https://www.youtube.com/watch?v=u2o-wHKDlpw) | Twist VFX | 97k | Aura layering, envelope, off-centre rotation |
| [Roblox VFX Tutorial Part 2](https://www.youtube.com/watch?v=JXoGT2PoOhs) | Ampro | 186k | 8-texture slash and hit, shards, flash (frames 5:00–6:40) |
| [How to ACTUALLY Use VFX in Roblox](https://www.youtube.com/watch?v=TkNZETIu0CM) | Develuper | 233k | Emit on animation events, rig-template auras |
| [Roblox VFX Beams Tutorial](https://www.youtube.com/watch?v=exWx1kYy0P0) | Jam Pro | 180k | Layered energy beam |
| [roblox flipbooks (sprite sheets) tutorial in under 3 minutes](https://www.youtube.com/watch?v=QKuk_Mau09I) | Dogmath | 101k | Flipbook layouts and modes |
| [HOW TO Make Anime Particles In Roblox Studio!](https://www.youtube.com/watch?v=1_5dpzFk3L8) | BlenderBuilder | 382k | Particle property tour |
| [How to make an Attack Animation in 4 Minutes](https://www.youtube.com/watch?v=bzS9B3-iVH0) | distovi | 136k | 5-beat key poses, overlap, arcs (whole video, framed) |
| [How to make PUNCH animations in ROBLOX STUDIO!](https://www.youtube.com/watch?v=WZyvOpIiaYE) | MonkeyDev | 139k | Slow load, pause, fast strike |
| [Roblox ANIMATION Guide #1 - Moon Animator (2026)](https://www.youtube.com/watch?v=267aFypaeWU) | Nisky | 283k | Torso first, exaggeration, FOV, easing |
| [How To Make Cutscenes Like A Pro \| Roblox Studio](https://www.youtube.com/watch?v=6D9T4iaQHAc) | Ludius | 204k | Camera-rig cutscenes |
