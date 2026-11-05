# Knowledge Management Skills for AI Agents

[![6 skills](https://img.shields.io/badge/skills-6-0b7285)](#all-skills)
[![Format: Agent Skills](https://img.shields.io/badge/format-SKILL.md-5f3dc4)](https://agentskills.io/specification)
[![Licence: CC BY 4.0](https://img.shields.io/badge/licence-CC%20BY%204.0-2b8a3e)](https://creativecommons.org/licenses/by/4.0/)

Free **Agent Skills** (`SKILL.md`) that help Claude, Claude Code, OpenAI Codex and other AI agents build and maintain knowledge that stays useful: set up a note system, write atomic notes, migrate old notes safely, keep a knowledge base clean, record decisions and capture lessons learned. Each skill includes guardrails that stop an agent changing or deleting your notes without approval.

By [**Afeez Adebayo**](https://afeezadebayo.com) · [Browse online](https://afeezadebayo.com/skills-md/productivity/knowledge-management/) · [LinkedIn](https://www.linkedin.com/in/afeez-adebayo/) · [X](https://x.com/za_icon)

Part of the [Productivity skills](https://github.com/afeezadebayo/productivity-skills) library.

## All skills

| Skill | What it does |
|---|---|
| [`note-system-setup`](note-system-setup) | Sets up a simple note system with folders, templates and rules |
| [`atomic-notes`](atomic-notes) | Turns reading and meetings into single-idea, linked notes |
| [`note-migration`](note-migration) | Moves notes between tools without losing content or links |
| [`knowledge-base-hygiene`](knowledge-base-hygiene) | Finds stale, duplicate and orphaned pages and proposes fixes |
| [`decision-records`](decision-records) | Records decisions with context, options, consequences and review dates |
| [`lessons-learned`](lessons-learned) | Turns a project or incident into lessons and tracked actions |

More skills in this set are planned. See the [full list online](https://afeezadebayo.com/skills-md/productivity/knowledge-management/).

## Install

```bash
git clone https://github.com/afeezadebayo/knowledge-management-skills.git
# Claude Code
cp -r knowledge-management-skills/note-system-setup ~/.claude/skills/
# Codex and other agents
cp -r knowledge-management-skills/note-system-setup ~/.agents/skills/
```

In Claude.ai or Claude Desktop, zip a single skill folder and upload it under **Settings → Capabilities → Skills**.

Then ask your agent, for example: *"Set up a note system for my work"* or *"Write a decision record for this choice."*

## Safety first

These skills propose changes and wait for approval. They never delete, merge or move notes on their own, and they keep a log of what changed.

## Licence

Released under [**CC BY 4.0**](https://creativecommons.org/licenses/by/4.0/). Use, share and adapt, including commercially, with credit to Afeez Adebayo and a link to the source page in each file.
