---
name: decision-records
description: "Writes, lints, supersedes and reviews short decision records (the ADR pattern, for any team, not only software) so the reason behind a significant decision outlives the people who made it. Gates whether a decision needs a record, drafts each one as proposed with sourced context, real options and honest consequences, backfills past decisions without inventing reasons, and lints the folder for numbering, statuses, two-way supersede links, stale proposals, overdue reviews and leaked secrets. Never sets accepted, edits an accepted record or publishes. Use whenever someone asks to write an ADR or decision record, log why a tool, vendor or policy was chosen, backfill a past decision, supersede or review old decisions, or keep a decision log. Do NOT use for meeting minutes (use meeting-notes-actions), running a decision meeting (use decision-meetings), post-project reviews (use lessons-learned), stale wiki pages (use knowledge-base-hygiene) or decisions about named individuals such as hiring, pay or discipline."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/decision-records/
  category: productivity
  subcategory: knowledge-management
  version: "1.0.0"
  created: "2026-10-23"
  updated: "2026-10-23"
---

# Decision Records: Capture Why a Decision Was Made Before the People Who Made It Move On

Most teams can say what they chose and cannot say why. The reason usually lives in one person's head or in a chat thread nobody can find. A decision record is a short file that holds the reason, the options that lost and the consequences the team accepted. It is the architecture decision record (ADR) pattern, widened so that an operations lead or a founder can use it as readily as an engineer.

This skill drafts, checks, links and reviews those records. It has two firm limits. The agent only ever writes a record as `proposed`, and a named person sets it to `accepted`. And the agent never writes a reason it was not given: every "because" traces to a source the user supplied, or it is marked `unknown` and handed back as a question.

## When to use this skill, and when not to

Use it when the user wants to:

- Write a record for a decision that was just made or is about to be made.
- Decide whether a decision deserves a record at all.
- Backfill a decision from a chat thread, meeting notes or an email.
- Supersede an old decision with a new one, or review the decisions that are due.
- Check a folder of records for gaps, one-way links, stale proposals and secrets.

Do not use it for:

- Minutes or action items from a meeting. Use `meeting-notes-actions`.
- Running the meeting where the decision is made. Use `decision-meetings`.
- Learning from a project or incident after the event. Use `lessons-learned`.
- Scope, goals and stakeholders of a piece of work. Use `project-briefs`.
- Finding stale or ownerless pages in a wiki. Use `knowledge-base-hygiene`.
- Choosing where records live. Use `note-system-setup`.
- Making the decision. The agent can lay out options and gaps. It does not choose.
- Decisions about a named person, such as hiring, pay, promotion, discipline or performance. These belong with HR or the line manager, and a decision record is widely read and long lived.
- Legal, regulatory, safety or financial sign-off. A record can note that such a decision was made, and a person with authority makes it. This skill is not legal, financial or HR advice, and it gives no advice on how long records must be kept.

## Guardrails that apply to every run

1. **Draft only.** Every record the agent writes has `status: proposed`. Only a human sets `accepted`, `rejected` or `deprecated`. The agent does not commit, publish, post to a wiki or message anyone. Drafts of messages are allowed and are never sent.
2. **Accepted records are not rewritten.** The body of an accepted record is never edited. A changed decision gets a new record. The only edits to an older record are its `status`, its `superseded_by` line and an append-only `Review log`, and each needs the user's confirmation first.
3. **No invented reasons.** Context, options and rationale come from sources the user supplied, and each source is named in the record. Anything missing is written as `unknown, ask {name}`. The agent does not infer motives.
4. **Sensitive data stays out.** Records last for years and many people read them. Do not put in passwords, keys, tokens, personal data, contract prices under confidentiality, security details or comments about individuals. Write a pointer instead, such as "see the signed contract in the finance folder". The lint script reports the line number of a suspicious pattern and never prints the value.
5. **No people decisions.** If the decision concerns a named individual's employment, stop and route it to the right person.
6. **Not advice.** Where a decision touches money, law, regulation or safety, the record says who with authority approved it, and the agent does not assess whether the approval was right.
7. **Figures are sourced.** A number in a record comes from the user or a cited source with a date. Anything that depends on a product version gets the marker `verify before relying on this`.
8. **Scope boundary.** The agent refuses to backdate a record, present a reconstructed record as contemporaneous, renumber, delete or bulk-rewrite records, or decide on the team's behalf.

