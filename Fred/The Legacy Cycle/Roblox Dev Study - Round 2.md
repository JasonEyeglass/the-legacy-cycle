---
tags: [legacy-cycle, research, youtube, game-design, animation, vfx, game-feel, hitstop]
updated: 2026-11-09
---

# Roblox Dev Study - Round 2

The second YouTube study round (2026-11-09): 14 videos on growing a Roblox game, combat feel and hit-stop, animation craft, and mesh/beam VFX. Transcripts were read through the YouTube page because the transcript API was IP-blocked. Everything below is my own summary, not transcript text.
- Back to [[The Legacy Cycle]]. Round 1 is [[Roblox VFX Techniques]]; the measured drills are in [[VFX and Animation Training Log]].
- The same four notes live in the LihFolk vault under `10 Game Design and Genres/`, `47 Game Feel and Juice/`, `08 Animation/03 Authoring/` and `07 Lighting VFX and Audio/`, all titled "... - Studied from YouTube".

## Worth applying to The Legacy Cycle first

1. **Finish the hit-stop in `CombatClient.Feedback`, using Sakurai's stack.** Drill 5 left the freeze without a shake.
   - Stop length = base × damage × a per-move coefficient (katana sweet spot or heavy finisher above 1).
   - Shake the **victim's visual root (`RootJoint.C0`) more and the attacker's less**, never `HumanoidRootPart`. Sideways only when grounded, decaying across the stop, scaled by camera distance.
   - The victim's reaction track fades in over about 4 frames (about 0.13 s) during the stop, instead of freezing on a pose.
   - The attacker's track runs at about 0.05 speed instead of 0, so the blade creeps.
   - These are Unverified on Roblox until tested in Studio.
2. **Hit confirmation stack for phone readability:**
   - a 2–3-frame white `Highlight` flash on the victim, a short knockback and a crunchy hit SFX on the contact frame;
   - camera shake behind the existing Camera-shake setting.
   - Drill 5 measured that the attacker's own body hides most of the target from the chase camera, so these signals carry the read.
3. **Shard-stack meshes for ember effects.** The Mask + Smooth + Simple Deform stack makes ember wraps for the Shrine absorb cutscene and spiral katana slashes. Use a dark-ember core with bright shards (contrast), and keep triangle counts mobile-safe.
4. **First minute check on the [[Join Flow]].** Several videos tie first-play bounce to anything before the fun: menus, loading pans, customizers. Test a path where a new player gets control within seconds (a preset hero, forge later) and compare bounce. This is a suggestion to measure, not a settled decision.
5. **Retention that fits a Mild rating:**
   - lean on GMTK's pacing, mystery (locked shrine doors you see before you can open them), long-term ember progression with short milestones, and a social flex;
   - avoid FOMO pressure aimed at young players.

## Making Roblox Games That Grow - Studied from YouTube

What seven popular videos agree on about making a Roblox game that finds players and keeps them. The notes are my own summaries (2026-11-09), not transcripts. Each claim is tagged with the video it came from. Creator numbers and algorithm details are **Unverified** creator claims: check them against Roblox's own creator docs before relying on them. Related: *Core Loops and Session Design* (LihFolk), *Retention and Return Hooks* (LihFolk), *Discovery and the Algorithm* (LihFolk), *First-Time User Experience and Onboarding* (LihFolk), *Thumbnails Icons and Store Page* (LihFolk), *Launch Strategy - The First Two Weeks* (LihFolk), *Update Cadence and Hype Cycles* (LihFolk).

### 1. The idea carries most of the weight

- **Explain it in one sentence.** The name or icon should tell a stranger what you do; "Hide or Die" does, "Prop Hunt" doesn't. If it takes more than about 10 seconds to get the game, players leave. *(AlvinBlox: Blow Up, Mistakes)*
- **Steal the reason to play, not the game.** Copy the proven core loop and change the theme or twist about 10–20% of it. Lineages: Doors←Rooms, Tower Defense Simulator←Tower Battles, Fisch←Fishing Simulator, Jailbreak←Redwood Prison, the "Steal a ___" wave.
  - A pure clone dies. A fully original idea usually dies too.
  - Ask two questions: *what's the proven reason to play?* and *what's uniquely mine?* If you can't answer both, change course. *(rasdev; AlvinBlox: Popular)*
