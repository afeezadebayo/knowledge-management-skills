---
name: knowledge-base-hygiene
description: "Audits a wiki, shared drive or notes vault for stale, ownerless, duplicate and broken pages, then produces a ranked review plan with an owner and a decision for each flagged page. Classifies pages by review class, runs a short standard-library script over a page-list export, proposes archive before delete and never deletes. Notes the verified limits of Notion verified pages and Confluence archiving. Use whenever someone asks to clean up a wiki, find outdated docs, set review dates, assign page owners, audit a knowledge base, archive old pages or fix broken internal links. Do NOT use for processing new captures into notes (use atomic-notes), choosing a folder structure (use note-system-setup), writing SOPs (use sops-process-docs) or building a retrieval index for AI (use rag-knowledge-bases)."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/knowledge-base-hygiene/
  category: productivity
  subcategory: knowledge-management
  version: "1.0.0"
  created: "2026-10-22"
  updated: "2026-10-22"
---

# Knowledge Base Hygiene: Find Stale, Ownerless and Broken Pages, Then Decide What to Do

A knowledge base rarely fails on day one. It fails slowly: a page nobody owns, a process that changed last spring, two pages that answer the same question differently. This skill audits a bounded set of pages, flags the ones that need a human decision, and writes a review plan. It works on a wiki, a shared drive, a help centre or a notes vault, because it only needs a list of pages and a few facts about each.

Two choices shape it. The agent recommends and the owner decides: nothing is archived, moved or deleted by the agent. And every threshold in the workflow is a starting default that the user can overrule, because the review-cadence figures found while researching this skill come from forum opinion and vendor material, not from research.

## When to use this skill, and when not to

Use it when the user wants to:

- Clean up a wiki or shared drive that has grown past what anyone remembers.
- Find outdated pages before a migration, an audit or a new-hire onboarding.
- Set owners and review dates on pages that have neither.
- Decide what to archive.
- Find duplicates and broken internal links.

Do not use it for:

- Turning new captures into permanent notes. Use `atomic-notes`.
- Choosing a folder or tag structure. Use `note-system-setup`.
- Writing or rewriting a standard operating procedure. Use `sops-process-docs`.
- Building a retrieval index for an AI system. Use `rag-knowledge-bases`.
- Checking whether a factual claim in a page is true. Use `fact-checking`. This skill checks age, ownership and structure, and does not judge accuracy.
- Legal, regulatory or employment decisions about records. If a page is a policy, contract, HR record or compliance document, the agent flags it and a person with authority decides. Retention periods are a legal question and this skill gives no advice on them.

## Guardrails that apply to every run

1. **Recommend, never act.** The agent produces a plan. It does not archive, move, restrict, edit or delete a page, and it does not message owners. Drafts of owner messages are allowed and are never sent by the agent.
2. **Archive before delete.** The strongest action the skill proposes is archiving, and only when the owner or a named delegate confirms. Deletion is out of scope. If the user asks for it, state that it needs a separate human decision and check the platform's own delete rules first.
3. **No fabricated figures.** Every number in the report comes from the export or from a cited source. The review intervals in this file are labelled defaults. A figure that depends on a product version carries the marker `verify before relying on this`.
4. **Personal and sensitive data.** The export contains page titles, URLs, owner names or emails, edit dates and view counts. These are personal data about staff. Work on the minimum columns, replace emails with initials or a staff ID if the user prefers, keep the export on the machine where the agent runs, and never paste it into an external service. Never read page bodies unless the user asks for a content check on named pages.
5. **Access before archive.** Archiving can change who sees a page. Before proposing any archive, ask whether the page has restricted access and whether archiving would widen it. Confluence documents this risk for inherited permissions (see the appendix).
6. **Not advice.** Retention, record-keeping and policy review have legal, regulatory and employment dimensions. This skill is not legal or HR advice. Route those pages to a person responsible for them.
7. **Scope boundary.** The skill reports and recommends. It does not change a platform's settings, permissions or automation.

## Inputs the agent must have before starting

Ask for what is missing in one message and wait.