## Inputs the agent must have before starting

Ask for what is missing in one message and wait. Do not proceed on assumptions.

| # | Input | What to ask if missing |
|---|---|---|
| 1 | The decision | "In one sentence, what was decided, or what has to be decided?" |
| 2 | Deciders and consulted | "Who decided, and who was consulted? Names or roles are fine." |
| 3 | Source material | "Where did the discussion happen? Paste the thread or notes, or give the file path." |
| 4 | Options considered | "Which alternatives were seriously discussed, including doing nothing?" |
| 5 | Where records live | "Which folder or tool holds your records, and what is the highest number so far?" |
| 6 | Decision state | "Is this already decided (and when), or still open?" |
| 7 | Cost of reversing | "If this turned out wrong, how long and how costly would it be to undo?" |

If the user has no records folder, propose `docs/decisions/` in a repository or a `Decisions` folder in a notes vault, and ask before creating it.

## Workflow

### Step 0: Confirm scope, sensitivity and location

1. State what you will do with the material: read the supplied sources, draft one record, run a local script, and change nothing else.
2. Ask whether the sources contain anything from the guardrail 4 list. If they do, work from a redacted copy.
3. Confirm the folder and the next number. Take the highest number in the folder, including `rejected`, `deprecated` and `superseded` records, and add one.

### Step 1: Run the gate. Does this decision need a record?

Score five triggers, each yes or no.

| # | Trigger |
|---|---|
| 1 | Hard to reverse: undoing it would take more than one working week or cost more than a figure the user names |
| 2 | Reach: it affects people or teams outside the group that decided |
| 3 | Standard: it sets a rule or default others must follow |
| 4 | Newcomer test: a person joining in a year would plausibly ask "why do we do it this way?" |
| 5 | Contested: people disagreed, or a rejected option had a strong advocate |

Decision rules:

- Two or more yes: write a standard record (Step 2).
- Exactly one yes: write a one-line Y-statement for the index (template below) and offer a full record.
- No yes: say that no record is needed and stop.

These triggers are this skill's defaults, drawn from Hidekazu Konishi (hard to reverse, spans components), the wshobson skill (a list of what counts and what to skip) and the GOV.UK framework (impact beyond the team). None of them publishes a scoring rule, so the user can change the thresholds.

### Step 2: Pick the depth

