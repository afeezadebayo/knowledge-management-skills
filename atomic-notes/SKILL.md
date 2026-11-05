---
name: atomic-notes
description: "Turns raw captures (highlights, meeting notes, clippings, voice-memo transcripts, an inbox backlog) into atomic, linked, source-tagged permanent notes in plain Markdown, Obsidian or any notes tool. Checks the vault for duplicates, titles each note as a claim, splits bloated notes, adds only links to notes that exist, flags every agent-added statement and leaves the user's own take for the user. Drafts only, never overwrites. Use whenever someone asks to process an inbox, write permanent or evergreen notes, make Zettelkasten notes, split a long note, merge duplicate notes or link notes together. Do NOT use for summarising a book or article for study (use active-reading-notes), choosing a folder structure (use note-system-setup), auditing stale wiki pages (use knowledge-base-hygiene) or logging meeting actions."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/atomic-notes/
  category: productivity
  subcategory: knowledge-management
  version: "1.0.0"
  created: "2026-10-22"
  updated: "2026-10-22"
---

# Atomic Notes: Turn Raw Captures into Linked Permanent Notes

A pile of captures is not a knowledge base. Highlights, clippings and meeting scribbles pile up in an inbox, and the pile is rarely read again. This skill processes a bounded batch of captures into permanent notes that hold one idea each, carry a claim as the title, say where the idea came from, link to notes that already exist and never invent a link or a fact.

It is built for an agent that can read a notes folder or an export of note titles. It works the same in Obsidian, Logseq, Notion, Apple Notes, plain Markdown folders or paper. The tool-specific facts sit in one fenced appendix at the end.

Two design choices separate it from most note-processing prompts. First, it drafts and the human decides: nothing is overwritten, moved or deleted. Second, it does not write the user's thinking for them. The note has a "Your take" line that only the user fills in, because a note written entirely by the agent only summarises the source.

## When to use this skill, and when not to

Use it when the user has captured material and wants it turned into durable notes:

- An inbox, a "to process" folder or a read-later pile.
- Highlights exported from a reading app.
- Meeting or call notes that contain ideas worth keeping beyond the meeting.
- A long note that has grown several arguments and needs splitting.
- Two or more notes that say the same thing and need merging.
- A set of notes that have no links.

Do not use it for:

- Studying a book or paper end to end. Use `active-reading-notes`, then bring the results here.
- Deciding how a vault or wiki is organised. Use `note-system-setup`.
- Finding stale, ownerless or duplicate pages across a wiki. Use `knowledge-base-hygiene`.
- Meeting decisions and action items. Use `meeting-notes-actions`. A task found inside a capture goes to the task system.
- Building a retrieval index for an AI system. Use `rag-knowledge-bases`.
- Giving advice on health, money, legal standing or employment. If captures touch these, the skill records what the source says, labels it unverified and routes it to a human. It does not interpret.

## Guardrails that apply to every run

1. **Draft, never write over.** Output notes as drafts. Create files only in a drafts location the user names. Never overwrite, move, rename or delete an existing note. Changes to an existing note are shown as a diff and applied only after the user says yes to that diff.
2. **Confirm bulk changes.** If a pass would create more than 5 notes or touch more than 1 existing note, list them and wait for explicit confirmation.
3. **No fabricated facts, numbers or links.** Every figure carries its source and date from the capture. A link target must appear in the search results or the supplied title list for this run. Anything that may have changed since it was written gets the marker `verify before relying on this`.
4. **Sensitive data stays out.** Captures can contain names, email addresses, phone numbers, account numbers, passwords, API keys, health details, client names and unpublished financial figures. Redact them before processing (see step 0). Never paste them into a prompt that goes to a third party, never write them into a note, never log them in the report.
5. **Local by default.** If a step would send note content to an external service (a hosted model, a web search, a sync tool), say so and ask first. One published note skill sends note text to external models with no privacy guidance, so treat this as an explicit gate.
6. **Scope boundary.** The skill refuses to delete notes, reorganise folders, publish or send notes, or decide what the user believes.

## Inputs the agent must have before starting

Do not start until all six are answered. Ask for what is missing in one message and wait.