| # | Input | What to ask if missing |
|---|---|---|
| 1 | Scope | "Which space, folder or vault, and roughly how many pages? If more than 300, which section first?" |
| 2 | Page list | "Can you export the page list with title, URL, owner, last edited date and, if you have it, views in the last 90 days?" |
| 3 | Platform | "Which tool holds the pages? Notion, Confluence, Google Drive, SharePoint, Obsidian or something else?" |
| 4 | Review classes | "Which pages are policy or compliance, which are operational processes, which are time-bound projects, and which are stable reference?" |
| 5 | People who have left | "Which owners have left the organisation or changed teams?" |
| 6 | Decision maker | "Who can approve an archive for each area if the owner cannot?" |

If the platform has no export, ask the user to paste a table. If the export has no view counts, the skill skips the low-views flag and says so in the report.

## Workflow

### Step 0: Check the data and the scope

1. Count the pages. Above 300, audit one section at a time. A large audit that cannot be reviewed is worse than a small one that is.
2. Confirm the columns: `id`, `title`, `url`, `owner`, `last_edited`, `views_90d` (may be empty), `class` and `links_to`. `links_to` is a semicolon-separated list of the ids a page links to. If the platform cannot export links, skip the link flags.
3. Check the date format. All dates must be `YYYY-MM-DD`. Convert others and tell the user.
4. Tell the user what will be done with the data (see guardrail 4) before reading it.

### Step 1: Assign a review class to every page

Pages age at different speeds, so one rule for all of them flags the wrong pages. Use four classes and record the choice in the `class` column.

| Class | What it covers | Default review interval |
|---|---|---|
| `compliance` | Policies, procedures an auditor or regulator may ask for | 90 days |
| `operational` | Runbooks, how-tos, escalation paths, tool guides that change with the tool | 30 days |
| `time-sensitive` | Project plans, launch pages, rotas, anything with an end date | 30 days |
| `stable` | Reference material, definitions, principles, templates | 180 days |

These intervals are defaults drawn from an Atlassian Community discussion in which contributors suggested monthly review for time-sensitive content, quarterly for compliance content, and 180 days for stable reference against 30 days for operational content. It is practitioner opinion, and some contributors promoted paid apps, so treat the numbers as a place to start. If the user has an existing policy, use theirs.

When a page does not fit a class, ask the owner. If no owner is available, use `stable` and mark the page `class guessed` in the report.

A useful check on the classes is the Diátaxis split between how-to guides, reference, tutorials and explanation. How-to guides tend to be `operational` and reference tends to be `stable`. Diátaxis does not give review intervals, so use it only to sort pages.

### Step 2: Run the audit script

Save the script in the Appendix as `kb_audit.py`. It uses only the Python standard library, reads the CSV, and prints one line per flagged page. Run it with the date of the audit so that the result is repeatable:

```
python3 kb_audit.py pages.csv --today 2026-10-22 --departed sam
```

The script raises these flags:

| Flag | Meaning |
|---|---|
| `NO_OWNER` | The owner field is empty |
| `OWNER_LEFT` | The owner is in the `--departed` list |
| `OVERDUE(Nd>Md)` | Days since last edit exceed the class interval |
| `LOW_VIEWS(n)` | Views in 90 days are at or below the threshold (default 2). Skipped when the column is empty |
| `DUP_TITLE` | Another page has the same title after case and punctuation are removed |
| `BROKEN_LINK(id)` | The page links to an id that is not in the export |
| `ORPHAN` | No other page in the export links to this page |

Last edited is a weak signal. A page can be edited for a typo and still be wrong, and a page can be untouched and still correct. Treat `OVERDUE` as a prompt for a human to look, not as a verdict.

If the agent cannot run code, apply the same rules by hand to a table of at most 50 rows and say that the check was manual.

### Step 3: Triage by the rules below

The script sorts its output. Confirm or adjust each row with these rules.

