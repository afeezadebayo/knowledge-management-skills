---
name: note-migration
description: "Runs a one-way migration of notes from one app to another as a measured transfer: takes a source baseline with a local standard-library script, lists content the destination cannot hold, converts a 10-note sample, converts into a new empty folder, then reconciles notes, files, links, names and dates against the source. Covers Evernote, Notion, Apple Notes, OneNote, Bear, Roam, Logseq, Keep and Confluence moves into Obsidian, Markdown folders or Notion. Never touches the source, never deletes, asks before each bulk step. Use whenever someone asks to move, migrate, import, export or switch notes apps, fix broken links or missing images after an import, or check that a migration lost nothing. Do NOT use for turning captures into notes (use atomic-notes), choosing a folder structure (use note-system-setup), auditing stale pages (use knowledge-base-hygiene) or two-way sync between live tools."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/note-migration/
  category: productivity
  subcategory: knowledge-management
  version: "1.0.0"
  created: "2026-10-22"
  updated: "2026-10-22"
---

# Note Migration: Move Notes Between Apps Without Losing Links, Files or Dates

Moving notes between apps usually ends with a folder that looks full and a vault that is quietly wrong. Links point at pages that were renamed, two notes with the same title become one, and images stay behind in the export. Nobody notices for months, because the file count looked right on the day.

This skill makes an agent run a migration as a measured transfer. It counts the source before anything moves, converts a sample first, converts into a new empty folder, and then compares the two sides number by number. It does not rewrite your notes or delete your source. Retiring the old app is your decision.

It covers moves into a folder of Markdown files (Obsidian, Logseq, plain folders, Git) and into Notion. The conversion itself is done by the best available importer. The agent adds what importers leave out: a baseline, a sample, a reconciliation and a loss list. Tool-specific facts sit in the appendix.

## When to use this skill, and when not to

Use it when the user wants to move an existing body of notes from one place to another:

- Evernote, Notion, Apple Notes, OneNote, Bear, Roam, Logseq or Google Keep into Obsidian or plain Markdown.
- A Confluence space or a Notion workspace into another wiki.
- A folder of Word, HTML or text files into a Markdown vault, or a vault being split or merged.

Do not use it for these jobs:

| Request | Use instead |
|---|---|
| Turn captures into permanent notes | `atomic-notes.md` |
| Choose the folder structure of the new home | `note-system-setup.md` |
| Find stale or ownerless pages before moving | `knowledge-base-hygiene.md` |
| Build a search index over the notes for an AI | `rag-knowledge-bases.md` |

If the user has not decided to leave the old tool, ask first.

## Guardrails that apply to every run

1. **The source is read-only.** The agent never edits, moves, renames or deletes the source export or the live source app. It works on a copy of the export.
2. **New folder only.** All output goes into a new, empty target folder. If the destination is an existing vault, output goes into a staging folder named `_import-YYYY-MM-DD/` inside it, and the user moves notes out later.
3. **Draft, never send.** The agent does not upload, publish, share or sync anything. For moves into Notion, it prepares the files and steps and the user runs the import.
4. **Five confirmation gates.** The agent stops and waits for a yes at each: scope and copies (step 0), the mapping after the sample (step 5), the bulk conversion (step 6), any link rewrite (step 8, in batches of at most 20 files) and any decision to retire the source (step 12, which the agent only recommends).
5. **No deletion, no cancellation.** The agent never deletes the source, empties a trash, or cancels or downgrades a subscription. Keep the source at least 30 days after sign-off (default).
6. **Personal data stays local.** Notes can hold names, emails, phone numbers, client details, health and financial information, and scans of identity documents and invoices. The inventory script runs on the user's machine and prints file names and counts. The agent reports names and flags and does not quote note bodies. Before reading any sample note, it asks whether the model runs somewhere the user accepts for that text.
7. **No credentials in the chat.** Never ask for a Notion integration token, an Evernote or Apple password, or a sync key. If an importer needs one, the user types it into the importer.
8. **Nothing is invented.** Thresholds here are labelled defaults. Tool facts carry a source and the date read. Version-dependent facts carry **verify before relying on this**.
9. **Not advice.** Do not advise on legal, tax, HR or medical record retention. Before the source is retired, ask whether a retention policy applies and route that to a person.
10. **Scope boundary.** The agent refuses to merge into a live vault without staging, to rewrite links before the sample passes, to run a script the user has not read, and to declare success from a file count alone.

