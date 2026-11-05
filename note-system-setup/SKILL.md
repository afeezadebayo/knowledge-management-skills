---
name: note-system-setup
description: "Designs or repairs the structure of a personal or team notes system (folders, naming, properties, inbox flow, weekly rhythm) for plain Markdown, Obsidian or any tool that exports files. Diagnoses first with a short standard-library script that flags root files, duplicate names, missing properties, broken links, orphans and an ageing inbox, then picks the smallest layout (PARA, flat with links, documentation kinds) and adds folders only when a flag calls for it. Proposes a move plan with link impact and never moves, renames or deletes without confirmation. Use whenever someone asks to set up or tidy a vault, choose PARA or folders, define note naming or prepare notes for an AI agent. Do NOT use for processing captures into notes (use atomic-notes), auditing stale wiki pages (use knowledge-base-hygiene), task capture (use task-capture-clarify) or building a retrieval index (use rag-knowledge-bases)."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/note-system-setup/
  category: productivity
  subcategory: knowledge-management
  version: "1.0.0"
  created: "2026-10-22"
  updated: "2026-10-22"
---

# Note System Setup: Design or Repair a Notes System Without Over-Building It

Most note systems are over-built before they are used. The folders, tags and templates arrive in week one and the notes arrive later, if at all. This skill designs or repairs the structure of a personal or team notes system for plain Markdown folders, Obsidian, or any tool that can export to files. It starts with a small structure, adds a folder only when a named problem calls for it, and shows every proposed move in a plan that the user approves.

Two choices shape it. First, the agent diagnoses before it designs: a short script reads the folder and reports what is actually wrong, so the structure answers real problems. Second, the agent proposes and the user decides: nothing is moved, renamed or deleted without a confirmed plan.

## When to use this skill, and when not to

Use it when the user wants to:

- Set up a new vault, notes folder or team notes space.
- Fix a folder that has grown messy, with files in the root, duplicate names or broken links.
- Choose between PARA, topic folders, a flat folder with links, or a documentation layout.
- Define a naming convention and a small set of properties.
- Prepare a notes folder that an AI agent will read.

Do not use it for:

- Turning captures into permanent notes. Use `atomic-notes`.
- Finding stale, ownerless or duplicate pages across a wiki. Use `knowledge-base-hygiene`.
- Capturing and clarifying tasks. Use `task-capture-clarify`.
- Running a weekly review. Use `weekly-review`.
- Building a retrieval index for an AI system. Use `rag-knowledge-bases`.
- Advice on tool purchases or on how long to keep legal, tax or employment records. Retention is a legal question and this skill gives no advice on it.

## Guardrails that apply to every run

1. **Propose, never move.** The agent produces a plan. It does not create, move, rename or delete a file or folder until the user confirms the plan. Confirm in batches of at most 20 moves.
2. **Back up first.** Before any approved move, the user confirms a copy or version control exists. Renames can break links in plain Markdown, and the agent cannot undo them.
3. **No deletion.** The strongest action is a move to `archive/`. Deleting is a separate human decision.
4. **No fabricated figures.** Thresholds in this file are labelled defaults. Facts about a tool carry its documentation and the date read, and anything that depends on a version carries `verify before relying on this`.
5. **Personal data.** The checker reads note text on the user's machine to find links and count words. Notes can hold names, client details, health or financial information. Run it locally, do not paste note text into an external service, report only file names and flags, and do not quote note bodies in the report.
6. **Local by default.** If a step would send file names or text to an external service, say so and ask first.
7. **Not advice.** Do not give legal, tax, employment or medical guidance about what to keep or discard. Route those questions to a person.
8. **Scope boundary.** The skill sets structure. It does not rewrite note content, process captures, or change tool settings.

## Inputs the agent must have before starting

Ask for what is missing in one message and wait.