1. **Archive candidate.** The page is overdue and has low views, and its class is not `compliance`. The recommendation is to archive after the owner confirms. A `compliance` page is never an archive candidate on views alone, because an auditor may need a page nobody reads.
2. **Assign owner.** The page has no owner or its owner left. Propose the team lead of the area or the person who last edited it as a candidate, and ask the decision maker from input 6 to confirm.
3. **Review.** The page is overdue but has traffic. Ask the owner to read it and either confirm, update or mark it for retirement.
4. **Merge check.** Two pages share a title. Ask the owner of each to pick which stays. Show what differs. Do not merge automatically.
5. **Fix links.** A link points to a missing id. List the source page and the missing target. The owner of the source page fixes it.

A page can match more than one rule. Take the first that applies, in the order above, and list the other flags in the same row.

### Step 4: Check the pages before recommending an archive

For each archive candidate, answer four questions from the page list or by asking:

1. Do other pages link to it? Archived pages may still be reached through links on some platforms, so a page with inbound links needs the linking pages checked first.
2. Is the page restricted, or does it sit under a restricted parent? If yes, flag the access risk.
3. Does anything outside the knowledge base depend on it, such as a support macro, an onboarding email or an audit checklist? Ask the owner.
4. Is there a replacement page? If yes, note the URL so that the archived page can point to it.

If any answer is unknown, the recommendation changes from `archive` to `ask owner first`.

### Step 5: Write the review plan

Give each flagged page one row: id, title, class, flags, recommended action, who decides, and a due date. Group by decision maker so that each person receives one list, not twenty messages. Limit the first round to 25 pages per decision maker. A longer list does not get done.

Draft one message per decision maker using the template below. Present the drafts to the user and do not send them.

### Step 6: Set up the ongoing record

Propose three fields on every page, so that the next audit is quicker: `owner`, `review class` and `next review date`. Where the platform supports verification or review reminders, say which (see the appendix) and note the plan limits. Where it does not, a Page Properties table or a spreadsheet works.

A calendar interval and a trigger work well together. Besides the date, name events that force a review of a page, such as a tool change, a policy change, a reorganisation or an owner leaving.

### Step 7: Report

Give the user a short report with the counts, the plan, the assumptions and anything the agent could not check. Use the report template. State plainly that no page was changed.

## Worked example

The data below is invented. The audit date is 22 October 2026. The agent has the ten-page export, learns that `sam` has left, and runs the script.

Input file `pages.csv`:

```
id,title,url,owner,last_edited,views_90d,class,links_to
p01,Refund policy,/refunds,dana,2026-08-30,140,compliance,p02
p02,Escalation matrix,/escalation,,2026-02-01,35,operational,p01;p09
p03,Onboarding checklist,/onboarding,lee,2026-10-05,60,operational,p04
p04,Tool logins guide,/logins,sam,2025-11-12,1,stable,
p05,Q3 launch plan,/q3-launch,sam,2026-06-20,0,time-sensitive,p03
p06,Onboarding Checklist,/onboarding-old,lee,2026-03-14,4,operational,p03
p07,Brand voice,/voice,priya,2026-05-02,88,stable,p03
p08,Holiday rota 2025,/rota-2025,,2025-12-01,0,time-sensitive,
p09,Support SLA targets,/sla,dana,2026-09-20,52,compliance,p99
p10,Meeting notes template,/mn,priya,2026-04-01,,stable,p07
```

Command and output, produced by running the script in the Appendix:

```
python3 kb_audit.py pages.csv --today 2026-10-22 --departed sam
audited 10 pages on 2026-10-22; flagged 8
p04 | Tool logins guide | stable | ARCHIVE-CANDIDATE (owner confirms) | OWNER_LEFT OVERDUE(344d>180d) LOW_VIEWS(1)
p05 | Q3 launch plan | time-sensitive | ARCHIVE-CANDIDATE (owner confirms) | OWNER_LEFT OVERDUE(124d>30d) LOW_VIEWS(0) ORPHAN
p08 | Holiday rota 2025 | time-sensitive | ARCHIVE-CANDIDATE (owner confirms) | NO_OWNER OVERDUE(325d>30d) LOW_VIEWS(0) ORPHAN
p02 | Escalation matrix | operational | ASSIGN-OWNER | NO_OWNER OVERDUE(263d>30d)
p06 | Onboarding Checklist | operational | REVIEW | OVERDUE(222d>30d) DUP_TITLE ORPHAN
p10 | Meeting notes template | stable | REVIEW | OVERDUE(204d>180d) ORPHAN
p03 | Onboarding checklist | operational | MERGE-CHECK | DUP_TITLE
p09 | Support SLA targets | compliance | FIX-LINKS | BROKEN_LINK(p99)
```