- **Y-statement:** one sentence, about ninety seconds to write (Konishi's estimate, anecdotal).
- **Standard record:** the template below, one to two pages. The lint script flags a body above 1,000 words by default, this skill's own conversion of that guidance at about 500 words a page. Put long comparisons in a linked document.

### Step 3: Collect and attribute the evidence

1. List every claim the record will make, with the source of each (a person, a message, a document, a date).
2. List every gap as `unknown, ask {name}`.
3. Ask at most five questions to close the gaps that matter. Do not ask for what the sources already say.

### Step 4: Write the options

1. Write at least two options. Include "keep things as they are" when that was a real choice.
2. Give each option one line on why it was attractive and one on why it was set aside or chosen, in the deciders' own reasons.
3. Apply the strawman test to every rejected option: could a decider have argued for it in good faith? If not, remove it or mark it `not seriously considered`.

### Step 5: Write context, decision and consequences

- **Context:** the forces and constraints in roughly three to six sentences, the range suggested by the mohitagw15856 skill. State facts, with sources. Leave out advocacy.
- **Decision:** one to three sentences in the active voice, beginning "We will".
- **Consequences:** at least one good and one bad line, written as `- Good:` and `- Bad:`. A record with only upsides is a pitch, and the lint script flags it.

### Step 6: Write the check and the review date

1. **How we will check:** one observable measure with a threshold and a time, for example "missed-reply count stays at or below last quarter's level for three months". Christoph Kappel reported about 60 records with no fitness function, so adherence could not be tested.
2. **review_by:** a date. Default twelve months, or sooner if a named event would force a rethink, such as a contract end or a team change. This default is the skill's own.

### Step 7: Number, name and place the file

Use `nnnn-kebab-title.md` (MADR's convention is `nnnn-title.md`). Numbers are sequential and never reused. The first record may be the decision to keep records, numbered `0001` here. Konishi numbers it `0000`, so pick one and keep it.

### Step 8: Lint

Save the script in the Appendix as `decision_lint.py` and run it on the folder:

```
python3 decision_lint.py docs/decisions --today 2026-10-23
```

Fix every flag in the new record. For flags on older records, report them and do not edit those records. If the agent cannot run code, check the same list by hand and say the check was manual.

| Flag | Meaning |
|---|---|
| `BAD_FILENAME` | Name does not match `nnnn-kebab-title.md` |
| `MISSING_FIELD(x)` | `status`, `date` or `deciders` is empty |
| `BAD_DATE`, `BAD_REVIEW_BY` | A date is not in `YYYY-MM-DD` form |
| `BAD_STATUS(x)` | Status is not one of proposed, accepted, rejected, deprecated, superseded |
| `MISSING_SECTION(x)` | A required heading is missing or empty |
| `FEW_OPTIONS(n)` | Fewer than two options |
| `ONE_SIDED_CONSEQUENCES` | No `Good` or no `Bad` line |
| `LONG(nw>mw)` | Body longer than the limit |
| `STALE_PROPOSED(nd)` | Proposed for more than 30 days (default) |
| `NO_REVIEW_BY`, `REVIEW_OVERDUE(date)` | An accepted record has no review date or is past it |
| `NUMBER_GAP` | A number is missing from the sequence |
| `SUPERSEDE_ONE_WAY`, `SUPERSEDED_BY_ONE_WAY`, `SUPERSEDED_NO_POINTER`, `SUPERSEDES_MISSING` | The supersede links do not agree in both directions |
| `RECONSTRUCTED_NO_SOURCES` | A reconstructed record names no source and no unknowns |
| `SENSITIVE_PATTERN(line n)` | An email address, key or password-like pattern is on that line |

### Step 9: Hand over for a human decision

Present the draft, the unknowns and the lint result. Tell the user what only a person can do: confirm the facts, circulate the draft, hold the review, and set the status. Konishi suggests sending the draft 24 to 48 hours before a review meeting and allowing 20 to 40 minutes for discussion. That comes from one author's experience, so treat it as a suggestion. After the user confirms `accepted`, the agent may update an index file if one exists.

### Step 10: Supersede, never rewrite

1. Draft the new record with `supersedes: {old number}` and a Context that names what changed.
2. After the user confirms the new record, propose two edits to the old record: `status: superseded` and `superseded_by: {new number}`. Show both and apply them only on confirmation.
3. Run the lint script. It checks the links in both directions.

### Step 11: Backfill a past decision

1. Set `provenance: reconstructed`. The `date` is the date of the decision if a source shows it. If none does, ask, and if it stays unknown use the earliest source date and say so in Context.
2. Start Context with "Reconstructed on {today} from {sources}."
3. Write only what the sources say. Each missing reason becomes `unknown, ask {name}`. Do not smooth over gaps to make the record read well.
4. Ask the person who decided to confirm before the record moves to `accepted`. Where nobody can confirm, leave it as `proposed` and say why.

The mohitagw15856 skill advises against retrospective records. Teams that adopt records late have a backlog, so this skill allows them under the labelling above and refuses to disguise them.

### Step 12: Review what is due

Run the lint script on a schedule. Konishi suggests a quarterly one-hour tech-lead review, again from one author's experience. For each `REVIEW_OVERDUE` record, offer three choices: reaffirm, supersede or deprecate. To reaffirm, append a line to the record's `Review log` section, such as `2027-04-22: still holds, checked by Lee`. Append only, and only with confirmation. For `STALE_PROPOSED` records, ask the owner to accept or reject each one before the next review, and list those that have not moved. A withdrawn proposal is recorded as `rejected`, with the reason.

### Step 13: Report

Give a short report using the template: what was drafted, the gate result, unknowns, lint flags and what the user must do. State that nothing was published or changed beyond the files named.

## Worked example

The data is invented. A six-person agency keeps records in `docs/decisions/`. On 23 October 2026 the user asks the agent to check the folder and add a record for a decision made last week.

The agent runs the lint script on the six existing records:

```
python3 decision_lint.py records --today 2026-10-23
checked 6 records on 2026-10-23; 5 lines flagged
0003-release-on-tuesdays.md | STALE_PROPOSED(64d)
0004-renew-hosting-contract.md | FEW_OPTIONS(1) ONE_SIDED_CONSEQUENCES SENSITIVE_PATTERN(line 9)
0005-replace-helpdesk-tool.md | SUPERSEDE_ONE_WAY(0002 must be status superseded with superseded_by 0005)
0006-??? | NUMBER_GAP (find the record or write a rejected one; never reuse the number)
0007-adopt-sso.md | REVIEW_OVERDUE(2026-09-01)
```

How the agent handles each line, without editing any accepted record:

- **0003** has been `proposed` for 64 days. The agent drafts a message asking Priya to accept or reject it.
- **0004** has one option, upsides only, and line 9 matches an email-or-password pattern. The agent does not print the value. It tells the user which line to redact, suggests a pointer to the finance folder, and asks Dana what else was considered and what the downside is.
- **0005** says it supersedes 0002, but 0002 is still `accepted`. The agent shows the two edits to 0002 (status `superseded`, `superseded_by: 0005`) and waits.
- **0006** is a missing number. The agent asks whether a file was lost or never written, and does not reuse the number.
- **0007** is a reconstructed record whose review date passed on 1 September. The agent offers reaffirm, supersede or deprecate.

The user confirms the 0002 edit. After it, the same command reports the links agree:

```
checked 6 records on 2026-10-23; 4 lines flagged
0003-release-on-tuesdays.md | STALE_PROPOSED(64d)
0004-renew-hosting-contract.md | FEW_OPTIONS(1) ONE_SIDED_CONSEQUENCES SENSITIVE_PATTERN(line 9)
0006-??? | NUMBER_GAP (find the record or write a rejected one; never reuse the number)
0007-adopt-sso.md | REVIEW_OVERDUE(2026-09-01)
```

For last week's decision the agent runs the gate. Reversing it would take about two weeks (yes), it affects the support team (yes), and Lee argued against it (yes). Three triggers, so a standard record. It drafts `0008` as `proposed`, asks four questions about the rejected option, writes `unknown, ask Lee` for the cost comparison, and runs the script again. The draft lints clean. The report ends: "Drafted 0008 as proposed. Edited 0002 after your confirmation. Nothing published."

## Templates

### Decision record

```
---
status: proposed
date: YYYY-MM-DD
deciders: {names or roles}
consulted: {names or roles}
review_by: YYYY-MM-DD
supersedes:
superseded_by:
provenance: decided
---
# {Short title in the form of the choice made}

## Context
{Three to six sentences. Forces, constraints, facts. Source: {where this came from}.}

## Options considered
### {Option A}
{Why attractive. Why chosen or set aside.}
### {Option B}
{Why attractive. Why chosen or set aside.}

## Decision
We will {action}.

## Consequences
- Good: {…}
- Bad: {…}

## How we will check
{One observable measure, a threshold and a date.}

## Review log
```

### Y-statement (one line for the index)

```
In the context of {situation}, facing {concern}, we decided for {option} and against {other option}, to achieve {goal}, accepting {downside}.
```

### Message to a decider (draft, not sent)

```
Subject: Decision {number}: please accept or reject by {date}

Hi {name},
The record "{title}" has been proposed since {date}. Could you confirm the facts, then reply with accept or reject? Open questions: {list}.
```

### Starting prompt

```
Use the decision-records skill. Decision: {one sentence}. Sources: {paths or pasted text}. Records folder: {path}, highest number {n}. Draft as proposed, do not publish, and run the lint script.
```

### Report

```
Decision records: {folder}, {date}
Gate: {triggers hit}/5 -> {Y-statement | standard | none}
Drafted: {number and title}, status proposed
Unknowns for you: {list}
Lint: {n} flags, {list}
Edits proposed to older records: {list, none applied until you confirm}
You need to: confirm facts, circulate, set the status.
Published or sent by the agent: nothing.
```

## Failure modes and anti-patterns

| # | Failure | How it shows up | Fix |
|---|---|---|---|
| 1 | **Documentation theatre.** Trivial choices are recorded, then the habit stops. | Records about formatter settings, then nothing after the fifth. Konishi names this pattern and reports it from his own work. | Run the gate. Record fewer decisions and keep each short. |
| 2 | **Advocacy or invention.** Options are strawmen, consequences are all upside, or the reasons are plausible guesses. | `ONE_SIDED_CONSEQUENCES`, one option, or a rationale with no source. | Apply the strawman test, require a bad consequence, and write `unknown, ask {name}`. |
| 3 | **Edit in place.** An accepted record is quietly changed when the plan changes. | The history of why the team changed course is gone. | Supersede with a new record. Append to the review log only. |
| 4 | **Proposed forever, found by no one.** Drafts sit unaccepted and accepted records have no review date. | `STALE_PROPOSED`, `NO_REVIEW_BY`, `REVIEW_OVERDUE`. | Schedule the lint run, name an owner for each proposal, and set review dates. |
| 5 | **No boundary, no check.** Records mix philosophy, architecture and unfinished evaluations, and nobody can test whether the decision is being followed. | Kappel's team never settled what belonged in a record, and with 40 to 50 records on one page the tooling slowed. | Fix the scope in the 0001 record, keep records short, and write a check for each. |

## Quality bar

A run is complete only if every item holds. Check each one and say which pass.

1. The gate result is stated with the triggers that were hit.
2. The record has `status: proposed`, and the report says the agent did not set any other status.
3. Every statement in Context, Options and Decision has a named source or is marked `unknown, ask {name}`.
4. There are at least two options and each has a reason for and against.
5. The Consequences section has at least one `Good` and one `Bad` line.
6. There is a "How we will check" measure with a threshold and a time.
7. There is a `review_by` date, or a stated reason why not.
8. The file name uses the next unused number and the lint script ran with no flag on the new record.
9. Where a record supersedes another, both ends of the link agree, or the pending edit is shown for confirmation.
10. No password, key, personal data or confidential price is in the record, and the report lists any redaction.
11. No accepted record body was edited.
12. The report lists what the user must do and states that nothing was published.

## Cost and effort

The script reads local text files and runs in well under a second for hundreds of records. A standard record takes one pass plus up to five questions. The human cost is the review: a person must read the draft and confirm the facts, and Konishi suggests 20 to 40 minutes of discussion for a contested decision. Backfilling from long threads costs tokens in proportion to length, so ask the user to paste the relevant part. The cheaper path for a small decision is the Y-statement. Nothing here needs an API or a paid service.

## Handoffs to related skills

- `decision-meetings`: before the record, when the decision still has to be made in a room.
- `meeting-notes-actions`: when the source is a meeting and the user needs actions rather than a record.
- `lessons-learned`: when the question is what to change after the decision played out.
- `knowledge-base-hygiene`: when old decision pages sit in a wiki and need an ownership and staleness audit.
- `note-system-setup`: when the problem is where records and their index should live.
- `atomic-notes`: when the reasoning behind a decision is worth keeping as a reusable note.
- `rag-knowledge-bases`: when records will feed an AI assistant, which should read only `accepted` records.

## Appendix: tool-specific notes

This appendix is fenced off so that the steps above stay tool-agnostic. Every item was read on 23 October 2026. Product and framework pages change, so `verify before relying on this`.

### The lint script

```python
import re, sys, argparse
from pathlib import Path
from datetime import datetime

STATUSES = {"proposed", "accepted", "rejected", "deprecated", "superseded"}
REQUIRED_FIELDS = ["status", "date", "deciders"]
REQUIRED_SECTIONS = ["context", "options considered", "decision", "consequences", "how we will check"]
SECRET = re.compile(r"(AKIA[0-9A-Z]{16}|(?i:password|passwd|api[_-]?key|secret|token)\s*[:=]\s*\S+|[\w.+-]+@[\w-]+\.[\w.-]+)")

ap = argparse.ArgumentParser()
ap.add_argument("folder")
ap.add_argument("--today", required=True)
ap.add_argument("--stale-proposed-days", type=int, default=30)
ap.add_argument("--max-words", type=int, default=1000)
a = ap.parse_args()
today = datetime.strptime(a.today, "%Y-%m-%d").date()

def split(text):
    m = re.match(r"^---\n(.*?)\n---\n(.*)$", text, re.S)
    if not m:
        return {}, text
    meta = {}
    for line in m.group(1).splitlines():
        if ":" in line:
            k, v = line.split(":", 1)
            meta[k.strip().lower()] = v.strip().strip('"')
    return meta, m.group(2)

def sections(body):
    out, cur = {}, None
    for line in body.splitlines():
        h = re.match(r"^##\s+(.*?)\s*$", line)
        if h:
            cur = h.group(1).lower()
            out[cur] = []
        elif cur:
            out[cur].append(line)
    return out

recs = {}
flags = []
for p in sorted(Path(a.folder).glob("*.md")):
    name = p.name
    if name.lower() in ("readme.md", "index.md", "template.md"):
        continue
    m = re.match(r"^(\d{4})-[a-z0-9]+(?:-[a-z0-9]+)*\.md$", name)
    f = []
    text = p.read_text(encoding="utf-8")
    meta, body = split(text)
    if not m:
        f.append("BAD_FILENAME")
        num = None
    else:
        num = m.group(1)
    for k in REQUIRED_FIELDS:
        if not meta.get(k):
            f.append(f"MISSING_FIELD({k})")
    st = meta.get("status", "").lower()
    if st and st not in STATUSES:
        f.append(f"BAD_STATUS({st})")
    d = None
    if meta.get("date"):
        try:
            d = datetime.strptime(meta["date"], "%Y-%m-%d").date()
        except ValueError:
            f.append("BAD_DATE")
    sec = sections(body)
    for s in REQUIRED_SECTIONS:
        if s not in sec or not any(l.strip() for l in sec[s]):
            f.append(f"MISSING_SECTION({s})")
    opts = [l for l in sec.get("options considered", []) if re.match(r"^(###\s|[-*]\s)", l)]
    if "options considered" in sec and len(opts) < 2:
        f.append(f"FEW_OPTIONS({len(opts)})")
    cons = "\n".join(sec.get("consequences", [])).lower()
    if cons.strip() and not (re.search(r"^[-*]\s*good", cons, re.M) and re.search(r"^[-*]\s*bad", cons, re.M)):
        f.append("ONE_SIDED_CONSEQUENCES")
    words = len(re.findall(r"\S+", body))
    if words > a.max_words:
        f.append(f"LONG({words}w>{a.max_words}w)")
    if st == "proposed" and d and (today - d).days > a.stale_proposed_days:
        f.append(f"STALE_PROPOSED({(today - d).days}d)")
    rb = meta.get("review_by", "")
    if st == "accepted":
        if not rb:
            f.append("NO_REVIEW_BY")
        else:
            try:
                if datetime.strptime(rb, "%Y-%m-%d").date() < today:
                    f.append(f"REVIEW_OVERDUE({rb})")
            except ValueError:
                f.append("BAD_REVIEW_BY")
    if meta.get("provenance", "decided").lower() == "reconstructed" and "unknown" not in body.lower() and "source:" not in body.lower():
        f.append("RECONSTRUCTED_NO_SOURCES")
    for i, line in enumerate(text.splitlines(), 1):
        if SECRET.search(line):
            f.append(f"SENSITIVE_PATTERN(line {i})")
    if num:
        recs[num] = {"meta": meta, "status": st, "file": name}
    if f:
        flags.append((name, f))

nums = sorted(recs)
if nums:
    seen = set(nums)
    for n in range(int(nums[0]), int(nums[-1]) + 1):
        if f"{n:04d}" not in seen:
            flags.append((f"{n:04d}-???", ["NUMBER_GAP (find the record or write a rejected one; never reuse the number)"]))
for n, r in recs.items():
    sup = r["meta"].get("supersedes", "").strip()
    by = r["meta"].get("superseded_by", "").strip()
    for t in filter(None, re.split(r"[,\s]+", sup)):
        t = t.zfill(4)
        tgt = recs.get(t)
        if not tgt:
            flags.append((r["file"], [f"SUPERSEDES_MISSING({t})"]))
        elif tgt["meta"].get("superseded_by", "").strip().zfill(4) != n or tgt["status"] != "superseded":
            flags.append((r["file"], [f"SUPERSEDE_ONE_WAY({t} must be status superseded with superseded_by {n})"]))
    if r["status"] == "superseded" and not by:
        flags.append((r["file"], ["SUPERSEDED_NO_POINTER"]))
    if by:
        b = recs.get(by.zfill(4))
        listed = [x.zfill(4) for x in re.split(r"[,\s]+", b["meta"].get("supersedes", "")) if x] if b else []
        if not b or n not in listed:
            flags.append((r["file"], [f"SUPERSEDED_BY_ONE_WAY({by.zfill(4)} must list {n} under supersedes)"]))

print(f"checked {len(recs)} records on {today}; {len(flags)} lines flagged")
for name, f in sorted(flags):
    print(name + " | " + " ".join(f))
```

It reads the Markdown files in one folder, flags problems and changes nothing. It treats `readme.md`, `index.md` and `template.md` as non-records. The 30-day and 1,000-word limits are changeable with `--stale-proposed-days` and `--max-words`.

### MADR

- MADR (Markdown Architectural Decision Records) offers four templates: full with explanations, minimal with explanations, bare, and bare minimal. Copy the chosen one into `docs/decisions` and name each record `nnnn-title.md`.
- The project is dual licensed under MIT or CC0, and says it follows semantic versioning. The repository page does not list template fields or status values, so read the template files before relying on them.

### GOV.UK Architectural Decision Record framework

- Guidance from the Government Digital Service, within the Department for Science, Innovation and Technology, first published on 4 November 2025. Required template fields are title, date, status, context, decision, consequences, stakeholders consulted and links to supporting documents.
- It sorts decisions by reach: team, cross-team, department-wide and cross-government, with a different approving body at each level. Status is required but the allowed values are not defined, so this skill uses the five listed in the workflow.
- The framework is mandatory only for cross-government and cross-public-sector decisions.

### AWS Prescriptive Guidance

- Kunce and Goby, March 2022, name three anti-patterns: no decision for fear of choosing wrongly, decisions without justification so topics are re-debated, and decisions never captured in a repository.

### Repositories and wikis

Keeping records in the same repository as the work, in `docs/decisions` or `docs/adr`, is the convention in the MADR and wshobson material. Wiki platforms were not tested for this skill. If the records live in Notion or Confluence, keep the same fields as page properties and apply the supersede rules by hand.

### Claude Agent Skills

- `name` is limited to 64 characters of lowercase letters, numbers and hyphens, and `description` to 1,024 characters. Claude Code truncates `description` plus `when_to_use` at 1,536 characters in its skill listing. Anthropic advises keeping the SKILL.md body under 500 lines. This file is longer because it carries its own script, which can move to `scripts/decision_lint.py` if the line count matters to you.
- `allowed-tools` pre-approves the tools it lists for the invoking turn. For this skill, do not pre-approve file-writing or shell tools, so that the approval prompt stays in place before any file is written or any script run.

## Sources

Last verified 23 October 2026 for every source below.

- GOV.UK, Architectural decision record framework: https://www.gov.uk/government/publications/architectural-decision-record-framework/architectural-decision-record-framework. Fields, reach levels, publication date.
- MADR, Markdown Architectural Decision Records: https://github.com/adr/madr. Template variants, naming, licence.
- AWS Prescriptive Guidance, Architectural decision records: https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/welcome.html. Anti-patterns and handover use.
- Hidekazu Konishi, Architecture Decision Records: templates and operational patterns (2026): https://hidekazu-konishi.com/entry/architecture_decision_records_templates_and_operations.html. Statuses, immutability, numbering, review cadence, failure modes. One author's experience, not measured.
- Christoph Kappel, Learnings from using ADR in a real project (18 August 2021): https://blog.unexist.dev/documentation/myself/2021/08/18/learnings-from-using-adr-in-a-real-project.html. Scope, size, missing verification. Single project.
- Ahmeti, Linder and Wohlrab, Exploring the adoption and effectiveness of ADRs in agile software development (2023): https://research.chalmers.se/en/publication/538920. Two-month action research study at one company; abstract read only.
- wshobson/agents, architecture-decision-records skill: https://github.com/wshobson/agents. When to write, statuses, directory convention.
- lyndonkl/claude, adr-architecture skill: https://www.skills.sh/lyndonkl/claude/adr-architecture. Listing page only.
- mohitagw15856/pm-claude-skills, architecture-decision-record skill: https://claudeskills.info/skills/mohitagw15856/pm-claude-skills/architecture-decision-record/. Listing page only.
- Anthropic, Skill authoring best practices: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices. Name and description limits, body size.
- Claude Code documentation, Skills: https://code.claude.com/docs/en/skills. Listing truncation, `allowed-tools`.

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/decision-records/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.