| # | Input | What to ask if missing |
|---|---|---|
| 1 | The captures to process | "Paste the captures or point me to the inbox folder. How many items are there?" |
| 2 | The purpose | "What will these notes be used for (a piece of writing, a decision, study, a project)? One sentence." |
| 3 | A way to search existing notes | "Can I search the vault? If not, paste or export the list of existing note titles." |
| 4 | Existing conventions | "Do you have a title style, folder for drafts and properties you already use? If not, I will use the defaults in this skill." |
| 5 | Write permission | "Should I output drafts in the chat only, or create files in a drafts folder? Name the folder." |
| 6 | Sensitivity | "Do these captures contain personal data, client names, credentials or anything under an NDA? Is the model running somewhere you are happy for it to read them?" |

If input 3 cannot be met, stop. Linking without seeing the existing notes produces broken or invented links, and the dedupe step cannot run.

## Workflow

### Step 0. Sensitivity triage

1. Scan for emails, phone numbers, long digit strings, key-like strings, and the words password, passcode, PIN, diagnosis, salary, invoice.
2. If any are found, replace them with `[redacted: type]` in the working copy and tell the user how many were redacted and of which type. Do not repeat the original values.
3. If input 6 says the material is under NDA or the model is not approved for it, stop and ask the user to confirm the route.

### Step 1. Bound the batch

1. Take at most 10 capture items or about 3,000 words of source text per pass. These are defaults chosen so a person can review the whole output in one sitting. Adjust them only if the user asks.
2. If the inbox holds more than 50 items, do not process it all. Rank items by relevance to the stated purpose (input 2), process the top 10 and list the rest. Then ask the user whether the remainder should be archived as a block. Archiving is the user's decision and is never deletion.

### Step 2. Extract candidate ideas

1. List up to 5 candidate ideas per capture item.
2. Label each candidate with exactly one building block: concept, argument, counter-argument, model, hypothesis or theory, or empirical observation. These six categories come from the Zettelkasten atomicity guide.
3. Route anything that is not one of the six:

| What it is | Where it goes |
|---|---|
| A task with an owner or date | The task system. Do not make a note. |
| A decision from a meeting | `meeting-notes-actions` |
| A quotation | Stays in the source note with its citation. |
| A fact easy to look up and unlikely to be needed | Skip. |
| A feeling, mood or private reflection with no idea in it | Skip unless the user asks to keep it. |

### Step 3. Keep, park or skip

A candidate becomes a note only if all three are true:

1. It fits one building block.
2. It connects to the stated purpose or to an existing note.
3. It can be stated as one sentence containing a verb.

Fail the first test: skip. Fail the second or third: park it in a "Parked" list with a reason. Parked items are not notes.

### Step 4. Search the vault and dedupe

1. For each kept candidate run at least 3 searches: the claim's key noun phrase, two synonyms, and the source's author or title. Record every query and its hit count.
2. If the vault cannot be searched, compare against the supplied title list and record that the check was title-only.
3. Decide using this table:

| Result of search | Action |
|---|---|
| An existing note makes the same claim | Do not create a note. Propose a diff that adds the new source and date to that note's Sources section. |
| An existing note makes a more general version of the claim | Create a new, narrower note. Link it with `extends`. |
| An existing note takes the opposite stance | Create a new note. Link both ways with `contradicts`. |
| An existing note makes the same claim with less detail | Propose a diff that refines it. Do not create a duplicate. |
| 3 or more notes share the key noun phrase | Show the titles to the user before writing. A hub note may be needed. |
| You cannot tell which row applies | Ask the user. Do not guess. |
| No hits | Create a new note. |

### Step 5. Atomicity test and splitting

Put each kept candidate through five questions. All five must pass.

1. Can the note be named in one short sentence without "and" joining two stances?
2. Is it understandable at a glance, without opening another note?
3. Could any part be removed without weakening the idea?
4. Is anything essential missing?
5. Is it exactly one of the six building blocks?

If a candidate fails:

1. Free-write the content as it stands.
2. Identify the building block of each element and give each its own note.
3. Check that nothing was lost: every sentence of the original is either in a new note, listed as dropped with a reason, or parked.
4. Do not split if the pieces become useless apart. A problem and its solution that only make sense together stay as one note.

Two guards against over-splitting, both defaults from this skill:

- A candidate under 25 words with no link is merged into its parent note or parked.
- A drafted note over 300 words is re-tested against the five questions. If it passes, keep it. Length is not the test.

### Step 6. Write the title

The title is a sentence that makes a claim.

