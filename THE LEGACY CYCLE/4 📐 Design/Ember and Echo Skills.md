---
created: 2026-11-06
owner: JasonEyeglass
tags: [design, combat, skills, echo, ember, build]
status: in test place
---

# Ember and Echo Skills

The first playable version of the skills from [[Merge Proposal - Legacy Cycle and Action MMO]]. Built in Fred's test place on 2026-11-06. Echoes live inside the Ember, so **everything unlocks when the player absorbs their Ember at a Shrine of Embers** (Fred's `HasEmber`).

## In Studio
Two scripts. None of Fred's scripts were changed.
- `ServerScriptService.EmberSkillsServer`: cooldowns, range checks and damage. Hits go through Fred's `Combat.Rules`, so block, parry and guard still apply, and it fires his `Combat.Fx` for hit sounds and feedback.
- `StarterPlayer.StarterPlayerScripts.EmberSkillsClient`: keys, placeholder animations, VFX (local only) and the skill bar at the bottom of the screen.
- To remove: delete both scripts.

## Skills
| Key | Skill | Source | Damage | Cooldown | Placeholder animation | VFX |
|---|---|---|---|---|---|---|
| Z | **Ember Strike** | Your Ember (shared by everyone) | 35, cone 11 studs | 6 s | Katana Heavy | Ember slash crescent, spark burst, ground ring |
| 1 | Cinder Lunge | Echo | 20, line 14 studs | 5 s | Dash + forward lunge | Ash trail, short slash |
| 2 | Ember Arc | Echo | 18, circle 9 | 7 s | RunningSlash (spin) | 360° slash, fire ring |
| 3 | Flare Burst | Echo | 8, circle 8, big knockback | 9 s | Parry | Gold dome |
| 4 | Rising Ash | Echo | 22, circle 7 in front | 8 s | DoubleJump | Fire pillar |
| X | Last Stand (ultimate) | Echo | 60, circle 16 | 30 s | Heavy, slowed | Charge, dome, rings, 6 pillars |

**Starter Echo:** *Wyll, the Ashen Squire*, damage role. One of the "minor heroes the world forgot" from the merge plan. It's the only Echo for now and is equipped automatically.

VFX follow the recipes in the LihFolk Vault's *VFX Design Playbook* (slash crescent, ground burst, aura): short-lived neon parts and ember particles, built on each client.

## Known gaps
- Kills by these skills don't give XP yet (CombatCore's defeat listeners are private).
- The HUD Ember arc doesn't drain on Ember Strike.
- Keyboard only: no phone buttons yet (the merge plan's 8-button rule).
- The Embers page in the M menu doesn't list the skills yet.
- No Echo swapping, mastery or saving which Echo is equipped.
- Numbers are first guesses.
- Last Stand missed once in testing, probably because Flare Burst had knocked the target out of range.

## Proposals