## Inputs the agent must have before starting

Ask for what is missing in one message and wait. Do not proceed on assumptions.

| # | Input | What to ask if missing |
|---|---|---|
| 1 | Source tool and size | "Which app are the notes in now, and roughly how many notes and attachments?" |
| 2 | Destination | "Where should they end up: an Obsidian vault, a plain Markdown folder, Notion, or something else?" |
| 3 | Export | "Do you already have an export? If not, which export formats does the source app offer, and do you have desktop access to it?" |
| 4 | What must survive | "Which of these matter most: internal links, images and files, created dates, tags, databases or tables, comments?" |
| 5 | Sensitivity | "Do any notes hold client, health, financial or legal material that must not leave your machine?" |
| 6 | Safety net | "Will you keep the source app and a copy of the export untouched until we have finished checking?" |
| 7 | Platform | "Windows, macOS or Linux, and can you run Python 3 locally?" |

If there is no export route at all, stop. The agent cannot invent one.

## Workflow

### Step 0: Confirm scope and copies

1. Confirm inputs 1 to 7.
2. Confirm the user holds a separate copy of the export and the source app stays untouched. If not, stop.
3. State the plan: inventory, sample, conversion into a new folder, reconciliation, report. Nothing in the source changes.
4. Gate 1: wait for a yes.

### Step 1: Reduce the load before moving

Ask whether any area can be left behind: archived notebooks, duplicate clippings, old meeting notes. Notion's Confluence migration guide advises exporting only the pages you need, because page count and large attachments add size and complexity. If the user is unsure what is stale, use `knowledge-base-hygiene.md` first. Record every exclusion, so the reconciliation can account for every note.

### Step 2: Export and take the source baseline

1. Help the user export in the richest open format the source offers. Use the appendix for the formats and limits of each tool.
2. Unzip the export into a read-only location. Keep the path short.
3. Run the inventory script from the Templates section on the source folder with `--json source.json`.
4. Record the baseline: notes, other files, bytes, links resolved, links broken, flags.

Broken links in the source are the baseline. A link already broken there is not a migration failure. The fortrabbit team that moved a Notion knowledge base to Markdown in 2026 left about 150 links broken and reported that many were already broken in Notion or pointed to trashed documents.

The script checks Markdown links and wikilinks only. If the source uses HTML links, Logseq `((block refs))` or Roam block references, write "not checked" in the report.

### Step 3: List what will not survive

Before converting, list what the destination cannot hold, using the appendix and the sample. Show it as one table.

| Content | Typical outcome | Action |
|---|---|---|
| Databases and tables with views | Flat CSV or one file per row; views, filters and relations lost | Ask if the data matters as data. If yes, keep the CSV beside the notes |
| Callouts, toggles, columns | Raw HTML or plain text | Accept, or schedule a clean-up pass |
| Comments and page history | Often not exported | Ask whether to copy them out by hand before the source is retired |
| Embedded drawings (Gliffy, Draw.io) | Not carried across | Export each as an image first |
| Web forms, automations, synced blocks | Not exportable | List by name for the user to rebuild |

Anything marked "must survive" in input 4 is a blocker. Raise it now.

### Step 4: Choose the conversion route

| Situation | Route |
|---|---|
| The destination tool lists the source as an official import source | Use that importer on a copy of the export. Do not write a converter |
| The source is plain Markdown or HTML and the destination is a folder | Copy into staging, then fix names and links in steps 7 and 8 |
| No importer exists and the vault has fewer than 30 notes (default) | Move by hand, the user doing the copying, the agent checking afterwards |
| No importer exists and the vault has 30 notes or more (default) | Write a converter script, print it in full, get the user to read and approve it, test on the sample first |

Never run a script the user has not read.

### Step 5: Convert a sample