1. It contains a verb.
2. A reasonable reader could disagree with it.
3. It is under 12 words.
4. It does not contain characters your tool uses for link syntax: `[`, `]`, `#`, `^`, `|`.
5. It does not duplicate an existing title (checked in step 4).

Concept notes may use a definition, such as "Retention is the share of users who return in a period". Topic titles ("Billing", "Notes on onboarding") fail the test.

### Step 7. Write the body

1. Paragraph 1: the claim in plain words.
2. Paragraph 2: the reason or evidence, attributed to the source and date.
3. Paragraph 3 (optional): limits, counter-evidence or what would change the claim.
4. Keep the whole body to 2 to 4 short paragraphs.
5. Add one line: `Your take:`. The user writes it. If the user has not supplied one, leave it as `Your take: (to write)`, keep the note's status as `draft` and list the note under "Needs your take" in the report.

The agent does not invent the user's opinion. The benefit of rewriting ideas in your own words is plausible but less settled than popular advice suggests. A 2021 replication of a well-known note-taking study found no significant difference in quiz performance between typed and handwritten notes, and could not confirm that own-words notes improve performance. The skill asks for the user's own line without claiming research proves a benefit.

### Step 8. Tag provenance

Tag each factual sentence in the body:

| Tag | Meaning |
|---|---|
| `[source]` | Stated in the capture. Cite which one. |
| `[checked]` | Confirmed against a second source. Give the link and the date checked. |
| `[inference]` | A conclusion drawn by the user or the agent. Say which. |
| `[agent-added]` | Information the agent added that is not in the capture. |

Default: add no `[agent-added]` statements. If one is needed for the note to make sense, the report lists it and the user approves or removes it. Never round, restate or improve a number. Copy it with its source and date.

If the claim involves health, money, legal standing or employment outcomes, add the line `Recorded from the source. Not advice. A qualified person should review before anyone acts on it.`

### Step 9. Link

1. Give each note 2 to 5 outbound links. These are defaults from this skill.
2. Write each link as `[[Target]]`, then a relation, then a one-line reason.
3. Relations come from a fixed list: `supports`, `contradicts`, `extends`, `example of`, `depends on`, `applies to`, `part of`.
4. A target must appear in the step 4 search results or the supplied title list. Do not link to a note you have not seen.
5. If a useful target does not exist, put it in "To create" in the report. Do not link to it.
6. If a hub note exists for the topic, link to it with `part of`.
7. A note with no valid links is allowed only if the report says why and flags it for review at the next pass.

### Step 10. Properties

Use the property block in the templates. Check it against the Obsidian-compatible rules before output:

1. The block starts and ends with `---` lines.
2. Each property name appears once, so there is one `tags` property.
3. Lists use one hyphen item per line.
4. Dates are `YYYY-MM-DD`.
5. Internal links inside a property are in double quotes.
6. No nested properties.

If the agent can run code, run the lint script in the templates on every draft. A nonzero exit blocks output until fixed.

### Step 11. Hand over drafts

1. Output each new note in full, one block per note.
2. Output each change to an existing note as a diff block.
3. Apply guardrails 1 and 2. Wait for the user's confirmation before creating files beyond the drafts folder.
4. Finish with the report.

### Step 12. Report

Produce the report using the template. It must account for every capture sentence as used, parked or dropped.

## Worked example

The example is invented for illustration. No figures in it come from a real team.

**Inputs.**

- Purpose: "Prepare the quarterly retrospective for a four-person support team."
- Existing note titles supplied by the user:
  1. Support tickets cluster around billing questions
  2. Documentation is read after the person has already asked
  3. Quarterly retrospectives should end with three decisions
  4. Onboarding calls shorten time to first value
  5. Support operations (hub note)