- **Mass appeal over niche.** The audience includes young kids, adults returning out of nostalgia, non-English speakers, phone players with 5 minutes, and parents playing with their kids. Instantly relatable beats niche: Grow a Garden isn't for gardeners. *(AlvinBlox: Blow Up)*
- **Execution means making it consumable.** One simple mechanic with lots of depth beats many systems:
  - Build a Base and Steal: build, get robbed, rebuild a better layout.
  - The sandwich game: one RNG roll that raises "what's next? how tall?"
  - "Simple scales, fancy fails." Big games called "slop" are often simple-mechanic genius. *(Tizzy)*
- **A strong core fantasy:** a tiny character in a giant kitchen, running an airport. *(Tizzy)*

### 2. The first minute decides everything

- **The first thing a player does should be the fun thing.** Avoid a loading menu, an up-front tutorial or character customization before play. A game that *needs* a tutorial is a design red flag. Teach in context. *(rasdev; AlvinBlox: Blow Up)*
- **Readable without words:** could you play it if the UI were in Chinese? Guide with big buttons, arrows, animation and sound. *(AlvinBlox: Blow Up)*
- **Every input gets an immediate effect.** Clear cause and effect: press the button, see the result. Fun shouldn't be gated behind 10 minutes of grind. Slap Battles: spawn, slap, laugh. *(AlvinBlox; Tizzy)*
- **Kill friction.** Play your own game on **phone and console**. Copies of hits fail on a broken ProximityPrompt or unpolished UI. Tizzy: 30% of Build a Base and Steal's players were on console; fixing console onboarding raised D1. *(Tizzy, creator claim)*
- A short intro **cutscene** cut first-session bounce from about 22% to about 15–16% in Hack a Business. *(Tizzy, creator claim)*

### 3. Bring players back, without being evil

- **Core loop:** the repeated action must feel satisfying every time, plus a reason to return tomorrow. A game that's finished in 10 minutes (a plain obby) can't grow. Replayability can be cheap: leaderboards, daily challenges, modes, PvP. *(AlvinBlox: Popular)*
- **Proven return hooks:**
  - progress that happens while you're away (Grow a Garden crops);
  - a curiosity hook (what do I unlock tomorrow?);
  - a **social flex** (most top games have one);
  - weekly updates or events, at least every 2 weeks;
  - friends. *(rasdev; Tizzy)*
- **Engaging, not addictive** (GMTK). Prefer these to FOMO pressure:
  - **pacing** across activity types and *intensity* (calm, intense, calm, peak), because sustained max intensity exhausts;
  - steady **novelty**;
  - **mystery/foreshadowing** (show the locked door before the key), which stretches the gap between content drops;
  - **long-term goals with short milestones**;
  - **exponential growth** loops;
  - **automation/optimization** (sprinklers, Factorio);
  - **flow-tuned challenge**: failure is fine when runs are short, you visibly improve, and the next run differs.
- AlvinBlox openly recommends FOMO ("dark patterns": offline donations, rare drops, falling behind friends). **Use stakes and events, but avoid manipulative pressure on a young audience.** See *Retention and Return Hooks* (LihFolk) and the policy notes.

### 4. How discovery works (creator summaries of Roblox's docs, Unverified)

