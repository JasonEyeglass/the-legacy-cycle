---
created: 2026-11-04
owner: JasonEyeglass
tags: [tasks, team, plan, import, proposal]
status: proposal
source: Claude doc https://claude.ai/artifact/GLB4s3NbNihss6AEtakRF9
---

# Team Tasks — Weeks 1–9 (Pre-production + Combat Prototype)

Part of [[Chat Import Hub - Roblox Action MMO]]. Built on [[Game Plan v0.1 - Action MMO]]. Note: this sheet uses 4 roles; [[Team and Roles]] lists 5, so reconcile before assigning.

## This week: five steps, all four together

First, we lock in who owns what and set up the shared tools. Nobody builds game content until these five steps are done. This sheet assumes the [[Game Plan v0.1 - Action MMO]] direction: the Shattered Sky setting and Echoes plus weapon styles. Write your name next to a letter.

| Role | Name | Hours per week |
| --- | --- | --- |
| A — Lead and combat (integrator) | | |
| B — World and story | | |
| C — Characters and motion | | |
| D — UI, audio and community | | |

- [ ] **1. Kickoff call (1 hour).** Fill in the table above. Agree who has the final say on design (suggested: A).
- [ ] **2. Roblox group.** Create a group to own the game. Give each person the smallest role they need. Turn on two-step verification for everyone. Upload every asset to the group, not to personal accounts. How-to: [[Setup Guides - Roblox Group and Rojo]].
- [ ] **3. Code home.** A creates a GitHub repo with a Rojo project and invites everyone. Make a private team Discord with channels: #design, #story, #art, #code, #playtest, #changelog.
- [ ] **4. Reference board.** Each person posts 3–5 videos or images: combat feel (A), world mood (B), character art (C), UI and music (D).
- [ ] **5. Vote on placeholders.** Pick a working title, and confirm the setting and the power system. Final lock-in is the **Pitch locked** gate at the end of week 3.

## Person A — Lead and combat (integrator)

A turns the combat idea into a playable arena by week 9 and merges everyone's work into one build each week.

**Weeks 1–3: plan the combat**

- [ ] Set up the repo folders: `src/server`, `src/client`, `src/shared`. One branch per person; A merges.
- [ ] Write the **ownership map**: which folders belong to whom.
- [ ] Write a one-page combat spec: a 3–4 hit M1 combo, dash, block, parry window, stun on hit, cooldowns, and target time-to-kill (a regular enemy dies in 2–5 hits).
- [ ] Decide the hit-detection approach: hitboxes checked on the server, the client only plays effects. Never trust the client.
- [ ] Define an **Echo move format** as data: name, cooldown, damage, hitbox, animation id, VFX id. With this, B and C can add Echoes without touching code.

**Weeks 4–9: build the arena**

- [ ] Combat core: input → server check → damage → effects shown to everyone.
- [ ] Weapon style #1 (sword): combo, dash, block and parry, using C's animations.
- [ ] Echo system plus 2 Echoes (4 moves and an ultimate each).
- [ ] A training dummy and 1 enemy type with telegraphed attacks (designed with B).
- [ ] Mobile buttons wired to combat (with D).
- [ ] A fresh build every Friday for the Saturday play-test.

**Done when:** a grey-box arena where you can fight with the sword and 2 Echoes, on PC and phone.

**Starter prompt for Claude:**

> "Set up a Rojo project for a Roblox action game. Server-authoritative melee combat: the client sends attack input, the server validates cooldown and range, runs a hitbox query, applies damage, then tells all clients to play effects. Start with a 4-hit M1 combo on a sword. Put move data (damage, cooldown, hitbox size, animation id) in a shared module so new moves are data, not code."

## Person B — World and story

B writes the story bible before anything is built, then blocks out the arena and the starter area so A and C have a place to test in.

**Weeks 1–3: the story bible v0.1**

- [ ] **The ending first:** who caused the Sundering, and why the player can remember what others forget. Keep this private to the team.
- [ ] A timeline of 10–20 events, from before the Sundering to now.
- [ ] Island 1 outline using the chapter template: hook → island → a person to care about → new Echo → boss → payoff and cliffhanger. Aim for 30–60 minutes. Draft: [[Chapter 1 - Lanternfall (Story Draft v0.1)]].
- [ ] A mystery ledger with at least 3 mysteries, each with its true answer and the chapter that reveals it.
- [ ] 6 Echo concepts, one line each: name, role (damage, control, support, tank), and the memory it holds. Agree the moves with A and the look with C.
- [ ] A glossary of invented words and their spelling.

**Weeks 4–9: block out the world**

- [ ] A grey-box arena for the prototype: plain parts, no art yet.
- [ ] A grey-box of the starter area (the first 10 minutes), ready for the vertical slice.
- [ ] Enemy #1 design with A: its role, 2 attacks, and how each attack is telegraphed.
- [ ] A script for the first 10 minutes, with no NPC line over 50 words.
- [ ] The play-test checklist for the Saturday sessions.

**Done when:** the story bible is agreed by all four, and the arena and starter-area grey-boxes are in the place.

**Starter prompt for Claude:**

> "Help me write a story bible for an anime-style Roblox MMO. Setting: a world broken into floating islands above a fog that erases memories. Players bond with Echoes, spirits of forgotten things that grant powers and reveal memories. Write the hidden ending first, then a timeline of 15 events, then an outline for Island 1 as a 30–60 minute chapter. Keep it suitable for ages 9 and up: dark themes are fine, no gore."