| # | Input | What to ask if missing |
|---|---|---|
| 1 | Purpose | "What is this system for: running projects, research and writing, a team knowledge base, or reference?" |
| 2 | Users | "Just you, a team, or an AI agent that reads it too?" |
| 3 | Tool | "Which tool holds the notes, and can it export or sync to plain Markdown files?" |
| 4 | Size and state | "Roughly how many notes, and is this a new start or a repair?" |
| 5 | What hurts | "What goes wrong today: you cannot find things, files pile up, links break, you never process the inbox?" |
| 6 | Safety net | "Is there a backup or version control I can rely on before any move?" |

If the user has no folder yet, skip the check and start at step 2 with an empty structure.

## Workflow

### Step 0: Confirm scope and safety

1. Confirm inputs 1 to 6.
2. Confirm that the agent works on a copy or a version-controlled folder. If neither exists, stop at the plan and do not move anything.
3. Tell the user what the checker reads (see guardrail 5).

### Step 1: Diagnose

Run the checker from the Appendix on the folder. It reports file names and flags only:

```
python3 vault_check.py path/to/notes --today 2026-10-22
```

| Flag | Meaning |
|---|---|
| `ROOT_FILE` | A note sits outside any folder |
| `ODD_CHARS` | The file name has characters outside letters, digits, space, hyphen, underscore, apostrophe and parentheses. This is a convention of this skill, not a tool rule |
| `DUP_NAME` | Two notes share a name, ignoring case. A link to that name is ambiguous |
| `MISSING_PROPS` | A note outside `inbox/` and `templates/` lacks `type`, `status` or `created` |
| `EMPTY` | Under 10 words after the properties |
| `INBOX_OLD(Nd>Md)` | An inbox note is older than the limit (default 14 days). Age comes from the `created` property, or the file date if there is none |
| `BROKEN_LINK(name)` | A `[[wikilink]]` points to no note |
| `ORPHAN` | No other note links to it. Not checked in `inbox/`, `templates/` or `archive/` |
| `BIG_FOLDER(n>m)` | A folder has more notes than the limit (default 50) |

The checker expects folders named `inbox`, `templates` and `archive`. Rename the constants if the user's names differ. It handles `[[wikilinks]]` only. If the user writes standard Markdown links, skip the link flags and say so.

Read the output with the user's answer to input 5. If what hurts does not appear in the flags, ask for an example before designing anything.

### Step 2: Choose a layout

Pick the smallest layout that fits the purpose. Show the user the choice and the reason.

| Purpose | Start with | Why |
|---|---|---|
| Any, and unsure | `inbox/`, `notes/`, `projects/`, `archive/`, `templates/` | Five folders, each with a clear job. Add nothing until step 3 says so |
| Running projects and commitments | PARA: Projects, Areas, Resources, Archives | Organises by actionability |
| Research and writing | A flat `notes/` folder plus a few index notes that link to related notes | Links carry the structure, so notes can belong to several topics |
| Team documentation | Four kinds of page: tutorials, how-to guides, reference, explanation (Diátaxis) | Separates pages by the reader's need |

PARA, as Tiago Forte defines it, has four categories. Projects are short-term efforts with a goal. Areas are ongoing responsibilities with no end point. Resources are topics of interest. Archives hold items from the other three that are no longer active. The article gives principles and not step-by-step filing rules, so this skill adds three tie-breaks:

1. If the item supports a project that is active now, file it with that project.
2. If there is no active project, file it under Resources.
3. If it is finished or on hold, move it to Archives.

Forte's article also says that areas never finish, so break them into projects to see real workload. Use that when the user cannot tell a project from an area: a project has an end, an area does not.

One published example of a plain-Markdown layout, from a second-hand MindStudio article, uses `_templates/`, `projects/`, `research/`, `reference/`, `meetings/` and `inbox/`. Treat it as an example and not as a standard. The article says not to over-engineer the structure.

### Step 3: Add structure only when a trigger fires

A new folder, tag or template needs a named trigger from the diagnosis or from the user. The defaults below are starting points for the user to change.