1. Pick 10 notes (default; the gbrain migrate skill uses 5 to 10) by metadata, not by reading them. Take the largest, the oldest, the one with the most attachments, the one with the most links, one with a table, one with a database, one with unusual characters in the title, one empty note, one with a duplicate title and one ordinary note. If a category does not exist, take the next largest. If the source has fewer than 10 notes, sample all of them.
2. Show the user the 10 file names. Ask them to strike any that are sensitive and replace them. Only then may the agent read them.
3. Convert the sample into a scratch folder, never the real target.
4. Check each note: title, headings and body complete, images shown, links working, tags present, created date kept, no raw HTML where Markdown was expected.
5. Write the mapping for bulk: how titles, tags, dates and attachments will be handled, using steps 7 to 11.
6. Gate 2: show the sample result and the mapping. Wait for a yes. If more than 2 of the 10 fail the same check (default), fix the route and run a fresh sample.

### Step 6: Convert in bulk into a new folder

1. Confirm the target folder is new and empty (or a dated staging folder).
2. If the source has more than 500 notes (a default), convert one top-level folder or notebook at a time and run steps 9 and 10 after each. A failure then costs one folder, not the vault.
3. Gate 3: show the plan (folders, counts) and wait for a yes.
4. Run the conversion. Keep the tool's log and count its warnings.

### Step 7: Fix names and paths

Apply these rules in the target only.