- Draft location: `_drafts/`
- Sensitivity: no personal data; local model.
- Capture (the user's meeting note):

> Retro 14 Oct 2026. Tickets about billing dates were the biggest group again. People confuse when the invoice is issued with when the card is charged. Amina thinks a one-paragraph explainer in the welcome email would help. We have not tested it. Three customers said they only found the help article after contacting us. Idea: ask for the first search term on each call. Sipho to draft the explainer by Friday. We keep leaving retros with too many actions.

**Step 0.** No personal data found. Zero redactions.

**Steps 2 and 3.**

| # | Candidate | Block | Decision |
|---|---|---|---|
| C1 | Customers confuse invoice date with charge date | observation | Keep |
| C2 | A welcome-email explainer may reduce billing tickets | hypothesis | Keep |
| C3 | Customers found the help article only after contacting support | observation | Keep |
| C4 | Asking for the first search term shows why help content was missed | hypothesis | Keep |
| C5 | Sipho drafts the explainer by Friday | task | Route to task system |
| C6 | Retros leave with too many actions | observation | Park: one remark, no evidence |

**Step 4. Queries run.**

| Candidate | Queries (hits) | Result |
|---|---|---|
| C1 | "billing" (1), "invoice date" (0), "charge" (0) | Matches note 1 on the same subject with less detail. Propose a diff. |
| C2 | "welcome email" (0), "explainer" (0), "billing tickets" (1) | No existing claim. New note. |
| C3 | "help article" (0), "documentation" (1), "found after" (0) | Matches note 2 as supporting evidence. Propose a diff. |
| C4 | "search term" (0), "first question" (0), "help content" (1) | No existing claim. New note. |

**Output: new note 1** (`_drafts/Welcome-email explainers might reduce billing-date tickets.md`).

```markdown
---
type: permanent
status: draft
source: "[[Retro 2026-10-14]]"
captured: 2026-10-14
created: 2026-10-22
tags:
  - support
  - billing
---

# Welcome-email explainers might reduce billing-date tickets

A one-paragraph explainer of invoice date versus charge date, placed in the welcome email, might reduce tickets about billing dates. [source: Retro 14 Oct 2026]

Billing-date tickets were the largest group at the 14 October 2026 retro, and customers confuse the invoice date with the charge date. Amina proposed the explainer. It has not been tested. [source: Retro 14 Oct 2026]

Your take: (to write)

## Links
- [[Support tickets cluster around billing questions]] - depends on - the explainer targets this cluster
- [[Documentation is read after the person has already asked]] - applies to - an email arrives before the question, which tests that claim
- [[Support operations]] - part of - hub for support notes
```

**Output: new note 2** (`_drafts/Asking for a customer's first search term shows why help content was missed.md`), same shape: hypothesis, status `draft`, one `[source]` line, `Your take: (to write)`, links to `Documentation is read after the person has already asked` (`supports`) and `Support operations` (`part of`).

**Output: diff for existing note 1** (`Support tickets cluster around billing questions`).

```diff
## Sources
+ - Retro 14 Oct 2026: billing-date tickets were the largest group; customers confuse invoice date with charge date. [source]
```

**Output: diff for existing note 2** (`Documentation is read after the person has already asked`).

```diff
## Sources
+ - Retro 14 Oct 2026: three customers said they found the help article only after contacting support. [source]
```

**Property lint.** The first new note exits 0 with the lint script.

**Report.**

| Item | Count | Detail |
|---|---|---|
| Captures processed | 1 | Retro note, 14 Oct 2026 |
| New notes (draft) | 2 | C2, C4 |
| Diffs proposed | 2 | C1, C3 |
| Routed elsewhere | 1 | C5 to the task system |
| Parked | 1 | C6, one remark without evidence |
| Needs your take | 2 | Both new notes |
| To create | 0 | |
| `[agent-added]` statements | 0 | |
| Redactions | 0 | |

The user approves the diffs, writes a "Your take" line on each new note and sets `status` to `permanent`.

## Templates

### Permanent note

```markdown
---
type: permanent
status: draft
source: "[[Source note or citation]]"
captured: YYYY-MM-DD
created: YYYY-MM-DD
tags:
  - topic
---

# Claim sentence with a verb

Paragraph 1: the claim in plain words. [source: where]

Paragraph 2: the reason or evidence, with source and date. [source: where]

Paragraph 3 (optional): limits or counter-evidence. [inference: user or agent]

Your take: (to write)

## Links
- [[Existing note]] - relation - one-line reason

## Sources
- Source, date captured. [source]
```

### Diff for an existing note

```diff
## Section being changed
- old line
+ new line
```

Above the block, state the note title, the reason, and "Reply yes to apply this diff."

### Batch report

```markdown
| Item | Count | Detail |
|---|---|---|
| Captures processed | | |
| New notes (draft) | | |
| Diffs proposed | | |
| Routed elsewhere | | |
| Parked | | (reason for each) |
| Needs your take | | |
| To create | | |
| [agent-added] statements | | |
| Redactions | | (types only, never values) |
```

Add a query log: candidate, queries run, hit counts, decision.

### Starting prompt

```text
Process these captures with the atomic-notes skill.
Purpose: [one sentence]
Captures: [paste or path]
Existing note titles: [paste, or confirm you can search the vault]
Drafts folder: [name, or "chat only"]
Sensitivity: [none / describe]
Draft only. Do not overwrite, move or delete anything.
```

### Property lint script

Save as `prop_lint.py` and run `python3 -I prop_lint.py note.md`. Exit code 0 means the block passes. It needs only the Python 3 standard library.

```python
import re, sys
from datetime import datetime

def lint(text):
    lines = text.split("\n")
    if lines[0].strip() != "---":
        return ["first line must be ---"]
    ends = [i for i, l in enumerate(lines[1:], 1) if l.strip() == "---"]
    if not ends:
        return ["no closing --- line"]
    out, seen = [], set()
    for n, l in enumerate(lines[1:ends[0]], 2):
        if not l.strip() or l.lstrip().startswith("- "):
            continue
        m = re.match(r"^([A-Za-z0-9_-]+):\s*(.*)$", l)
        if not m:
            out.append(f"line {n}: nested or malformed property")
            continue
        k, v = m.groups()
        if k in seen:
            out.append(f"line {n}: duplicate {k}")
        seen.add(k)
        if "[[" in v and not v.startswith('"'):
            out.append(f"line {n}: quote internal links")
        if k in ("created", "captured") and v:
            try:
                datetime.strptime(v, "%Y-%m-%d")
            except ValueError:
                out.append(f"line {n}: {k} must be YYYY-MM-DD")
    out += [f"missing {k}" for k in ("type", "status", "source", "created", "tags") if k not in seen]
    return out

problems = lint(open(sys.argv[1], encoding="utf-8").read())
print("\n".join(problems) or "OK")
sys.exit(1 if problems else 0)
```

## Failure modes and anti-patterns

| # | Failure | How to spot it | What to do instead |
|---|---|---|---|
| 1 | **Processing as summarising.** The inbox becomes a folder of summaries, none connected. This is the collector's fallacy. | Notes with no links, no "Your take", titles that are the source's title. | Require a link and a claim title before a note counts. Park what fails. The named fix is a processing step before anything enters the permanent system. |
| 2 | **Topic-titled catch-all notes.** Titles like "Onboarding" collect everything. | The title has no verb. Searching returns one long note on every topic. | Apply the title test in step 6. Split with step 5. |
| 3 | **Over-splitting.** Notes shrink to fragments that cannot be used alone. | Notes under 25 words with no link. Problem and solution in separate notes. | Apply the merge guard and the problem-and-solution exception in step 5. Usability beats orthodoxy. |
| 4 | **Paralysis over atomicity.** The agent or the user holds back notes until they are perfect. | Many items parked, none created. | Unfinished notes are allowed. Keep `status: draft` and move on. The atomicity guide says not to be paralysed by the idea of needing atomicity before you write notes. |
| 5 | **Invented links and confident but unchecked claims.** The agent links to notes that do not exist or states a claim the source never made. One published note skill does not check whether a claim is true or its source accurate. | A link target missing from the query log. A sentence with no tag. | Targets come only from step 4 results. Every sentence carries a provenance tag. Anything `[agent-added]` is listed for approval. |

## Quality bar

Run every item before handing over. Each one is checkable.

1. Every new note title contains a verb and is under 12 words. Count them.
2. The query log shows at least 3 searches per kept candidate, or states that the check was title-only.
3. Every link target appears in the query results or the supplied title list. Compare the two lists.
4. Every link has one of the seven relations and a one-line reason.
5. Every factual sentence carries one of the four provenance tags.
6. The count of `[agent-added]` statements in the report equals the count in the notes.
7. Every new note has a source line with a date.
8. Every new note has `Your take:` and status `draft` unless the user supplied the take.
9. The property lint script exits 0 for every draft, or the six property rules were checked by eye and the report says so.
10. The report accounts for every capture sentence as used, parked or dropped.
11. No existing note was overwritten, moved, renamed or deleted. Check the write log.
12. No redacted value appears in the output, and no more than 5 new notes or 1 changed note were created without confirmation.

## Cost and effort

The cost is mostly input tokens. English text runs at about 1.3 tokens per word (a rule of thumb; check your tokeniser), so a 3,000-word batch is about 4,000 input tokens before search results, and the worked example's output is under 1,000 tokens. Searching a local folder has no API cost. A hosted connector may count searches against its rate limits, so batch the queries. The cheaper path is to supply the title list and compare by title only, which risks missing near-duplicates worded differently.

## Handoffs to related skills

- `active-reading-notes`: turning a book or report into notes before processing them here.
- `spaced-repetition`: turning permanent notes into review cards.
- `note-system-setup`: choosing where drafts and permanent notes live.
- `knowledge-base-hygiene`: reviewing and archiving notes later.
- `meeting-notes-actions`: decisions and actions from meetings.
- `fact-checking` and `source-evaluation`: moving a claim from `[source]` to `[checked]`.

## Appendix: tool-specific notes

Everything above is tool-agnostic. This appendix is the only place that names a product. Verify each point before relying on it, because products change.

**Obsidian** (checked 22 October 2026 against Obsidian Help and the `obsidian-markdown` skill by kepano).

- Properties are YAML `name: value` pairs between `---` lines at the top of the file. Types: Text, List, Number, Checkbox, Date, Date and time, Tags. A type set for a property name applies across the vault.
- Each property name must be unique within a note. Only one `tags` property is allowed.
- Hashtags in a text property do not create tags. Internal links in a property must be quoted. Nested properties are not supported.
- `tag`, `alias` and `cssclass` lose default support in Obsidian 1.9. Use `tags`, `aliases` and `cssclasses`. Verify before relying on this.
- Wikilinks: `[[Note Name]]`, `[[Note Name|Display Text]]`, `[[Note Name#Heading]]`, `[[Note Name#^block-id]]`. The kepano skill states that Obsidian tracks renames automatically for wikilinks.
- Tags may contain letters, numbers (not first), underscores, hyphens and forward slashes.

**Plain Markdown folders.** Use relative links such as `[text](../path/note.md)`. Renaming a file breaks them unless a tool rewrites them, so keep a link check in the review pass.

**Logseq, Notion, Apple Notes and other tools.** Not verified in this version of the skill. Use the tool-agnostic steps, write links by hand and check the product's own documentation for property and link syntax before adding a block.

**Claude Agent Skills.** `name` is limited to 64 characters (lowercase letters, numbers, hyphens) and `description` to 1,024. Only metadata loads at start-up. Anthropic advises using skills from trusted sources only. Verify limits in the Anthropic documentation.

## Sources

Last verified 22 October 2026 for every source below.

- Anthropic, Agent Skills overview: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview. Frontmatter limits and security advice.
- Zettelkasten Method, The Complete Guide to Atomic Note-Taking: https://zettelkasten.de/atomicity/guide/. Atomicity test, six building blocks, splitting rules, common mistakes.
- Obsidian Help, Properties: https://obsidian.md/help/properties. Property types, YAML rules, quirks.
- kepano, obsidian-markdown skill: https://github.com/kepano/obsidian-skills. Wikilink and property syntax as an agent skill.
- lyndonkl, zettel-note skill (via claudeskills.info): https://claudeskills.info/ja/skills/lyndonkl/claude/zettel-note/. Claim titles, link labels, dedupe rule, gaps. Directory page, excerpt only.
- hxy9243, zettel-brainstormer skill (via skills.cat): https://skills.cat/skills/hxy9243/skills/zettel-brainstormer. Link traversal caps, gaps in privacy guidance.
- Tessl registry review of an Obsidian skill: https://tessl.io/registry/skills/github/deepgram/dglabs-deepclaw/obsidian/review. Review criteria and recommended workflow checkpoints.
- dsebastien.net, PKM anti-patterns: https://www.dsebastien.net/ai-wiki-pkm-pkm-anti-patterns/. Collector's fallacy and related patterns, with fixes.
- Urry et al. (2021), replication of Mueller and Oppenheimer (2014), reported by the Association for Psychological Science: https://www.psychologicalscience.org/news/releases/no-benefit-handwriting-notes.html. Psychological Science, 32(3), 326 to 339. 145 Tufts undergraduates in the replication, 67 Princeton undergraduates in the original.
- Sam McLeod, common pitfalls in writing agent skills (4 July 2026): https://smcleod.net/2026/07/writing-and-reviewing-agent-skills-common-pitfalls/. Description and structure pitfalls.

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/atomic-notes/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.
