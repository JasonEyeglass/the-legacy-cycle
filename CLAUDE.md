# THE LEGACY CYCLE

Obsidian vault: `THE LEGACY CYCLE/` (inside this folder, has `.obsidian/`).

Connected via the `obsidian` MCP server (Obsidian "MCP Connector" plugin, http://127.0.0.1:27201/mcp). Prefer its tools for reading, searching and writing notes; Obsidian must be open for it to work. If it is unavailable, write the Markdown files directly.

## Rule: everything we create goes into the vault
- Save every note, doc, plan, idea or other written output created in this project as a Markdown file inside the vault, not elsewhere and not only in chat.
- Use Obsidian conventions: `[[wikilinks]]` to related notes, `#tags`, and YAML frontmatter (`created: YYYY-MM-DD`, `tags:`) at the top.
- Link each new note to at least one existing related note so it connects in the graph. When a new topic appears, create or update a hub/index note for it and link from there.
- Before creating a note, search the vault for an existing one on the same topic and update it instead of duplicating.
- Never edit or delete files in `.obsidian/` unless asked.

## Team rules (5 people, each with their own Claude)
This folder is a shared Git repo. Your memory is not shared with teammates' Claudes; the vault and this file are. Treat them as the team's shared memory.

- **Start of session:** run `git pull` (or confirm the user's Obsidian Git plugin just pulled). Then read [[Team and Roles]], [[Current Status]] and [[Decisions Log]] in the vault before doing anything.
- **Know who you work for.** Ask the user which team member they are if it is not clear, and check their role in [[Team and Roles]].
- **Ownership:** every note has `owner:` in its frontmatter. Only edit notes the current user owns. For someone else's note, add a suggestion under a `## Proposals` heading at the end (name and date), never change their text. New notes get `owner: <current user>`.
- **Decisions:** when the team or user settles a design question, add one line to [[Decisions Log]] (date, decision, who decided, link to note). Do not reverse a logged decision without the user confirming.
- **Status:** update the user's line in [[Current Status]] when starting or finishing a piece of work.
- **End of session:** commit with a clear message and push (`git add -A && git commit -m "..." && git push`). If the push is rejected, `git pull`, resolve any conflict by keeping both people's text, then push. Never force-push, never `git reset --hard`, never delete a teammate's work to fix a conflict.
- **Personal memory:** anything a teammate needs to know goes in the vault, not only in your private memory.
- **Never commit secrets.** `.obsidian/plugins/*/data.json` holds personal MCP tokens and is git-ignored; keep it that way.