How the agent reads it:

- `p04` (tool logins guide, stable, 344 days old, one view, owner left) and `p05` (Q3 launch plan, time-sensitive, 124 days old, no views, owner left) are archive candidates. Neither owner can confirm, so the agent asks the decision maker for the platform team. `p05` has no inbound links. `p04` is linked from `p03`, so the agent asks whether the onboarding checklist still needs it and whether logins are referenced in onboarding emails before recommending anything.
- `p08` (holiday rota 2025) has no owner, no views, no inbound links and an end date long past. The agent proposes archive and asks who owns the rota process.
- `p02` (escalation matrix) has no owner and was last edited 263 days ago against a 30-day interval, but it has 35 views and `p01` links to it. The problem is ownership, so it goes to the support lead as an assign-owner item, not an archive.
- `p03` and `p06` share a title. `p03` was edited 17 days ago and has inbound links. `p06` is 222 days old and nothing links to it. The agent shows both and asks the owner, `lee`, which stays.
- `p09` (support SLA targets) is compliance, edited a month ago and busy. The only flag is a broken link to `p99`. The agent lists the fix and does not recommend archive.
- `p10` has no views column, so the low-views check was skipped. It is 204 days old against a 180-day interval and nothing links to it. It goes to `priya` for review.
- `p01` and `p07` are not flagged and are left alone.

Summary the agent reports: 10 pages audited, 8 flagged, 3 archive candidates pending owner confirmation, 1 needs an owner, 3 reviews or merge checks, 1 link fix. Nothing was changed.

## Templates

### Owner message (draft, not sent)

```
Subject: Review needed: {N} pages in {area}

Hi {name},

I ran a page audit on {area} on {date}. These pages are listed under you or your team and need a decision:

{id} | {title} | {flag summary} | suggested: {action}

For each page, please reply with one of: keep as is, update by {date}, archive, or reassign to {name}.
Archiving keeps the page and removes it from the main tree and search. Nothing is deleted.

If you are not the right person, tell me who is.
```

### Review plan row

| Page | Class | Flags | Suggested action | Decides | Due |
|---|---|---|---|---|---|
| {id} {title} | {class} | {flags} | {action} | {name} | {date} |

### Report

```
Knowledge base audit: {scope}, {date}
Pages audited: {n}. Flagged: {n}. Class guessed: {n}.
Archive candidates (owner to confirm): {n}
Ownerless or owner left: {n}
Reviews due: {n}. Merge checks: {n}. Broken links: {n}.
Checks skipped: {list, for example views column empty}.
Changes made by the agent: none.
Drafts ready for you: {n} owner messages.
Needs a human decision: {list of compliance or restricted pages}.
```

## Failure modes and anti-patterns

| # | Failure | How it shows up | Fix |
|---|---|---|---|
| 1 | **Treating edit date as accuracy.** A recently edited page is assumed correct and an old one assumed wrong. | Pages with typo edits pass; stable references are flagged every audit. | Treat the date as a prompt to look. Use classes so stable pages are judged on a longer interval. |
| 2 | **Archiving on views alone.** Rarely read pages are retired, including the ones an auditor needs. | A compliance page is archived and cannot be found at the next audit. | Never recommend archive for `compliance` on views alone. Ask the owner first. |
| 3 | **One owner for everything.** A single curator is named for the whole base. | Flags pile up with one person and nothing moves. | Give each area its own decision maker. Separate the author from the accountable owner where they differ. |
| 4 | **Archive widens access.** A restricted page is archived and becomes visible to more people. | An HR or finance page appears in a wider audience. | Check restrictions and inheritance before any archive proposal (guardrail 5). |
| 5 | **The audit that never ends.** The first round lists hundreds of pages. | No owner replies and the list is ignored. | Cap the first round at 25 pages per decision maker and schedule the next round. |