| Trigger | Response |
|---|---|
| Notes in the root | Move them to `inbox/` for processing, not straight to `notes/` |
| A folder over the size limit (default 50 notes) | Ask what the user looks for there, then propose a split by that question or an index note |
| A search the user repeats and cannot answer | Add one property or one index note for it |
| A document type the user creates every week | Add one template |
| Nothing | Add nothing |

If the user asks for a structure that the diagnosis does not call for, say so once, record the request as optional, and continue. Over-engineering a system while the notes stay thin is a documented pattern (see failure modes).

### Step 4: Set a naming convention

Offer one convention and let the user change it.

1. Permanent notes: a short claim as the title, such as `welcome emails reduce billing tickets`. `atomic-notes` explains how to write one.
2. Projects, reference pages and meetings: a noun phrase plus an ISO date where the date matters, such as `q4 onboarding revamp` or `2026-10-22 support sync`.
3. Names are unique across the whole folder, ignoring case, so that a link has one target.
4. Stay with letters, digits, spaces, hyphens and apostrophes in names. The checker flags anything else as `ODD_CHARS`.

### Step 5: Set a minimal property block

Three properties on every note outside `inbox/` and `templates/`: `type` (note, project, reference, meeting), `status` (draft, active, done) and `created` (`YYYY-MM-DD`). Add `tags` and `aliases` only if the user already uses them.

If the tool is Obsidian, the Appendix lists the property rules that the checker follows. If it is not Obsidian, check the product's documentation for property syntax before writing any block.

### Step 6: Define the inbox and the rhythm

1. All captures go to `inbox/`. Nothing enters `notes/` or `projects/` unprocessed.
2. Processing means a note gets a claim title, a link and properties. Use `atomic-notes` for that step. Practitioner advice is to require a processing step before anything enters the permanent system.
3. Set one review slot per week to empty the inbox and run the checker. `weekly-review` covers the wider routine.
4. Set the inbox age limit. The default is 14 days. Choose a number the user will keep.

### Step 7: Write the move plan

For every proposed change, one row: current path, proposed path, reason, link impact, and the flag that triggered it.

1. Link impact: count the notes that link to the file by name. If the tool rewrites links on rename, say so only if the user confirms it. If not, the plan lists each linking note, because a rename can break the link.
2. Resolve `DUP_NAME` before any rename, because links to a duplicated name are ambiguous.
3. Keep the plan to 20 moves per batch. Append each approved batch to a revert log with old and new paths.

Present the plan and wait. Do not move anything.

### Step 8: Check the design against known anti-patterns

Before finalising, test the proposal against these patterns, which come from one practitioner's list of personal knowledge management anti-patterns:

- Saving without processing (collector's fallacy).
- Switching tools more than writing (tool-hopping).
- Polishing folders and templates while notes stay thin (over-engineering).
- Waiting for a perfect note (perfectionism). Rough drafts belong in the inbox.
- Thousands of notes never revisited. Build a review rhythm and archive.

If the proposal fails any of them, simplify it and say what was removed.

### Step 9: Report

Give the user the diagnosis counts, the chosen layout and the reason, the move plan, the optional items, and the checks that were skipped. State that nothing was changed.

## Worked example

The data below is invented. The user keeps research and project notes in a folder of 12 Markdown files, says "files pile up and I never empty the inbox", and has a git repository. The audit date is 22 October 2026. To show every flag, the example lowers the folder limit to 4.

Folder tree:

```
vault/
  Untitled.md
  archive/billing date confusion.md
  inbox/call-notes-oct.md
  inbox/clip-pricing-article.md
  inbox/random-thought.md
  notes/Billing date confusion.md
  notes/empty-stub.md
  notes/pricing page experiments.md
  notes/welcome emails reduce billing tickets.md
  notes/why we log decisions?.md
  projects/q4-onboarding-revamp.md
  templates/note.md
```

Command and output, produced by running the script in the Appendix:

```
python3 vault_check.py vault --today 2026-10-22 --max-folder 4
checked 12 notes on 2026-10-22; 14 flags
Untitled.md | ORPHAN
Untitled.md | ROOT_FILE
archive/billing date confusion.md | DUP_NAME
inbox/clip-pricing-article.md | INBOX_OLD(51d>14d)
inbox/random-thought.md | INBOX_OLD(18d>14d)
notes/ | BIG_FOLDER(5>4)
notes/Billing date confusion.md | DUP_NAME
notes/empty-stub.md | EMPTY
notes/empty-stub.md | ORPHAN
notes/pricing page experiments.md | MISSING_PROPS
notes/pricing page experiments.md | ORPHAN
notes/why we log decisions?.md | ODD_CHARS
notes/why we log decisions?.md | ORPHAN
projects/q4-onboarding-revamp.md | BROKEN_LINK(Tool logins)
```

How the agent reads it:

- `Untitled.md` is in the root and nothing links to it. It holds unprocessed scribbles, so the plan moves it to `inbox/` and not to `notes/`.
- `archive/billing date confusion.md` and `notes/Billing date confusion.md` share a name. The agent notes that `[[Billing date confusion]]` in the welcome-emails note matches both files, so the link is ambiguous. The plan proposes renaming the archive copy with a suffix after the user confirms, and lists the one linking note.
- The two oldest inbox notes are 51 and 18 days old against a 14-day limit. They go into the next processing session with `atomic-notes`.
- `notes/` has 5 notes against the example limit of 4. With the default limit of 50 this flag would not fire, and the agent does not propose a split.
- `notes/empty-stub.md` is a stub that nothing links to. The agent asks whether to keep it. If not, it goes to `archive/`, not to the bin.
- `notes/pricing page experiments.md` has no `created` property and no inbound links. The agent does not invent a date. It asks the user for one or proposes leaving it blank.
- `notes/why we log decisions?.md` has a question mark in its name and nothing links to it, so a rename to `why we log decisions` is safe once confirmed.
- `projects/q4-onboarding-revamp.md` links to `[[Tool logins]]`, which does not exist. The agent asks whether to create that note or remove the link.

Result: no new folders, one move, two renames proposed, three questions for the user. The agent does not create the folders `research/` or `reference/` that a template might suggest, because no flag called for them. Nothing was changed.

## Templates

### Vault README (the user keeps this in the root of the notes folder)

```
# How this notes folder works
Purpose: {one sentence}
Folders: inbox (unprocessed), notes (permanent), projects (active work), archive (finished), templates.
Names: claim titles for notes; noun phrase plus ISO date for projects and meetings; unique across the folder.
Properties: type, status, created (YYYY-MM-DD).
Rhythm: empty the inbox once a week. Run the checker after.
Last structure change: {date} ({reason})
```

### Move plan

| # | Current path | Proposed path | Reason | Notes linking to it | Flag |
|---|---|---|---|---|---|
| 1 | {path} | {path} | {reason} | {count and names} | {flag} |

### Report

```
Notes system review: {scope}, {date}
Notes checked: {n}. Flags: {n}.
Layout chosen: {layout} because {reason}.
Folders added: {n} (trigger: {flag or user request}).
Moves proposed: {n}. Renames proposed: {n}. Questions for you: {n}.
Checks skipped: {list}.
Changes made by the agent: none.
```

### Starting prompt

```
Use the note-system-setup skill. My notes are in {folder}. They are for {purpose}. What hurts: {problem}. I have a backup or git history: {yes/no}. Run the checker, tell me what is wrong, and propose the smallest structure that fixes it. Show a move plan and wait for my approval.
```

## Failure modes and anti-patterns

| # | Failure | How it shows up | Fix |
|---|---|---|---|
| 1 | **Over-engineering.** Folders, tags and templates built before the notes exist. | Many empty folders, thin notes, long setup sessions. | Step 3: add structure only for a named trigger. Start with five folders. |
| 2 | **Tool-hopping.** The structure is redesigned for each new app. | Repeated migrations, no stable habits. | Choose a layout that survives export to plain Markdown. Judge the system by the notes it produces. |
| 3 | **Moving without a link check.** Files are renamed and links break. | A rise in `BROKEN_LINK` flags after a move. | Step 7: count linking notes, work in batches of 20, keep a revert log, back up first. |
| 4 | **Designing for a hypothetical.** The structure fits an imagined future user. | Folders for projects that do not exist. | Ask what hurts today and map each folder to a flag or a repeated task. |
| 5 | **A graveyard inbox.** Captures arrive and are never processed. | `INBOX_OLD` flags every week. | Set the age limit, one weekly slot and a processing step. Archive what the user will not process. |

## Quality bar

A run is complete only if every item below holds. Check each one and say which pass.

1. The report states how many notes were checked and how many flags were raised, and the counts match the script output.
2. Every folder proposed has a named trigger from the diagnosis or from the user.
3. The chosen layout comes with a reason tied to the user's purpose.
4. Each move has a link-impact count.
5. `DUP_NAME` flags are resolved or listed before any rename.
6. No batch exceeds 20 moves.
7. The plan states that a backup or version control was confirmed, or that no move is proposed.
8. Thresholds are labelled as defaults or as the user's choice.
9. The report quotes no note body and names only files and flags.
10. The report states that no file or folder was created, moved, renamed or deleted by the agent.
11. Any tool-specific claim carries a source and a date, or is marked unverified.

## Cost and effort

The checker reads a folder of Markdown files and runs in a fraction of a second for a few thousand notes. Cost sits in the conversation: diagnosis and a plan take one session, and each approved batch takes a few minutes of review. Reading note text with a hosted model is not needed for this skill, because the checker works on names, properties and links. If the user wants the agent to open notes, limit it to named files.

## Handoffs to related skills

- `atomic-notes`: for processing inbox items into permanent notes.
- `knowledge-base-hygiene`: for reviewing stale, ownerless or duplicate pages in a wiki after the structure exists.
- `weekly-review`: for the weekly slot that empties the inbox.
- `task-capture-clarify`: for tasks found in captures.
- `rag-knowledge-bases`: when the folder will feed an AI assistant.

## Appendix: tool-specific notes

This appendix is fenced off so that the steps above stay tool-agnostic. Every item was read on 22 October 2026. Platform features change, so `verify before relying on this`.

### The checker script

```python
import os, re, sys, argparse
from datetime import datetime
ap = argparse.ArgumentParser()
ap.add_argument("root"); ap.add_argument("--today", required=True)
ap.add_argument("--inbox-days", type=int, default=14); ap.add_argument("--max-folder", type=int, default=50)
a = ap.parse_args()
today = datetime.strptime(a.today, "%Y-%m-%d")
notes = {}
for d, _, fs in os.walk(a.root):
    if any(p.startswith(".") and p != "." for p in os.path.relpath(d, a.root).split(os.sep)): continue
    for f in fs:
        if f.endswith(".md"): notes[os.path.relpath(os.path.join(d, f), a.root)] = os.path.join(d, f)
names = {}
for p in notes: names.setdefault(os.path.basename(p)[:-3].lower(), []).append(p)
flags, inbound, folders = [], {}, {}
for p, full in sorted(notes.items()):
    top = p.split(os.sep)[0] if os.sep in p else ""
    folders[os.path.dirname(p)] = folders.get(os.path.dirname(p), 0) + 1
    text = open(full, encoding="utf-8").read()
    base = os.path.basename(p)[:-3]
    if top == "": flags.append((p, "ROOT_FILE"))
    if re.search(r"[^A-Za-z0-9 '()_-]", base): flags.append((p, "ODD_CHARS"))
    if len(names[base.lower()]) > 1: flags.append((p, "DUP_NAME"))
    props, body = {}, text
    if text.startswith("---\n") and "\n---" in text[4:]:
        head, body = text[4:].split("\n---", 1)
        for l in head.split("\n"):
            m = re.match(r"^([A-Za-z0-9_-]+):\s*(.*)$", l)
            if m: props[m.group(1)] = m.group(2)
    if top not in ("templates",):
        if top != "inbox" and not {"type", "status", "created"} <= props.keys(): flags.append((p, "MISSING_PROPS"))
        if len(body.split()) < 10: flags.append((p, "EMPTY"))
    if top == "inbox":
        c = props.get("created", "")
        try: d0 = datetime.strptime(c, "%Y-%m-%d")
        except ValueError: d0 = datetime.fromtimestamp(os.path.getmtime(full))
        age = (today - d0).days
        if age > a.inbox_days: flags.append((p, f"INBOX_OLD({age}d>{a.inbox_days}d)"))
    for t in re.findall(r"\[\[([^\]|#]+)", body):
        k = t.strip().lower()
        if k in names:
            for q in names[k]: inbound[q] = inbound.get(q, 0) + 1
        else: flags.append((p, f"BROKEN_LINK({t.strip()})"))
for p in sorted(notes):
    top = p.split(os.sep)[0] if os.sep in p else ""
    if top not in ("inbox", "templates", "archive") and inbound.get(p, 0) == 0: flags.append((p, "ORPHAN"))
for d, n in sorted(folders.items()):
    if n > a.max_folder: flags.append((d + "/", f"BIG_FOLDER({n}>{a.max_folder})"))
print(f"checked {len(notes)} notes on {a.today}; {len(flags)} flags")
for p, f in sorted(flags): print(f"{p} | {f}")
```

The script lists names and flags. It does not move, create or change any file. It reads note text locally to find links and count words.

### Obsidian properties

- Properties are YAML `name: value` pairs in a block at the top of the file, enclosed by `---` lines.
- Each property name must be unique within a note, so only one `tags` property is allowed.
- Dates use `YYYY-MM-DD`. Internal links in properties must be quoted. Nested properties are not supported.
- `tags`, `aliases` and `cssclasses` are default properties. `tag`, `alias` and `cssclass` lose default support in Obsidian 1.9.
- The page does not say how invalid YAML is handled.

The checker looks for the presence of `type`, `status` and `created` only. It does not lint YAML. Check the property rules above when writing blocks.

Source: https://obsidian.md/help/properties

### Plain Markdown folders

Use relative links such as `[text](../path/note.md)` if the tool does not use wikilinks. A rename breaks them unless a tool rewrites them, so keep the link count in the move plan.

### Other tools

Notion, Apple Notes, Logseq and other tools were not verified in this version. Use the tool-agnostic steps and check the product's documentation for folder, property and link behaviour before proposing any change.

### Claude Agent Skills

`name` is limited to 64 characters and `description` to 1,024. Only metadata loads at start-up. Anthropic advises installing skills only from trusted sources. Verify limits in the Anthropic documentation.

## Sources

Last verified 22 October 2026 for every source below.

- Tiago Forte, The PARA Method: https://fortelabs.com/blog/para/. Four categories, actionability, archive conditions, and the lack of filing rules.
- dsebastien.net, PKM anti-patterns: https://www.dsebastien.net/ai-wiki-pkm-pkm-anti-patterns/. Collector's fallacy, tool-hopping, over-engineering, perfectionism and graveyards, with the author's fixes.
- MindStudio, LLM wiki knowledge base pattern: https://www.mindstudio.ai/blog/karpathy-llm-wiki-knowledge-base-pattern. Example plain-Markdown layout. Second-hand, with no primary source linked, so used as an example only.
- Mac Power Users forum, "I deleted my second brain": https://talk.macpowerusers.com/t/i-deleted-my-second-brain/41280. Anecdotes about upkeep and tool churn. Forum opinion, not evidence.
- Diátaxis, Start here: https://diataxis.fr/start-here/. Four kinds of documentation.
- Obsidian Help, Properties: https://obsidian.md/help/properties. Property rules.
- Anthropic, Agent Skills overview: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview. Frontmatter limits and security advice.

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/note-system-setup/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.