| Problem | Rule |
|---|---|
| Name ends in a 32-character hexadecimal ID (Notion's pattern) | Strip the ID. Before stripping, check that no two files in the same folder would end up with the same name |
| Two notes would share a name | Keep both. Add a suffix such as ` (2)`, and write the original title in a `title` property or the first heading. Never let a copy overwrite another |
| Characters illegal on the user's file system (on Windows: backslash, slash, colon, asterisk, question mark, double quote, angle brackets and pipe; verify before relying on this) | Replace with a hyphen. Record the original title in the `title` property |
| Full path longer than 240 characters (default; Windows ZIP extraction is documented to fail above 260) | Shorten folder names, not note titles |
| Two names differing only by case | Treat as duplicates, because Windows and macOS file systems are usually case-insensitive by default (verify before relying on this). Rename one |

The script flags hexadecimal IDs, duplicate titles and long paths.

### Step 8: Repair links

1. Run the script on the target and compare with the source (step 10 shows the command).
2. Sort every broken link into one of three classes.

| Class | Test | Action |
|---|---|---|
| Already broken in the source | Present in source.json broken list | Leave it. List it for the user |
| Fixable | Exactly one note in the target has that title | Propose the rewrite |
| Ambiguous | Two or more notes share the title | Never guess. List the candidates |

3. Show the proposed rewrites as a before-and-after list in batches of at most 20 files. Gate 4: apply a batch only after a yes for that batch.
4. Rewrite links only into the form the destination uses. Do not mix wikilinks and relative Markdown links in the same vault without the user's say-so.

If the importer left placeholders instead of links (fortrabbit saw `NOTION_PAGE:(UID)` after incremental imports), search for the pattern and treat each as a broken link.

### Step 9: Check attachments

1. Every image and file referenced in a note exists in the target. The script reports missing files as broken links.
2. Files that no note references are listed as unreferenced, not deleted. Some are legitimate (CSV exports kept as data).
3. Images loaded from external URLs may not be in the export. Check images in the sample.
4. Keep attachment placement consistent: one shared attachments folder or one beside each note. Pick one for everything.

### Step 10: Reconcile source against target

Run the script on the target with `--json target.json`, then compare:

```
python3 inventory.py --compare source.json target.json
```

Apply these rules (defaults, unless the user sets stricter ones).

| Measure | Pass rule |
|---|---|
| `notes` | Target equals source minus documented exclusions. Every missing note is named |
| `other_files` | Target equals source, except files the user chose to leave out |
| `links_broken` | Target is no higher than source. Each extra broken link is named and fixed or accepted |
| `links_ambiguous` | Zero, or each one listed for the user |
| `flag:hex_id_name`, `flag:long_path` | Zero in the target |
| `flag:duplicate_title` | Zero in the target, or each pair kept on purpose |

If the target has fewer notes than the source and the gap is undocumented, suspect duplicate titles collapsing into one.

### Step 11: Check dates, tags and titles

1. **Dates.** Check whether created and modified dates survived. If the export metadata holds them, apply them as `YYYY-MM-DD`. If not, say so in the report. Never infer a date.
2. **Time zones.** Record the time zone the conversion assumed. If unknown, ask.
3. **Tags.** Count distinct tags in source and target. A forum report from 2020 describes a Google Keep export writing `tags: - null`, so search the target for empty or null tags. Verify before relying on this for current versions.
4. **Titles.** For every file whose name changed, confirm the original title survives in a property or heading.

### Step 12: Human check and sign-off

1. The user opens 10 notes in the new tool (20 if the vault has more than 2,000 notes; defaults). Pick from the sample categories again, plus three notes the user uses weekly.
2. The user confirms, for each: text, images, links and tags.
3. The agent writes the report and recommends one of three outcomes: accept, fix and re-run named items, or abandon. It does not retire the source. Retiring the source is gate 5 and belongs to the user, after the retention question in guardrail 9.

## Worked example

All data below is invented. The numbers come from running the script in this file on a small test folder built for this example, with the output condensed.

**Situation.** A support team moves its Notion "Support Wiki" to an Obsidian vault. Export route: Notion Markdown and CSV, include subpages. 8 notes, 2 images, 1 CSV table. The user marks links and images as must-survive.

**Step 2, source baseline.** The agent runs the script on the unzipped export:

```
notes 8, other_files 3, links_resolved 8, links_broken 1
flags: hex_id_name 9, duplicate_title 1 (macros x2), empty_note 1
broken: Refund policy -> Legacy (page not in export)
```

The one broken link was already broken in Notion, so it goes on the baseline list.

**Step 3, loss list.** The CSV table loses its views. The user accepts that and keeps the CSV beside the notes.

**Steps 4 to 6.** Obsidian lists Notion as an official import source, so the route is its importer on a copy of the export. All 8 notes serve as the sample and convert cleanly into a scratch folder. The user approves, and the bulk run goes into a new folder.

**Step 10, first reconciliation:**

```
measure             source  target    diff
notes                    8       7      -1
other_files              3       2      -1
links_resolved           8       7      -1
links_broken             1       2      +1
flag:duplicate_title     1       0      -1
flag:hex_id_name         9       0      -9
```

**Reading it.** Seven notes, not eight, and the duplicate-title flag has cleared: two pages were both called Macros and one overwrote the other. One file is missing and the extra broken link is `Billing FAQ -> billing.png`, an image that never arrived. The hexadecimal IDs are gone, as expected.

**Fix, with gate 4.** The agent proposes two changes: restore the second Macros page as `Macros (2).md` with its original title in the first heading, and copy `billing.png` from the source into the attachments folder. The user says yes to both. After the fix:

```
measure             source  target    diff
notes                    8       8      +0
other_files              3       3      +0
links_resolved           8       8      +0
links_broken             1       1      +0
```

**Step 12.** The user opens the notes and confirms them. The report says: 8 of 8 notes, 3 of 3 files, 1 link broken in the source and left alone, dates not preserved (all show the import date). It recommends accept. The source stays untouched for 30 days.

## Templates

### Inventory script

Save as `inventory.py`. It uses only the Python standard library, reads files and writes nothing except the optional JSON file. Read it before running it.

```python
#!/usr/bin/env python3
"""Count notes, files and links in a folder. Read-only.
python3 inventory.py FOLDER [--json out.json] | --compare a.json b.json"""
import json, re, sys, unicodedata
from collections import Counter, defaultdict
from pathlib import Path
from urllib.parse import unquote

HEX_ID = re.compile(r"[ _-][0-9a-f]{32}$")
MD_LINK = re.compile(r"!?\[[^\]]*\]\(([^)\s]+)(?:\s+\"[^\"]*\")?\)")
WIKI = re.compile(r"!?\[\[([^\]|#]+)(?:#[^\]|]*)?(?:\|[^\]]*)?\]\]")

def norm(s):
    return unicodedata.normalize("NFC", s).casefold()

def title(p):
    return norm(HEX_ID.sub("", p.stem))

def scan(root):
    root = Path(root).resolve()
    files = [p for p in root.rglob("*") if p.is_file()]
    notes = [p for p in files if p.suffix.lower() == ".md"]
    others = [p for p in files if p.suffix.lower() != ".md"]
    by_stem = defaultdict(list)
    for n in notes:
        by_stem[title(n)].append(n)
    names = {norm(str(p.relative_to(root))) for p in files}
    by_name = defaultdict(list)
    for p in files:
        by_name[norm(p.name)].append(p)
    out = Counter()
    problems = defaultdict(list)
    referenced = set()
    for n in notes:
        text = n.read_text(encoding="utf-8", errors="replace")
        for target in MD_LINK.findall(text):
            if re.match(r"^[a-z][a-z0-9+.-]*:", target, re.I) or target.startswith("#"):
                continue
            path = unquote(target.split("#")[0])
            dest = norm(str((n.parent / path).resolve().relative_to(root))) if (n.parent / path).resolve().is_relative_to(root) else None
            if dest in names:
                out["links_resolved"] += 1
                referenced.add(dest)
            else:
                out["links_broken"] += 1
                problems["broken"].append(f"{n.relative_to(root)} -> {target}")
        for target in WIKI.findall(text):
            t = target.strip()
            if Path(t).suffix and Path(t).suffix.lower() != ".md":
                hits = by_name.get(norm(Path(t).name), [])
                referenced.update(norm(str(h.relative_to(root))) for h in hits)
            else:
                hits = by_stem.get(norm(Path(t).stem), [])
            if len(hits) == 1:
                out["links_resolved"] += 1
            elif not hits:
                out["links_broken"] += 1
                problems["broken"].append(f"{n.relative_to(root)} -> [[{target}]]")
            else:
                out["links_ambiguous"] += 1
                problems["ambiguous"].append(f"{n.relative_to(root)} -> [[{target}]]")
    for p in files:
        rel = str(p.relative_to(root))
        if HEX_ID.search(p.stem):
            problems["hex_id_name"].append(rel)
        if len(str(p)) > 240:
            problems["long_path"].append(rel)
    for stem, hits in by_stem.items():
        if len(hits) > 1:
            problems["duplicate_title"].append(f"{stem} x{len(hits)}")
    for p in others:
        if norm(str(p.relative_to(root))) not in referenced and p.suffix.lower() not in {".csv"}:
            problems["unreferenced_file"].append(str(p.relative_to(root)))
    for n in notes:
        if n.stat().st_size == 0:
            problems["empty_note"].append(str(n.relative_to(root)))
    return {"notes": len(notes), "other_files": len(others), "bytes": sum(p.stat().st_size for p in files),
            **out, "problems": {k: v[:50] for k, v in problems.items()}, "problem_counts": {k: len(v) for k, v in problems.items()}}

def compare(a, b):
    keys = ["notes", "other_files", "links_resolved", "links_broken", "links_ambiguous"]
    print(f"{'measure':<18}{'source':>8}{'target':>8}{'diff':>8}")
    for k in keys:
        x, y = a.get(k, 0), b.get(k, 0)
        print(f"{k:<18}{x:>8}{y:>8}{y - x:>+8}")
    for k in sorted(set(a["problem_counts"]) | set(b["problem_counts"])):
        x, y = a["problem_counts"].get(k, 0), b["problem_counts"].get(k, 0)
        print(f"{'flag:' + k:<18}{x:>8}{y:>8}{y - x:>+8}")

if __name__ == "__main__":
    if len(sys.argv) == 4 and sys.argv[1] == "--compare":
        compare(json.load(open(sys.argv[2])), json.load(open(sys.argv[3])))
    elif len(sys.argv) >= 2:
        res = scan(sys.argv[1])
        if "--json" in sys.argv:
            json.dump(res, open(sys.argv[sys.argv.index("--json") + 1], "w"), indent=1)
        print(json.dumps({k: v for k, v in res.items() if k != "problems"}, indent=1))
        for k, v in res["problems"].items():
            print(f"\n{k}:"); [print("  " + i) for i in v[:10]]
    else:
        sys.exit(__doc__)
```

### Migration report

```
MIGRATION REPORT: [source] to [destination], [date]

Scope: [notes] notes, [files] files, exclusions: [list or none]
Route: [importer and version, or script name]
Sample: [n] notes, [pass/fail], mapping approved [yes/no]

Reconciliation (source / target):
  notes [ ] / [ ]    other_files [ ] / [ ]
  links_resolved [ ] / [ ]    links_broken [ ] / [ ]    links_ambiguous [ ]
  flags: hex_id_name [ ]  duplicate_title [ ]  long_path [ ]  empty_note [ ]

Named losses: [notes, files, links, or none]
Baseline broken links left alone: [count]
Not checked: [block references, HTML links, comments, history]
Dates preserved: [yes/no/partly]  Tags: [source] / [target]
Human spot check: [n] of [n] passed
Recommendation: [accept / fix named items / abandon]
Source retained until: [date]. Retirement is the user's decision.
```

### Starting prompt

```
Use the note-migration skill. I want to move my notes from [source] to
[destination]. I have [export / no export yet]. Links and images matter most.
Please start with the inventory and do not change anything in the source.
```

## Failure modes and anti-patterns

| # | Failure | How to spot it | What to do instead |
|---|---|---|---|
| 1 | Success declared from a file count | The report says "all notes imported" and has no link or attachment numbers | Run step 10. Compare links and files, and name every difference |
| 2 | Bulk run before a sample | Problems found at note 3,000 | Run step 5. Fix the route on 10 notes |
| 3 | Duplicate titles collapse | Target has fewer notes than source and the gap equals the number of duplicate-title pairs | Check `flag:duplicate_title` first. Rename with a suffix, never overwrite |
| 4 | Silent loss of non-text content | Databases, callouts, comments or drawings are missing and nobody was told | Show the step 3 loss list before converting |
| 5 | Source retired too early | The old subscription is cancelled the day the import finishes | Keep the source 30 days. Notion export links expire after 7 days, so keep the export zip too |

## Quality bar

Check every item before handing over.

1. The user confirmed gate 1 before any file was touched.
2. `source.json` exists and the report quotes its numbers.
3. The loss list was shown before conversion and each "must survive" item was resolved or flagged.
4. A sample of 10 notes (or all, if fewer) was converted into a scratch folder and checked against the list in step 5.
5. The user approved the mapping (gate 2) and the bulk plan (gate 3).
6. The target folder was new and empty, or a dated staging folder.
7. `flag:hex_id_name` and `flag:long_path` are zero in the target.
8. `notes` and `other_files` match the source, or each gap is named.
9. `links_broken` in the target is no higher than in the source, or each extra is named.
10. Every link rewrite was approved in a batch of at most 20 files.
11. The report states what was not checked and whether dates were preserved.
12. No struck note was read, no note body was quoted and no credential was requested.
13. The report recommends the source's retirement and does not execute it.

## Cost and effort

The workflow makes no paid API calls. The agent reads the script output (counts and file names) and the 10 sample notes, never the whole vault, so a large vault costs about the same tokens as a small one. For a small vault the cheaper path is steps 2, 5 and 10 only. Do not skip step 10. Notion exports can take up to 30 hours, so request the export early.

## Handoffs to related skills

`note-system-setup.md` decides where notes live after arrival. `knowledge-base-hygiene.md` finds stale pages before or after the move. `atomic-notes.md` turns migrated captures into permanent notes. `rag-knowledge-bases.md` indexes the finished vault. `ai-security-privacy.md` and `human-in-the-loop.md` cover the data and approval rules used here.

## Appendix: tool-specific notes

Every fact below was read on 22 October 2026. **Verify before relying on this**: menus, limits and plans change.

### Obsidian Importer

Obsidian Help lists these official import sources through the Importer plugin: Notion, Airtable, Microsoft OneNote, Evernote, Apple Notes, Apple Journal, Google Keep, Bear, Craft, Roam Research, Logseq and Tomboy/Gnote, plus HTML, CSV, Markdown and Textbundle files. The page states no import limits, does not say what fails to convert and gives no backup advice, which is why steps 2, 3 and 10 exist. For link syntax, the kepano `obsidian-markdown` skill uses wikilinks for notes inside the vault and standard links for external URLs.

### Notion export

From the Notion help page "Export your content":

- Formats: PDF, HTML, and Markdown and CSV. Full-page databases export as CSV, with Markdown files for each subpage.
- Callouts export as HTML, because Markdown has no equivalent.
- "Include subpages" creates separate files for nested pages.
- Database exports cover the current or default view only. Form views cannot be exported.
- Pages the exporter cannot access, such as another user's private pages, are left out.
- Exports can take up to 30 hours and the download link expires after 7 days.
- On Windows, ZIP files fail when paths exceed 260 characters. Unticking "Create folders for subpages" or using 7-Zip can fix this.

A vendor blog (unmarkdown.com, 25 February 2026) reports 32-character hexadecimal IDs on nested page names, images in per-page subfolders and dropped text colours. The author sells a competing tool, so test these on your sample.

### Evernote export

From the Evernote help article on ENEX and HTML export:

- Export works only in the desktop app for Mac or Windows. The web version cannot export.
- Formats: ENEX, single-page HTML or multi-page HTML.
- Up to 100 notes at a time, or entire notebooks. ENEX files can be capped between 300 MB and 2 GB each.
- Tags, created date and author are optional attributes. Tick them, or they are not in the file.
- The article does not say whether internal links or history are preserved. Test an export first.

### Moving into Notion

From Notion's Confluence migration guide: the API importer moves a whole space (under 15 GB) and keeps comments. The zip importer takes selected pages (under 5 GB in total) and skips users and comments. Confluence Expand becomes Toggle and Panels become Callouts. Gliffy and Draw.io content is not connected, so export those drawings as images first. The page does not cover macros or permissions.

### Claude Agent Skills

Anthropic limits `name` to 64 characters and `description` to 1,024. Claude Code documents `disable-model-invocation: true` for skills with side effects, so they run only when the user calls them. This skill writes files, so that setting suits it. Install skills only from sources you trust.

## Sources

Last verified 22 October 2026 for every source below.

- Obsidian Help, Import notes: https://obsidian.md/help/import. Import sources.
- Notion Help, Export your content: https://www.notion.com/help/export-your-content. Formats, views, time and path limits.
- Notion Help, Migrate from Confluence to Notion: https://www.notion.com/zh-cn/help/guides/migrate-from-confluence-to-notion. Size limits, element mapping.
- Evernote Help, Export notes and notebooks as ENEX or HTML: https://help.evernote.com/hc/en-us/articles/209005557-Export-notes-and-notebooks-as-ENEX-or-HTML. Desktop-only export, limits.
- Anthropic, Agent Skills best practices: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices. Frontmatter limits.
- Claude Code documentation, Skills: https://code.claude.com/docs/en/skills. `disable-model-invocation`.
- garrytan, gbrain, skills/migrate/SKILL.md: https://github.com/garrytan/gbrain. Additive-only contract, 5 to 10 file sample.
- jeremylongshore, obsidian-migration-deep-dive (via skills.sh): https://www.skills.sh/jeremylongshore/claude-code-plugins-plus-skills/obsidian-migration-deep-dive. Backup-only safeguard. Excerpt only.
- jeremylongshore, apple-notes-migration-deep-dive (via openskillindex): https://openskillindex.com/skills/comeonoliver-skillshub-apple-notes-migration-deep-dive. Script approach. Excerpt only.
- kepano, obsidian-skills, obsidian-markdown: https://github.com/kepano/obsidian-skills. Wikilink convention.
- fortrabbit, Moving our knowledge base from Notion to Markdown (11 June 2026): https://blog.fortrabbit.com/moving-knowledge-base-from-notion-to-markdown. Broken links, placeholders.
- Unmarkdown, Notion export broken (25 February 2026): https://unmarkdown.com/blog/notion-export-broken. Vendor claims.
- Obsidian Forum, Meta: Migration Workflows (March 2021, comments from 2020): https://forum.obsidian.md/t/meta-migration-workflows/15252. Null tags report. Dated.
- dmuth.org, Evernote to Obsidian migration, part 1 (28 March 2021): https://dmuth.org/2021/03. About 3,000 notes moved. Dated.

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/note-migration/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.