## Quality bar

A run is complete only if every item below holds. Check each one and say which pass.

1. The report states how many pages were audited and how many flagged, and the counts match the script output.
2. Every flagged page has a recommended action and a named decision maker.
3. No page in the `compliance` class is an archive candidate on views alone.
4. Every archive candidate has an answer for inbound links, restrictions, outside dependencies and a replacement, or is marked `ask owner first`.
5. The report lists every check that was skipped and why.
6. Review intervals are labelled as defaults or as the user's own policy.
7. No page body was read unless the user asked for a content check, and the report says so.
8. The report states that no page was changed, moved, archived or deleted by the agent.
9. Owner messages are drafts and are marked as not sent.
10. First-round lists have no more than 25 pages per decision maker.
11. Personal data in the export was reduced to the columns used and was not sent to an external service.

## Cost and effort

The script reads one CSV and runs in a fraction of a second for thousands of rows. The effort is human: expect each owner decision to take a few minutes per page, so a 25-page list is a short task and a 300-page list is not. If the agent reads page bodies for a content check, cost grows with page length, so limit this to named pages. Web or API access to a platform is not needed for the audit itself. If the user connects a platform API for the export, check the platform's rate limits and plan terms first.

## Handoffs to related skills

- `atomic-notes`: for turning the useful ideas in a retiring page into permanent notes before it is archived.
- `note-system-setup`: when the audit shows that the structure, not the pages, is the problem.
- `sops-process-docs`: when an operational page needs to be rewritten as a procedure.
- `rag-knowledge-bases`: when the pages feed an AI assistant and stale content is producing wrong answers.
- `fact-checking`: when a page is current by date but suspected wrong.

## Appendix: tool-specific notes

This appendix is fenced off so that the steps above stay tool-agnostic. Every item was read on 22 October 2026. Platform features change, so `verify before relying on this`.

### The audit script

```python
import csv, sys, re, argparse
from datetime import date, datetime
INTERVAL = {"stable": 180, "compliance": 90, "operational": 30, "time-sensitive": 30}
ap = argparse.ArgumentParser()
ap.add_argument("csv"); ap.add_argument("--today", required=True)
ap.add_argument("--departed", default=""); ap.add_argument("--low-views", type=int, default=2)
a = ap.parse_args()
today = datetime.strptime(a.today, "%Y-%m-%d").date()
gone = {x.strip().lower() for x in a.departed.split(",") if x.strip()}
rows = list(csv.DictReader(open(a.csv, encoding="utf-8")))
ids = {r["id"] for r in rows}
norm = lambda t: re.sub(r"[^a-z0-9]+", " ", t.lower()).strip()
titles = {}
for r in rows: titles.setdefault(norm(r["title"]), []).append(r["id"])
inbound = {i: 0 for i in ids}
for r in rows:
    for t in filter(None, r["links_to"].split(";")):
        if t in inbound: inbound[t] += 1
out = []
for r in rows:
    f = []
    own = r["owner"].strip().lower()
    if not own: f.append("NO_OWNER")
    elif own in gone: f.append("OWNER_LEFT")
    cls = r["class"] if r["class"] in INTERVAL else "stable"
    age = (today - datetime.strptime(r["last_edited"], "%Y-%m-%d").date()).days
    if age > INTERVAL[cls]: f.append(f"OVERDUE({age}d>{INTERVAL[cls]}d)")
    v = r["views_90d"].strip()
    if v != "" and int(v) <= a.low_views: f.append(f"LOW_VIEWS({v})")
    if len(titles[norm(r["title"])]) > 1: f.append("DUP_TITLE")
    bad = [t for t in filter(None, r["links_to"].split(";")) if t not in ids]
    if bad: f.append("BROKEN_LINK(" + ",".join(bad) + ")")
    if inbound[r["id"]] == 0: f.append("ORPHAN")
    if not f: continue
    overdue = any(x.startswith("OVERDUE") for x in f); low = any(x.startswith("LOW_VIEWS") for x in f)
    if overdue and low and cls != "compliance": act = "ARCHIVE-CANDIDATE (owner confirms)"
    elif any(x in ("NO_OWNER", "OWNER_LEFT") for x in f): act = "ASSIGN-OWNER"
    elif overdue: act = "REVIEW"
    elif "DUP_TITLE" in f: act = "MERGE-CHECK"
    else: act = "FIX-LINKS"
    out.append((r["id"], r["title"], cls, act, " ".join(f)))
rank = {"ARCHIVE-CANDIDATE (owner confirms)": 0, "ASSIGN-OWNER": 1, "REVIEW": 2, "MERGE-CHECK": 3, "FIX-LINKS": 4}
out.sort(key=lambda x: (rank[x[3]], x[0]))
print(f"audited {len(rows)} pages on {today}; flagged {len(out)}")
for o in out: print(" | ".join(o))
```

