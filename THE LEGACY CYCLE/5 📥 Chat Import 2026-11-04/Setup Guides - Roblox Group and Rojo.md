---
created: 2026-11-04
owner: JasonEyeglass
tags: [setup, tools, rojo, roblox, import]
status: reference
source: Claude chat, 2026-11-04
---

# Setup guides: Roblox group and Rojo

Part of [[Chat Import Hub - Roblox Action MMO]]. Covers steps 2 and 3 of [[Team Tasks - Weeks 1-9]].

## 1. Create the Roblox group

You don't create the group in Roblox Studio. You make it on the Roblox website, in the Creator Dashboard, and then publish the game from Studio under that group. Creating a group costs **100 Robux**.

**Create it (one person, usually the lead)**
1. Go to the Creator Dashboard at create.roblox.com and sign in.
2. Open the account switcher in the top-left corner and click the **plus (+)** button.
3. Upload a logo and enter a group name. A placeholder is fine; you can change the profile later.
4. Click **Purchase**.

**Set up roles for the team**
1. In the account switcher, select the group, open the switcher again, and choose **Roles**. Roles higher in the list have more authority.
2. Create these roles, from top to bottom:

| Role | Who | Permissions to turn on |
| --- | --- | --- |
| Owner | Lead | Everything |
| Lead dev | One trusted backup, so two people can publish | Publish experiences, View analytics |
| Developer | The other team members | Edit experiences, View analytics |

3. Invite each teammate by username on the **Members** page, then assign their role.
4. Turn on **two-step verification** on every account. Roblox requires it for permissions like managing roles and handling group money.
5. Only give **Manage and spend group revenue** to the owner for now.

**Put the game under the group (in Studio)**
1. Use **File → Publish to Roblox As**. Set the creator to the group, not your personal account.
2. Turn on **Team Create** (the Collaborate button) so everyone can work in the same place.
3. When anyone uploads models, animations or sounds, choose the group as the owner. Assets on personal accounts can stop working in the group's game.

## 2. What Rojo is

Rojo is a free tool that keeps your game's scripts as normal files on your computer and syncs them into Roblox Studio while you work.

- **Code lives in a folder of files** (`.luau`) that you edit in VS Code, Cursor or with Claude.
- **Rojo syncs those files into Studio** as you save, so you still test in Studio as usual.
- **The folder goes into git and GitHub,** so you get a full history and branches. If something breaks, go back to a previous version instead of using Studio's undo, which in Team Create also rolls back teammates' work.

Rojo handles **code** only. Maps, models and other things you build in Studio stay in the place file and are shared through Team Create.

**Setup (lead, about 30 minutes)**
1. Install **Rokit** (a tool installer for Roblox), then run `rokit add rojo-rbx/rojo` in the project folder.
2. Run `rojo init` to create `src/server`, `src/client`, `src/shared` and `default.project.json`.
3. Install the **Rojo plugin** in Roblox Studio.
4. Run `rojo serve`, then click **Connect** in the Studio plugin.
5. Push the folder to GitHub and invite the team.

Note: the existing [[Project Plan Overview]] says "Team Create with script drafts; Rojo later." Both work. Rojo from the start gives a git history for code; it's the team's call.

## 3. Who needs the Rojo plugin

Only people who write scripts.

| Role | Needs the Rojo plugin? |
| --- | --- |
| Combat / lead | Yes: writes most of the code and merges everyone's work |
| UI and audio | Yes, if they write UI scripts (likely) |
| World and story | Only if they edit quest or dialogue data stored as script files |
| Art and motion | No: works in Blender and Studio and uploads assets to the group |

**Rule with Team Create:** only one person (the lead) syncs code into the shared Team Create place. If two people sync at once, Rojo overwrites each other's scripts.

**Daily flow for anyone who codes**
1. Work in your **own test copy** of the place (File → Save to File), with Rojo connected.
2. When your code works, commit it to your branch on GitHub.
3. The lead merges the branches and syncs the combined code into the shared Team Create place for the Friday build.

People who don't write code don't need Rojo or GitHub. They work in the shared Team Create place as normal.