- A **spark** (ads, search, friends, TikTok) brings the first players. Roblox tests the game on a small home-page cohort, then expands to similar players if engagement holds. *(rasdev)*
- Signals named: playthrough rate (impressions to plays, called the #1 signal), first-play bounce, play days, playtime (claimed to count up to about 60 min per player per day), qualified sessions, days played with friends, spend days, Robux per user.
  - Signals are measured **per player**, so small games compete fairly.
  - The window was claimed to move from 7 to 28 days, which hurts pop-and-die games. No penalty for a slow launch. *(rasdev)*
- The three things to master: **session length, retention, monetization**. Players leaving at about 5 minutes points to a progression or fun problem. *(AlvinBlox: Popular)*
- **Ads buy players and data, not ranking.**
  - Seed with small, steady spend over 2–4 weeks rather than one big burst. AlvinBlox suggests about 10k–30k R$ total, not 100k.
  - Big ad spend on a weak game gives a spike, then zero.
  - Better games pay less per play. Ad data lags up to 48 h, so don't judge on day 1.
  - Example of a failing funnel: 2M impressions, then 6k clicks, then 1k plays, then nobody stays. That means fix the game. *(rasdev; AlvinBlox)*

### 5. Click, then community

- **The icon or thumbnail** should be simple and colourful, show the action or one idea (Rivals), or a giant character. Bright colours, a Roblox character render, and a few bold words. An "UPDATE" tag signals freshness. Claimed: 16:9 sponsored thumbnails got +40% playthrough. *(rasdev; AlvinBlox: Mistakes)*
- **Build a community before launch:** Discord or group, sneak peeks, testers to fill the first servers. Small YouTubers and streamers. Commissioned TikTok/Shorts creators are cheaper than Roblox ads for the first springboard. *(AlvinBlox: Mistakes)*
- **Make it clippable.** Watch which moments creators react to and amplify them. Theirs: hitting a thief flings them into the void with a ragdoll, which brought free creator coverage. *(Tizzy)*

### 6. Monetize so players *want* to pay

- Sell **convenience and cosmetics**, not power: speed-ups, a teleporter pass, timed boosts, spins.
- Never paywall a core verb (pay-to-sprint).
- Things a player can buy again tomorrow drive spend days.
- Make the first purchase *feel* great; it's a pivotal moment. *(AlvinBlox: Popular; rasdev)*

### 7. Process and mindset

- **MVP first:**
  - a tiny loop and a few mechanics;
  - playtest with friends, watch what they do, then refine;
  - grow 10→50→100 players.
  - Done beats perfect. AlvinBlox would rather make 10 small games a year than one risky one. *(AlvinBlox)*
- **Volume of iteration:** competitor research, then an idea, then ship, then check stats, repeat. Tizzy's team shipped about 9 tutorial versions, daily patches and weekly updates. Find what caused each spike and do more of it. *(Tizzy; rasdev)*
- **Feedback:** an in-game board, page comments, Discord. Repeated complaints are signal. Ask "what did I do wrong?"; don't blame the algorithm. *(AlvinBlox: Mistakes; waveshine)*
- **Expect flops:** waveshine's first game on a new account got 163 visits. His first hit was janky but had core retention. Players care about fun, not clean code. A community from one modest game becomes the free testers for the next. *(waveshine)*
- **Love it or leave it:** you'll play it thousands of times. Know when to quit a game that's stuck for months. Talent is the real bottleneck; templates and cheap commissions flopped for Tizzy. *(AlvinBlox; Tizzy)*

### Common mistakes

- **A pitch that needs a paragraph:** players decide from the icon and name in about a second.
- **A loading screen, tutorial or customizer before the fun:** first-play bounce is a top signal.
- **Never testing on phone or console:** friction there silently kills D1.
- **Too many systems in a first game** (battle + clan + pets + story): new players get confused and leave.
- **Throwing 100k R$ of ads at a weak loop:** a spike, then zero. Fix retention first.
- **Pay-to-win or paywalled core verbs:** players resent it, and it hurts the spend and retention signals.
- **Cloning a hit 1:1, or inventing something nobody understands:** aim for about 80% proven and about 20% yours.
- **Solo-only design:** a game that's more fun with friends spreads through invites.
- **Leaning on FOMO pressure for a young audience:** prefer pacing, novelty, mystery and goals.
- **Judging an ad campaign or update after one afternoon:** the data lags.

### Checklist

- [ ] The game is explainable in one sentence, and the icon or name shows the verb.
- [ ] A new player does the fun thing within the first 10–30 seconds, with no front-loaded tutorial or menu.
- [ ] Every core input has an instant, visible and audible effect.
- [ ] Tested on phone and console: no broken prompts, UI fits, onboarding works.
- [ ] One simple core mechanic with depth; a reason to return tomorrow (away-progress, unlocks, events).
- [ ] A social element (friends, trading, a flex) and at least one clippable moment.
- [ ] Monetization is convenience or cosmetic and re-buyable; the first purchase feels good.
- [ ] Community and testers exist before launch; ads are small and steady to seed data.
- [ ] Updates ship on a cadence (weekly, at worst biweekly), and each is checked against D1, playtime and bounce.

### Sources

| Video | Channel | Views (2026-11-09) | URL |
|---|---|---|---|
| How To Make A Popular Roblox Game | AlvinBlox | 505k | https://www.youtube.com/watch?v=Le4QhC3dDGk |
| Here's Why Roblox Games Blow Up | AlvinBlox | 288k | https://www.youtube.com/watch?v=Y93DRfB-Deo |
| Avoid These 5 Roblox Dev Mistakes | AlvinBlox | 182k | https://www.youtube.com/watch?v=f63stwpsaXw |
| Watch this before making ROBLOX Games | waveshine | 561k | https://www.youtube.com/watch?v=xnlaYLnCtFc |
| The Most Valuable Roblox Algorithm Video You'll Ever Watch | rasdev | 122k | https://www.youtube.com/watch?v=e4o-ECkzDoI |
| My Roblox Game Makes $119,300/mo (38K CCU, here's how) | Tizzy RBLX | 347k | https://www.youtube.com/watch?v=LZOohZHsJus |
| How to Keep Players Engaged (Without Being Evil) | Game Maker's Toolkit | 2.7M | https://www.youtube.com/watch?v=hbzGO_Qonu0 |

## Hit-Stop Craft and Combat Feel - Studied from YouTube

How Smash Bros. builds its hit-stop, and how a low-fidelity prototype gets good combat feel. The notes are my own summaries (2026-11-09); the Sakurai video was read from its Japanese auto-captions. For frame and millisecond budgets on Roblox devices, see *Hit-Stop and Screen Shake Tuning by Numbers* (LihFolk). Related: *Game Feel Start Here* (LihFolk), *Feedback Timing - Sound Visual and Haptic Sync* (LihFolk), *Juice on a Budget - Mobile and Low-End* (LihFolk), [[VFX and Animation Training Log]].

### 1. Sakurai's eight hit-stop techniques (Smash Bros. Ultimate)

Hit-stop freezes attacker and victim for a moment at the hit; Final Fight made it famous. Smash's version is a **freeze plus a micro-shake**:

| # | Technique | Why |
|---|---|---|
| 1 | **The victim shakes more, the attacker less** | Shows who took the hit |
| 2 | **The hurtbox never shakes.** Offset only the visual | Shaking the real position makes hits that should connect miss |
| 3 | **On the ground, shake sideways only; in the air, shake in all directions** | Vertical shake on the ground sinks the feet into the floor |
| 4 | **Amplitude decays across the stop**: big first, small last | The duration is known up front, so the decay is divided across it |
| 5 | **Size = damage × a per-move coefficient** | A sweet spot (Marth's tipper) feels far stronger than its damage alone; per-character tuning matches the source feel |
| 6 | **The victim blends into the damage pose over about 4 frames** during the stop | Neither frozen in the old pose nor snapped; keeps impact *and* smoothness |
| 7 | **The attacker creeps at a sub-frame speed** during the stop (the sword tip slides a little), then resumes as if nothing happened | Sells the blade biting in; toggled per attack, since some read better without it |
| 8 | Extras: electric hits get a longer stop; stops shrink for crowded multiplayer; **shake amplitude grows when the camera is zoomed out** | Hits read at every camera distance |

None is noticeable alone; the stack is what makes it feel good.

### 2. Anatomy of a good hit (IndividualKex, from Valheim)

1. **Anticipation:** sets the weight.
   - Slow, deliberate combat gets a long windup with good animation.
   - Fast action combat can be a single pose for about 0.1 s.
   - A smear particle sells the motion.
2. **Hit:** most of the feel.
   - **The victim's stagger/knockback is about 0.1 s.** It goes from neutral to the hit pose in about 1 frame: smooth yet instant.
   - **Freeze frame:** about 1 frame (about 20 ms) at contact, longer for heavy hits.
   - **Screen shake:** the lowest effort and highest reward, but polarizing. Always offer a toggle.
   - **Particles:** keep them simple, consistent and readable. Lavish VFX on otherwise flat combat is a red flag.
   - **Sound:** the best place to spend on quality. Great sound carries low-fidelity everything else.
3. **Follow-through/recovery:** usually the longest part. The enemy regains balance; shake, particles and sound fade out. Seconds for slow combat, tenths (often cancelable) for action combat.

**A low-fidelity prototype, upgraded one layer at a time:**
1. animation + freeze frame;
2. damage numbers;
3. crunchy SFX;
4. **white flash on the enemy for 2–3 frames**;
5. spark burst;
6. **knockback**;
7. stylized post FX;
8. screen shake.

Every layer was a visible upgrade. Slow, deliberate games can skip most of them and rely on animation weight and sound.

### 3. Mapping to Roblox (my translation, Unverified until tested in Studio)

- **Freeze:** on each client, `AnimationTrack:AdjustSpeed(0)` on both tracks for the stop, then restore. Work out the stop length from the hit's damage × a per-move multiplier.
- **Victim shake without moving the hurtbox:** on each client, shake a visual offset, e.g. the victim's root `Motor6D` (`RootJoint`/`Root`) `C0`. Never move `HumanoidRootPart.CFrame`. The server-side hit detection keeps using the unshaken root.
  - Sideways only when grounded (`Humanoid.FloorMaterial ~= Enum.Material.Air`).
  - Amplitude decaying linearly over the stop.
- **Damage pose blend:** during the stop, play the hit-reaction track with a short fade-in (about 4 frames at 30 fps, about 0.13 s) instead of snapping it in.
- **Attacker creep:** instead of speed 0, set the attacker's track to a tiny speed (e.g. 0.05) during the stop. Make it a per-move flag.
- **Camera-distance scaling:** multiply the shake amplitude by the camera distance ÷ the default zoom.
- **White flash:** a `Highlight` with FillColor white and FillTransparency 0, shown for 2–3 frames (about 0.07–0.1 s), then faded.

### Common mistakes

- **Shaking the real character position:** hitboxes drift and connecting hits miss.
- **Vertical shake on a grounded character:** feet sink into the floor.
- **A constant shake amplitude:** it buzzes instead of hitting. Decay it across the stop.
- **One hit-stop length for every move:** sweet spots and heavies need a multiplier.
- **Freezing the victim in the pre-hit pose, or snapping to the damage pose:** blend over about 4 frames.
- **Screen shake with no option to turn it off.**
- **Spending on flashy VFX while the sound is weak:** sound carries feel further.
- **Shake that's invisible when the camera is zoomed out:** scale it by camera distance.

### Checklist

- [ ] Hit-stop length = base × damage × a per-move coefficient; longer for heavies and sweet spots.
- [ ] Both rigs pause; the attacker optionally creeps; the victim blends into the hit pose over about 4 frames.
- [ ] The victim shakes more than the attacker, visual only, sideways on the ground, all directions in the air, decaying amplitude.
- [ ] The victim gets a white flash (2–3 frames), knockback, and a stagger of about 0.1 s.
- [ ] Crunchy, high-quality hit sound synced to the contact frame.
- [ ] Screen shake is toggleable and scaled by camera distance.

### Sources

| Video | Channel | Views (2026-11-09) | URL |
|---|---|---|---|
| Eight Hit Stop Techniques [Design Specifics] | Masahiro Sakurai on Creating Games | 106k | https://www.youtube.com/watch?v=tycbMSjDDLg |
| A Guide to Good Combat Feel - Game Dev Tutorial | IndividualKex | 96k | https://www.youtube.com/watch?v=FpGgpUWEb_k |

Not usable (no transcript available): SugarPunch, "A.B.I.torial: What Is Hitstop?" (s4HKw7Hqqd0).

## Animation Craft and Blender Workflow - Studied from YouTube

The craft rules and the Blender-to-Roblox workflow from two animation videos. The notes are my own summaries (2026-11-09), not transcripts. Blender menu names are from the videos (Blender 4.x era); check them in your version. Related: [[VFX and Animation Training Log]], *Combat Animation Craft* (LihFolk), *Weapon Swing Design* (LihFolk), *Timing Spacing and Frame Charts* (LihFolk), *Animation Start Here* (LihFolk).

### 1. Making a move feel real (Devgrams and Draco)

Build up a plain A-to-B arm raise in four passes:

| Pass | What it adds | How |
|---|---|---|
| 1. Pose to pose | It works, but robotically: the limb takes the shortest path | Two keys |
| 2. **Anticipation** | Readability: the move is announced | About 10 frames in, swing the opposite way first |
| 3. **Drag** | Weight: the end of the limb trails | As the arm rises, bend the forearm back so the hand lags |
| 4. **Follow-through** | Life after impact | Impact pose, then **overshoot** past it, then settle back. Nothing freezes in place |

**M1 sword swing (about 30 frames):**
1. Turn on motion paths (Calculate Motion Paths, "around frame") and clean the arcs.
2. The **torso leads**: it leans in on the windup and out on the release, then overshoots and settles.
3. The arm follows the torso, offset by a couple of frames (overlap), with drag on the way up.
4. Each hit of a combo string is its own file, chained in game.
5. Expect several passes; nobody gets the arc right first time.

**How to improve:**
- **Rotoscope** reference limb for limb: game clips (Elden Ring, Sekiro), film, a baseball-throw GIF. It teaches posing and timing. If the reference fps differs, scale the keys to match. Never post rotoscoped work as your own portfolio piece.
- Keep exercises **simple** until the basics are solid, then add complexity.
- Learn the 12 principles and physics-in-animation material.

### 2. Blender workflow for Roblox (Nisky, Advanced Guide)

Uses an R6 IK rig and the "RBX Animations" Blender add-on plus the Blender Animations Studio plugin from his beginner guide.

#### Frame rate
- **Animate at 24 fps.** 60 fps looks floaty and CG-like, and most animators use 24. Don't lower the scene fps to get a choppy look; use stepped interpolation instead (below).

#### Pose Library (biggest time-saver)
- Select all bones (with no keys selected), then **Create Pose Asset**, saved to **User Library** so it appears in every file. A star means unsaved. Set the thumbnail with *Capture screenshot preview*. Organize it into catalogs.
- Applying a pose asset on a frame keys it.
- **4-second idle:** idle-A at frame 0, idle-B at 30, idle-A at 60, then **Shift+E, Make Cyclic**. Then **offset the head and arm keys 2–3 frames later**: instant overlap, and it looks much smoother.

#### Stepped, Spider-Verse-style motion
- In the Graph Editor, select one key, then **Add Modifier, Stepped Interpolation**. Select all, **Copy F-Modifiers**, paste to every curve.
- Step 2 means animating "on twos", 3 means on threes. It looks great one-shot and odd when looped.

#### Rig handling
- **Reset a pose:** A, then Alt+G and Alt+R.
- **Two rigs in one scene:** never Ctrl+C/Ctrl+V the rig, because shared data makes a mess. Right-click the collection, **Duplicate Collection**, then re-hide the helper bones and widgets the duplicate unhides.
- **Long moves:** move the whole rig in Object Mode. Use the root bone only when the travel is part of the animation (jumps, runs, falls).
- **Auto-keying trap:** with auto-key on, even Object Mode moves key the current frame. Be on frame 0 when repositioning, or you'll chase phantom keys.

#### Your avatar on the rig
- **Texture:** in Studio, right-click the character, Export Selection as OBJ, which gives a `*_diff` texture. In Blender's Shader Editor, use the Image Texture's folder icon to swap the face and body images.
- **Accessories:**
  1. Export only the accessory as OBJ, then import it.
  2. Object, Set Origin, Origin to Geometry, then zero its location and rotation (often Z = 180°).
  3. Position it, select the accessory, shift-select the armature, enter Pose Mode, pick the bone, then **Ctrl+P, Bone**.
- **Purple (missing) texture:** the downloaded zip wasn't extracted. The texture must sit beside the .blend.

#### Import into Studio
1. In the add-on, **Export animation**, which copies the baked data to the clipboard.
2. In Studio, insert a rig. Moon Animator's inserter anchors it. With free "Load Character" plugins, **anchor HumanoidRootPart yourself**; he says unanchored rigs caused 99% of failed imports.
3. Blender Animations plugin: **Import from clipboard**, set the **priority** (Idle for idles), then Upload. Moon Animator can import the uploaded animation for looping and further edits.

### Common mistakes

- **Pose-to-pose with no anticipation, drag or overshoot:** the move reads as robotic.
- **Everything hitting its key on the same frame:** no overlap, so it looks stiff. Offset the head and arms by 2–3 frames.
- **Animating at 60 fps:** it looks floaty and CG. Use 24, and stepped interpolation for a choppy style.
- **Copy-pasting a rig for a second character:** use Duplicate Collection.
- **Repositioning a rig off frame 0 with auto-key on:** phantom keys.
- **An unanchored HumanoidRootPart on import:** the import fails silently.
- **An unextracted tutorial zip:** a purple rig.
- **Posting rotoscoped animation as original work.**

### Checklist

- [ ] Scene at 24 fps; looping clips set to cyclic.
- [ ] Each move has anticipation, drag on the trailing limb, overshoot, and a settle.
- [ ] Motion paths checked; arcs are clean; the torso leads and the limbs follow by about 2 frames.
- [ ] Reusable poses saved to the User Library pose assets, with thumbnails.
- [ ] Second rigs made with Duplicate Collection; long moves in Object Mode on frame 0.
- [ ] In Studio: HumanoidRootPart anchored, the correct priority set before upload.

### Sources

| Video | Channel | Views (2026-11-09) | URL |
|---|---|---|---|
| Make Your Roblox Animations Feel REAL, Roblox Animation Tips 2026 | Devgrams and Draco | 76k | https://www.youtube.com/watch?v=AH30avEEC9A |
| Roblox Animation in Blender: Advanced Guide (2026) | Nisky | 54k | https://www.youtube.com/watch?v=EM9u4gIHoRg |

Not usable (no transcript available): Pandastroyer, "BEST GUIDE to animating on Roblox for free!" (CKlQJlNuXPI).

## Blender Mesh VFX and Beam Scenery FX - Studied from YouTube

Three techniques from three videos: a procedural mesh-VFX modifier stack in Blender (energy wraps and anime slashes), hand-painted Beam scenery effects, and a free flipbook pipeline. The notes are my own summaries (2026-11-09). Blender menu names are from older Blender UIs; check them in 5.x. Related: [[Roblox VFX Techniques]], *Slash VFX Recipes - Crescent and Ring* (LihFolk), [[VFX and Animation Training Log]].

### 1. The procedural shard stack (MaskedFaces, BlenderBuilder)

One modifier stack turns any dense mesh into broken, swirling energy ribbons:

| Step | Blender | Effect |
|---|---|---|
| 0 | Base mesh: a **UV Sphere subdivided once** (an energy wrap), or a **Circle extruded and scaled inward into a flat ring, subdivided about 3×** (a slash). Put the 3D cursor at the world origin first (Shift+S) | Density = detail; more costs performance |
| 1 | Object Data, **Vertex Group**: assign all vertices | Gives the modifiers something to weight |
| 2 | **Vertex Weight Edit** on that group: Falloff Linear, then a **Custom Curve inverted**; Influence uses a new **Clouds** mask texture with **Global** texture coordinates | Noise-driven weights |
| 3 | **Mask** on the group with **Threshold about 0.48** | Cuts the surface into noisy shards; a higher threshold means thinner shards |
| 4 | **Smooth**, Repeat about 20 | Rounds the hard voxel cuts into flowing ribbons |
| 5 | **Simple Deform, Twist, Z axis**: angle to taste for a wrap, **over 360°** for a spiral slash | The swirl |

**Finishing:**
- **Keep an unmodified duplicate as a backup**, then apply the modifiers on the working copy.
- For a slash: in Edit Mode with X-ray on, circle-select (C) and delete stray shards so a crescent remains.
- Duplicate and rotate copies to fill gaps; vary the shades.
- Preview material: Emission strength about 10 over a darker Base Color, with Bloom.

**Into Roblox:**
1. Export as **OBJ** (or FBX) and import as a MeshPart (MeshId through the import folder icon).
2. **Wrap it snugly around a Neon sphere core** without clipping into it.
3. **Contrast between layers:** a saturated or dark core with light Neon shards, or the reverse. Light on light washes out. Stack 2–3 copies in white, bright blue and dark blue (a spirit-bomb look).
4. Animate on the client: spin with a rotation tween, and grow and fade with Size/Transparency tweens. Neither video shows this step.

### 2. Beam scenery effects (DatBoiEle)

**Stylized waterfall:**
1. A Beam with Transparency 0, equal Width0/Width1, FaceCamera off.
2. **Paint your own texture from reference**: draw in red, then make it white on transparent so Roblox can tint it.
3. Stack 2–3 duplicated beams with the base texture in different tints, with TextureSpeed on.
4. Add one lighter beam with an **edges-only texture**.
5. A Transparency sequence that fades toward the bottom.

**Wind lines:**
1. Paint a white swirl or partial-circle stroke on transparent.
2. Put it on a **curved Beam** (CurveSize0/1) and lengthen the beam until the stroke reads as a gust.
3. Scatter several around the scene.

**Why bother:** waterfalls, falling leaves, wind lines and cauldron fire layered onto a static build are what make scenery feel alive.

### 3. A free flipbook pipeline (DatBoiEle)

1. Hand-draw the frames in a browser animator (FlipAnim) or Clip Studio Paint, which has better brushes, and export a **GIF**.
2. Use ezgif "GIF to sprite sheet" with a **4×4** grid (8×8 for long GIFs).
3. In Photopea, magic-wand delete the grid background so it's transparent.
4. ParticleEmitter: FlipbookLayout 4×4, import the texture.
5. For a looping fire, use FlipbookMode **PingPong** (not OneShot), with **no random rotation and no spread**.

(Blender-rendered sheets are cleaner: see the drill notes. PhaseVFX is the preferred plugin for flipbooks in our workflow.)

### Common mistakes

- **Too much subdivision before the Mask modifier:** heavy meshes that cost frames in Roblox.
- **Applying modifiers without a backup copy:** the procedural settings are lost.
- **Light shards on a light core:** the effect washes out. Contrast the layers.
- **Mesh shards clipping into the Neon core:** z-fighting and a cheap look.
- **Using a texture pack you didn't make for a signature effect:** paint your own from reference.
- **OneShot or random rotation on a looping flipbook fire:** it pops and spins wrongly.
- **Inserting Toolbox particles (e.g. leaves) without auditing them:** malware risk. Audit every insert.

### Checklist

- [ ] Shard mesh: vertex group, then Weight Edit (Clouds, Global), then Mask (about 0.48), then Smooth (about 20), then Simple Deform Z. Backup copy kept.
- [ ] Mesh triangle count is reasonable for mobile; the mesh imports cleanly as a MeshPart.
- [ ] The core and shard colours contrast; shards sit snug, not clipping.
- [ ] Beam textures are hand-painted white on transparent, tinted in Studio, with layered beams plus an edge layer.
- [ ] Looping flipbooks use PingPong or Loop with no random rotation; OneShot only for bursts.
- [ ] Any Toolbox asset is audited before use.

### Sources

| Video | Channel | Views (2026-11-09) | URL |
|---|---|---|---|
| How To Make A Blender VFX For Roblox Studio [FULL PROCESS] | MaskedFaces | 149k | https://www.youtube.com/watch?v=ivILbr1D5e8 |
| HOW TO Make Anime Sword Slash Effect, Easy Blender VFX Tutorial! | BlenderBuilder | 127k | https://www.youtube.com/watch?v=LObz5zCkcDQ |
| I learned Roblox VFX the FAST way.. heres how i did it | DatBoiEle | 159k | https://www.youtube.com/watch?v=l8wZp4JHXjw |