Sample file and run are shown in the worked example. The script flags and sorts. It does not move or change any file.

### Notion

- Verification of individual pages and database pages requires the Business or Enterprise plan. The help page does not say whether wiki verification has the same restriction.
- A verified page needs at least one owner. The creator of a page in a wiki is its default owner, and owners can be changed at any time.
- A page can be verified until a set time or indefinitely. At expiry, the owner is notified in the Notion inbox and by email to re-verify. The help page gives no fixed durations.
- To verify you need "Can edit" or "Full access". Settings, then Verified pages, lets you search and filter by owner or teamspace.
- Adding a Verification property to a database also adds an Owner property.
- Only pages can become wikis. Databases cannot.

Source: https://www.notion.com/help/wikis-and-verified-pages

### Confluence Cloud

- Archiving removes an item from the content tree and stores it in that space's archive. The documentation says it "does not fully delete your content".
- Archived items are left out of quick search and analytics, although advanced search can include them. Links to archived items still work and show an "Archived" lozenge. Editing, commenting and liking are disabled.
- Each space has its own archive. There is no site-wide archive.
- Archiving needs the space-level Archive permission, which only space admins can assign. Anyone can archive items they created. Anyone with space access can view archived content, subject to page restrictions.
- Check inherited permissions first, because archiving can expose an item to a wider audience.
- Nested archiving is all or nothing.
- Space admins on Premium or Enterprise can bulk archive up to 500 items per attempt. Bulk deletion is capped at 100 items.
- An Atlassian Community reply recommends the Content Manager on Premium to find pages with no active owner and reassign them in bulk. That is a user suggestion, not documentation.

Sources: https://support.atlassian.com/confluence-cloud/docs/archive-pages/ and https://community.atlassian.com/forums/Confluence-questions/content-governance-determining-ownership-and-expected-lifespan/qaa-p/3259716

### Other platforms

Google Drive, SharePoint, Obsidian and help-centre tools were not verified in this version of the skill. Use the tool-agnostic steps, build the page list by hand or with the platform's own export, and check the product's documentation for archive and permission behaviour before proposing any action.

### Claude Agent Skills

`name` is limited to 64 characters and `description` to 1,024. Only metadata loads at start-up. Anthropic advises installing skills only from trusted sources. Verify limits in the Anthropic documentation.

## Sources

Last verified 22 October 2026 for every source below.

- Notion Help, Wikis and verified pages: https://www.notion.com/help/wikis-and-verified-pages. Owners, verification plans, expiry behaviour.
- Atlassian Support, Archive content items (Confluence Cloud): https://support.atlassian.com/confluence-cloud/docs/archive-pages/. Archive behaviour, permissions, bulk caps.
- Atlassian Community, content governance, ownership and expected lifespan: https://community.atlassian.com/forums/Confluence-questions/content-governance-determining-ownership-and-expected-lifespan/qaa-p/3259716. Practitioner opinion on owner versus author and on review intervals. Some contributors promoted paid Marketplace apps.
- Diátaxis, Start here: https://diataxis.fr/start-here/. Four kinds of documentation, used here to sort pages into classes.
- Anthropic, Agent Skills overview: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview. Frontmatter limits and security advice.

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/knowledge-base-hygiene/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.