## Person C — Characters and motion

C sets the art style and proves the Blender-to-Roblox pipeline early, then makes everything that moves in the prototype: the sword, two Echoes and one enemy.

**Weeks 1–3: style and pipeline**

- [ ] An art-style sheet: 3 candidate looks → pick 1. Decide body proportions (standard Roblox R15 rig or a custom rig), color palette, and whether outlines are used.
- [ ] Test the pipeline once, end to end: a model in Blender → export → import into Studio → animate → upload to the group. Write the steps down; the first time is the slowest.
- [ ] Concept sketches (AI-assisted is fine) for the 6 Echoes and 3 weapons, matching B's one-liners.
- [ ] A VFX budget with A and D: low, medium and high quality tiers, so phones don't lag during ultimates.

**Weeks 4–9: what moves in the prototype**

- [ ] The sword model plus animations: idle, run, 4 M1 hits, dash, block, parry, hit reaction.
- [ ] 2 Echoes: 4 moves and an ultimate each, with animation and VFX. The ultimate should look good as a 5–10 second clip.
- [ ] Enemy #1: model, idle, walk, 2 attacks with a visible wind-up, hit reaction, and a death effect that breaks into light or ash (no blood).
- [ ] All assets uploaded to the Roblox group, with ids listed for A.

**Done when:** the sword, 2 Echoes and enemy #1 are animated in the arena and run smoothly on a phone.

**Starter prompt for Claude** (you can use the Blender and Roblox Studio connections on the team computer):

> "Using Blender, make a stylised anime sword for a Roblox game, under 2,000 triangles, with a clean silhouette readable on a phone screen. Then set up a simple rig and export it for Roblox. List each step so I can repeat it for the next weapon."

## Person D — UI, audio and community

D makes the game playable on a phone, makes hits sound good, and runs the play-tests that decide whether we pass the week 9 **Fun test** gate.

**Weeks 1–3: design the screens and the sound**

- [ ] A UI style guide: fonts, colors, minimum button and text sizes, designed phone-first.
- [ ] Wireframes for the combat HUD: health, M1, dash, block, 4 Echo buttons and an ultimate (8 buttons at most), plus the main menu.
- [ ] A thumb test: mock up the button layout and try it on two real phones (one small, one large).
- [ ] A sound list: swing, hit, heavy hit, dash, parry, ultimate, enemy death. Use the Roblox audio library or properly licensed sounds only.
- [ ] Reserve the game name on TikTok, YouTube and X, and set up a public Discord skeleton (keep it closed for now).

**Weeks 4–9: build and test**

- [ ] Build the HUD and mobile buttons in Studio, wired to A's combat.
- [ ] A settings menu: graphics quality (ties to C's VFX tiers) and a screen-shake toggle.
- [ ] Add sounds to the prototype, timed to the hit frame.
- [ ] Recruit 10 outside testers who aren't on the team, and make a 5-question feedback form.
- [ ] Run the **Fun test** in week 9, and record ultimate clips for future marketing.

**Done when:** the prototype plays on a phone with clear buttons and good hit sounds, and 10 testers have given feedback.

**Starter prompt for Claude:**

> "Build a mobile-first combat HUD for a Roblox action game: health bar, M1 button, dash, block, 4 ability buttons with cooldown overlays and an ultimate button with a charge meter. Lay it out for thumbs on a phone in landscape, with minimum 48-pixel tap targets, and show keybind hints instead on PC. Make it scale for both small and large phones."

## Who does what, week by week

All four work in parallel on paper for 3 weeks, then build in parallel for 6 weeks. Everything comes together in the arena for the week 9 Fun test.

| Role | Weeks 1–3 | Weeks 4–5 | Weeks 6–8 | Week 9 |
| --- | --- | --- | --- | --- |
| A · Combat | Spec, repo, Echo format | Combat core + sword (W4–6) | 2 Echoes, enemy, mobile (W7–9) | ↳ |
| B · World, story | Story bible v0.1 | Arena grey-box | Starter area + first-10 script (W6–9) | ↳ |
| C · Art, motion | Style sheet + pipeline | Sword animations | 2 Echoes + enemy #1 anims (W6–9) | ↳ |
| D · UI, audio | Wireframes + sound list | HUD, buttons, SFX, recruit testers (W4–8) | ↳ | **Fun test** |
| Gate | **Pitch locked** (end of W3) | | | **Fun test** (end of W9) |

If the Fun test fails, we spend 1–2 more weeks on combat feel before starting the vertical slice. Combat is the base everything else is built on.

## Weekly rhythm and team rules

We meet twice a week and play the build together every weekend. Everything else happens in Discord.

| When | What | Who |
| --- | --- | --- |
| Monday, 30 min | Plan the week: each person names their 2–3 tasks | All |
| Friday | Merge everyone's work into one build | A |
| Saturday, 1 hour | Play the build together; note what feels bad | All, led by D |
| Any day | Post what changed in #changelog | Whoever changed it |

**Rules**

- **Stay in your own folders.** Ask before moving or deleting someone else's work.
- **Never use undo in Team Create.** It also rolls back teammates' work. Use git to go back.
- **The server checks everything.** Damage, cooldowns, rewards and purchases.
- **AI suggests, a person decides.** Check what Claude made by playing it, not by trusting its summary.
- **New ideas go in #design**, with what they replace. Nothing joins the plan quietly.
- **Upload assets to the group**, never to a personal account.
