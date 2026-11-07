---
name: lessons-learned
description: "Prepares and writes up a review of a finished project, launch, event or non-safety incident, then makes the results usable: a sourced timeline, an expected-versus-actual table, blameless findings, lessons that each carry a trigger, context and named reuse point, and actions with one owner, a date, evidence and an effect check. A standard-library script flags vague, ownerless, overdue, unevidenced and unchecked actions. Drafts only; a person facilitates, reviews and approves. Use whenever someone asks for lessons learned, a post-mortem, retrospective, after action review or project review, wants debrief notes turned into actions, or needs old review actions checked. Do NOT use for personal weekly reflection (use weekly-review), recording a decision (use decision-records), won or lost deals (use win-loss-analysis) or any injury, safety, legal or regulatory incident, which goes to the responsible person."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/lessons-learned/
  category: productivity
  subcategory: knowledge-management
  version: "1.0.0"
  created: "2026-10-23"
  updated: "2026-10-23"
---

# Lessons Learned: Review a Project or Incident, Then Make the Lessons Findable and the Actions Real

Holding a review is easy. Using what it produced is hard. Newell and colleagues studied 13 projects in six organisations and found that "lessons learned" databases were widely adopted and not widely used: the captured knowledge was not judged useful, or project teams did not know it existed. A later write-up of an American Productivity & Quality Center study added that lessons stored without their context cannot be applied by someone on a different project.

This skill prepares and writes up a review of a finished project, launch, event or incident, and it is built around those two findings. Every lesson must name a reuse point, which is the checklist, template or onboarding step where someone will meet it again. Every action must have one owner, a date, evidence of completion and a later check on whether it worked. A short script checks both. A person facilitates the conversation, a person approves the write-up, and the agent sends and publishes nothing.

## When to use this skill, and when not to

Use it when the user wants to:

- Review a project, launch, event, campaign, migration or non-safety incident after it ends, or at a milestone while it runs.
- Turn messy notes from a debrief into a timeline, findings, lessons and actions.
- Prepare the question sheet and pre-read for a review someone else will facilitate.
- Check an existing list of review actions for missing owners, vague wording and actions that were closed and never checked.

Do not use it for:

- Personal weekly reflection. Use `weekly-review`.
- Quarterly planning. Use `quarterly-planning-review`.
- Recording why a decision was made. Use `decision-records`.
- Won and lost deals. Use `win-loss-analysis`.
- Rewriting a process after the review. Use `sops-process-docs`.
- Anything involving injury, a safety event, a regulatory notification, a legal claim or a data breach. Stop and route it to the person responsible. This skill is not legal, safety or HR advice, and whether a written review can be requested in legal proceedings depends on the jurisdiction, so ask counsel before writing anything down.
- Judging an individual's performance. Reviews describe systems and decisions. Performance belongs with the line manager.
- Running the live session. The agent can prepare and write up. A human facilitates.

## Guardrails that apply to every run

1. **Draft only.** The agent produces drafts. It does not send messages, assign tasks in a tracker, publish to a wiki or edit a checklist. Proposed changes are shown as a diff for the user to apply.
2. **Evidence only.** The timeline and the expected-versus-actual table come from sources the user supplied. A claim without a source goes under "to confirm". The agent does not infer causes it was not shown.
3. **Blameless by default.** Describe decisions, conditions and information available at the time. Do not name a person in a cause sentence. Google's SRE book states the principle and gives the reason: blame discourages reporting, so problems get hidden. If the user wants a blame-aware review that names the system first and the person second, as the jamditis skill does, they must ask for it.
4. **Sensitive data.** Reviews contain names, customer details, messages and sometimes credentials. Keep the evidence on the machine where the agent runs, redact personal data and secrets before pasting anything, and leave out information that identifies end users. The SRE book makes the same exclusion for shared postmortems.
5. **Route out the serious cases.** If the evidence suggests injury, a regulated event, legal exposure or a possible offence, stop, say why, and hand the matter to a named person.
6. **No employment outcomes.** The review does not rate people, recommend discipline or feed an appraisal. Items about an individual go to the line manager as a private note, not into the review.
7. **Figures are sourced.** Numbers in the review come from the evidence, with their date. Defaults in this skill are labelled as defaults.
8. **Scope boundary.** The agent will not choose the root cause for the team, mark an action done, mark an effect confirmed without evidence, or share the review outside the group the user names.

## Inputs the agent must have before starting

Ask for what is missing in one message and wait. Do not proceed on assumptions.

| # | Input | What to ask if missing |
|---|---|---|
| 1 | The event | "What are we reviewing, and what were its start and end dates?" |
| 2 | Type | "Is it a project, a launch, an event, an incident or a milestone check?" |
| 3 | Expected outcome | "What was meant to happen? Goals, success measures, the plan." |
| 4 | Evidence | "What records exist: plan, timeline, tickets, chat export, metrics? Paste or point to the relevant parts." |
| 5 | People | "Who took part, and who will facilitate? Who should review the write-up?" |
| 6 | Reuse points | "Where will future teams look: a launch checklist, a template, an onboarding doc, a decision record folder?" |
| 7 | Sensitivity | "Does any of this involve safety, legal, regulatory or personal matters?" |
| 8 | Existing actions | "Is there a list of actions from earlier reviews? Export it as CSV." |

## Workflow

### Step 0: Confirm scope and route out

1. State what you will do: read the supplied evidence, draft a review, run a local script.
2. Check input 7. Any yes under guardrail 5 stops the run until a responsible person is named.
3. Ask whether the evidence has been redacted. If not, ask the user to do it before pasting.

### Step 1: Choose the review type and timing

Reviews work best close to the event. The ODI guide to after action reviews says to hold them immediately while memories are fresh, to build them into planning at key points, and notes that they are often done only at the end. The defaults below borrow the shape of the timing rules in the jamditis project-retrospective skill: same-day debrief and a written review within 48 hours for live events, one week for tool incidents, one month for launches and investigations. The exact windows in the table are this skill's own defaults, and the user can change them.

| Type | Debrief | Written review |
|---|---|---|
| Live event | Same day | Within 2 working days |
| Incident | Within 5 working days | Within 1 week of the debrief |
| Launch or project | Within 1 month of the end | Within 1 week of the debrief |
| Milestone check on a long project | At each named milestone | Same week |
| Short mode (team of 1 to 3, or a small piece of work) | 15 minutes | One page |

The 15-minute figure is the lower end of the ODI examples. If the event is already older than these windows, say so in the review and note that recall will be weaker.

For incidents, define the trigger before the incident rather than after. Google's SRE book lists user-visible downtime or degradation, any data loss, an on-call intervention such as a rollback, a resolution time above a threshold, and a monitoring failure that left someone to notice the problem by hand. It gives no numeric thresholds and says any stakeholder may request a review. Ask the user for their own thresholds and write them down.

### Step 2: Build the timeline and the expected-versus-actual table

1. List events in date and time order from the evidence. Give each a source. Cap the timeline at 20 entries and group minor items.
2. Mark gaps as `gap: no source for {period}`.
3. Build the comparison with three columns: what was expected, what happened, and the difference. This is the ODI plan-against-outcome step.
4. Put every claim that has no source under "to confirm" and ask the participants to correct the draft before the session.

### Step 3: Prepare the session

Give the facilitator a one-page sheet. The ODI guide uses three sets of questions: what was supposed to happen, what actually happened and why they differed; what worked, what did not and why; and what would you do differently next time. It also gives facilitation rules: include every team member, appoint a facilitator, use an uninvolved note-taker, post the questions where everyone can see them, and if input is slow ask each person for one thing that worked and one that did not. If openness or time is limited, gather ideas first and discuss afterwards.

### Step 4: Turn notes into findings

1. Separate **facts** (what happened), **conditions** (what made it possible) and **causes** (why it went the way it did). Mark a cause as `agreed`, `proposed` or `disputed`. Where participants disagree, record both views and do not pick.
2. Apply the specificity test to every item under "did not work". It should name the decision, give the impact with a figure if there is one, say which condition in the system allowed it, and help a reader avoid it. This test is taken from the jamditis skill.
3. Keep at most three key takeaways for the headline, a limit used by the neurofoo AAR skill. Everything else sits in the body.
4. Include what worked. A review of only failures teaches the team to avoid reviews.

### Step 5: Write lessons that can be found again

For each candidate lesson, fill the lesson card (template below). Three fields carry the weight.

- **Trigger:** the situation in which someone should recall this, written as "When you ..." in the words a person would use at that moment.
- **Context:** enough about the original event that a stranger can judge whether it applies, since the APQC material found that lessons without context cannot be applied.
- **Reuse point:** the named, existing place where the lesson will be met next. It must be a checklist line, template field, onboarding step, calendar trigger or decision record.

Decision rules:

- A candidate with all three fields is a **lesson**. Draft the exact change to the reuse point as a diff and show it. Do not edit the file.
- A candidate with no reuse point is an **observation**. Ask the user who could place it. If nobody can within a week, leave it in the review notes and out of the lessons list.
- Two lessons with the same trigger are merged.

Putting a reuse point on every lesson is this skill's design response to the research above. The research reports the problem, and no study read for this skill shows that reuse points fix it, so check after the next similar event whether the lesson was met.

### Step 6: Write the actions

For each action, require:

1. **One owner**, a named person and not a team.
2. **A due date.**
3. **Evidence of completion**, such as a link or a before-and-after measure.
4. **An effect check date**, defaulting to 30 days after completion. For larger changes the jamditis skill uses 30, 60 and 90 day check-ins, and the user can adopt those.
5. **Specific wording.** The ODI guide calls "Make more time to understand the audience" too vague, and its stronger version names who to contact and what to ask. The script flags actions that open with weak verbs such as improve, ensure or consider, or that have fewer than five words.

Limits, all defaults: five actions per review, so that the list gets done, and three open actions per owner. If the user has more, rank them and park the rest as observations.

### Step 7: Run the check script

Save the script in the Appendix as `lessons_check.py`, put actions in `actions.csv` and lessons in `lessons.csv`, and run:

```
python3 lessons_check.py actions.csv --lessons lessons.csv --today 2026-10-23
```

| Flag | Meaning |
|---|---|
| `NO_OWNER`, `NO_DUE`, `BAD_DATE` | Missing owner or date, or a date not in `YYYY-MM-DD` form |
| `VAGUE` | Weak opening verb or fewer than five words |
| `OVERDUE(nd)` | Open and past its due date |
| `STALE_OPEN(nd)` | Open for more than 90 days since creation (default) |
| `DONE_NO_EVIDENCE` | Marked done with no evidence |
| `EFFECT_UNCHECKED(nd)` | Done at least 30 days ago with no effect check (default) |
| `OWNER_OVERLOAD(n>m)` | More than three open actions for one owner (default) |
| `TOO_MANY_ACTIONS(n>m)` | More than five actions in one review (default) |
| `NO_TRIGGER`, `NO_CONTEXT`, `NO_REUSE_POINT` | A lesson card is missing that field |

Fix every flag in the new review. For older reviews, report the flags and ask the owners. If the agent cannot run code, apply the same rules by hand and say so.

### Step 8: Hand over for review

Present the draft review, the diffs for reuse points, the unknowns and the script output. The SRE book says an unreviewed postmortem "might as well never have existed", and asks senior engineers to check that key data was collected, the root cause goes deep enough and the action plan is appropriate. Ask the user to name a reviewer. The SRE book also advises sharing with the widest useful audience and storing the review in a shared repository, and the user decides both.

### Step 9: Follow up

On the effect check date, rerun the script. For each flagged action, draft a message to the owner asking for the evidence or a new date. Mark an effect as confirmed only when the user supplies a measure. When an action changed a reuse point, update the lesson card with the new location.

### Step 10: Report

Use the report template. State that nothing was sent, published or edited.

## Worked example

The data is invented. A five-person studio ran an autumn launch that went live on 5 October 2026. Priya, who ran the launch, asks for a review on 23 October. It is a launch, so the debrief window is one month and the write-up is due within a week of the debrief.

The agent builds a timeline from the launch plan and the support inbox export. Entry 4 reads: "09:10, payment provider shows degraded service. Source: support inbox. Cause: to confirm." The expected-versus-actual table shows the launch email was due at 09:00 and went out at 09:25, and that two staff published different prices for the same plan. It asks Lee and Priya to correct the draft.

After the session the agent drafts three lessons and six actions. It adds them to the studio's `lessons.csv` and `actions.csv`, which also hold three older actions from a summer review, and runs the script:

```
python3 lessons_check.py actions.csv --lessons lessons.csv --today 2026-10-23
checked 9 actions on 2026-10-23; 9 rows flagged
A1 | action | EFFECT_UNCHECKED(41d)
A2 | action | NO_OWNER NO_DUE VAGUE
A3 | action | OVERDUE(33d)
A8 | action | DONE_NO_EVIDENCE
A9 | action | OVERDUE(69d) STALE_OPEN(130d)
L2 | lesson | NO_REUSE_POINT
L3 | lesson | NO_TRIGGER NO_CONTEXT
R1 | review | TOO_MANY_ACTIONS(6>5)
priya | owner | OWNER_OVERLOAD(4>3)
```

How the agent reads it:

- **A2** ("Improve communication between support and product") has no owner, no date and a weak verb. The agent proposes a replacement, "Add a launch calendar to the shared drive and have product update it before each launch", and asks who owns it. It does not choose an owner.
- **L3** ("Support learns about launches late") has no trigger or context, and its reuse point is "Launch calendar". The agent asks the group for the situation in which someone would recall it, since the lesson is an observation until then.
- **L2** (price changes need one approver) has no reuse point. The agent asks where price-page changes are approved today, and drafts a one-line addition to the launch checklist for the user to apply.
- **R1** has six actions and **priya** owns four open ones. The agent ranks the six by effect on the next launch, proposes that A6 move to observations, and asks Priya whether Lee can take A5. Both choices stay with the user.
- **A3** is 33 days overdue and **A9** is 69 days overdue and 130 days old. The agent drafts a message to each owner asking for a new date or a decision to drop the action.
- **A1** was finished 41 days ago and its effect was never checked, and **A8** was closed with no evidence. The agent asks Lee what changed on launch day and where the proof is.

It does not change a flag by editing the data. Flags clear when the data is corrected by the user.

## Templates

### Review document

```
# Review: {event}, {start} to {end}
Type: {launch | project | event | incident | milestone}   Review date: {date}
Facilitator: {name}   Note-taker: {name}   Reviewer: {name}   Audience: {group}

## Three takeaways
1. {…}
2. {…}
3. {…}

## Expected versus actual
| Expected | Actual | Difference |
|---|---|---|

## Timeline (source for each entry; gaps marked)
## What worked
## What did not work (decision, impact, condition, what to do differently)
## To confirm
## Lessons (see cards)  /  Actions (see actions.csv)
## Left out on purpose: {personal data, end-user details, items sent to line managers}
```

### Lesson card

```
Title: {a claim, for example "Provider outages need a named check on launch day"}
Trigger: When you {situation}
Context: {event, date, the facts that make it applicable}
What to do: {one or two sentences}
Source event: {review id}
Reuse point: {the checklist, template or document, and the exact line}
Diff: {the proposed change, not applied}
```

### Lessons and actions files

```
id,title,trigger,context,reuse_point,source_event            (lessons.csv)
id,review,action,owner,due,status,created,done,checked,evidence   (actions.csv)
```

Use `open` or `done` for `status` and `YYYY-MM-DD` for dates.

### Facilitator sheet

```
1. Plan against outcome: What was supposed to happen? What actually happened? Why did they differ?
2. Worked and did not: What worked, and why? What did not, and why?
3. Next time: What would you do differently? Which action could one person finish in 30 days?
If input is slow: one thing that worked and one that did not, from each person.
```

### Message to an action owner (draft, not sent)

```
Subject: {action id}: evidence or new date

Hi {name},
The action "{action}" was due {date}. Could you reply with the evidence it is done, a new date, or a note that we should drop it?
```

### Starting prompt

```
Use the lessons-learned skill. Event: {name, dates, type}. Evidence: {paths or pasted, redacted}. Expected outcome: {…}. Reuse points: {list}. Draft only: nothing sent or published. Run the check script on actions and lessons.
```

### Report

```
Review: {event}, drafted {date}
Type and window: {type}, {within window | late}
Takeaways: {n}. Lessons: {n} with reuse points, {n} observations. Actions: {n}.
Script: {n} rows flagged, {list}
To confirm: {list}. Disputed causes: {list}.
Proposed diffs: {n}, none applied.
You need to: facilitate, name a reviewer, apply the diffs, diary the effect checks.
Sent, published or edited by the agent: nothing.
```

## Failure modes and anti-patterns

| # | Failure | How it shows up | Fix |
|---|---|---|---|
| 1 | **Captured, never reused.** The review is filed and nobody meets it again. | Lessons with no trigger or reuse point. Newell and colleagues found databases that were adopted and not used. | Require a trigger, context and reuse point. Check at the next similar event whether the lesson was met. |
| 2 | **Blame creeps in, or silence follows.** Causes name people, so the next review hides things. | "Dan forgot" appears in a cause sentence. Participants say little. | Rewrite causes as decisions and conditions. Route individual matters to the line manager. |
| 3 | **Vague recommendations.** Actions read like intentions. | `VAGUE` flags, such as "improve communication". | Apply the specificity test and rewrite each action with an object, an owner and a date. |
| 4 | **Actions with no owner, no check or too many.** The list is long and nothing closes. | `NO_OWNER`, `OVERDUE`, `OWNER_OVERLOAD`, `EFFECT_UNCHECKED`. | Cap actions per review and per owner, require evidence and diary the effect check. |
| 5 | **Late, partial or unreviewed.** The review happens months on, only at the end, or nobody checks the write-up. | Timeline full of gaps, a single author, no reviewer named. | Use the timing table, add milestone checks, and name a reviewer. |

## Quality bar

A run is complete only if every item holds. Check each one and say which pass.

1. The review type and its timing window are stated, and a late review is labelled as late.
2. Every timeline entry has a source, and every gap or unsourced claim is listed under "to confirm".
3. No cause sentence names an individual, unless the user asked for a blame-aware review.
4. Disputed causes show both views.
5. There are no more than three headline takeaways.
6. Every "did not work" item names a decision, an impact and a condition.
7. Every lesson has a trigger, context and reuse point, and every candidate without a reuse point is labelled an observation.
8. Every reuse point change is shown as a diff and none was applied.
9. Every action has one named owner, a date, an evidence requirement and an effect check date.
10. The script ran and every flag was fixed or explained.
11. No personal data, secret or end-user identifier is in the review.
12. The report says nothing was sent, published or edited.

## Cost and effort

The script reads two small CSV files and finishes instantly. The agent's cost grows with the evidence it reads, so supply the timeline sources and not a full chat export. The human cost is the session: ODI gives 15 minutes for a short review after a one-day workshop and longer for a full one, and no standard duration for a launch review, so ask the user how long they will allow. The cheaper path for a small piece of work is short mode, with three questions and one or two actions. Nothing here needs a paid service.

## Handoffs to related skills

- `decision-records`: when a finding is a decision that needs its reasoning kept. Hand the decision to that skill and link the record from the lesson.
- `weekly-review` and `quarterly-planning-review`: for the user's own rhythm, which should include a look at overdue review actions.
- `meeting-follow-up`: for sending the actions once the user approves them.
- `sops-process-docs`: when a lesson changes a procedure and the procedure needs rewriting.
- `knowledge-base-hygiene`: when old review pages need an ownership and staleness audit.
- `atomic-notes`: when a lesson is worth keeping as a reusable personal note.
- `win-loss-analysis`: for sales outcomes.

## Appendix: tool-specific notes

This appendix is fenced off so that the steps above stay tool-agnostic. Every item was read on 23 October 2026. Practices and product pages change, so `verify before relying on this`.

### The check script

```python
import csv, re, argparse
from collections import Counter
from datetime import datetime

WEAK = re.compile(r"^(improve|ensure|make sure|be more|try to|try|consider|look into|focus on|communicate|do better|enhance|strengthen|monitor)\b", re.I)
ap = argparse.ArgumentParser()
ap.add_argument("actions"); ap.add_argument("--lessons")
ap.add_argument("--today", required=True)
ap.add_argument("--stale-days", type=int, default=90)
ap.add_argument("--effect-days", type=int, default=30)
ap.add_argument("--max-open-per-owner", type=int, default=3)
ap.add_argument("--max-actions-per-review", type=int, default=5)
a = ap.parse_args()
today = datetime.strptime(a.today, "%Y-%m-%d").date()

def day(v):
    v = (v or "").strip()
    if not v:
        return None
    return datetime.strptime(v, "%Y-%m-%d").date()

out = []
acts = list(csv.DictReader(open(a.actions, encoding="utf-8")))
open_by_owner = Counter(r["owner"].strip().lower() for r in acts if r["status"].strip().lower() == "open" and r["owner"].strip())
per_review = Counter(r["review"].strip() for r in acts)
for r in acts:
    f = []
    st = r["status"].strip().lower()
    own = r["owner"].strip()
    text = r["action"].strip()
    try:
        due, created, done, checked = day(r["due"]), day(r["created"]), day(r["done"]), day(r["checked"])
    except ValueError:
        out.append((r["id"], "action", ["BAD_DATE"])); continue
    if not own: f.append("NO_OWNER")
    if not due: f.append("NO_DUE")
    if WEAK.match(text) or len(text.split()) < 5: f.append("VAGUE")
    if st == "open":
        if due and due < today: f.append(f"OVERDUE({(today - due).days}d)")
        if created and (today - created).days > a.stale_days: f.append(f"STALE_OPEN({(today - created).days}d)")
    if st == "done":
        if not r["evidence"].strip(): f.append("DONE_NO_EVIDENCE")
        if done and not checked and (today - done).days >= a.effect_days: f.append(f"EFFECT_UNCHECKED({(today - done).days}d)")
    if f: out.append((r["id"], "action", f))

for o, n in open_by_owner.items():
    if n > a.max_open_per_owner: out.append((o, "owner", [f"OWNER_OVERLOAD({n}>{a.max_open_per_owner})"]))
for rv, n in per_review.items():
    if n > a.max_actions_per_review: out.append((rv, "review", [f"TOO_MANY_ACTIONS({n}>{a.max_actions_per_review})"]))

if a.lessons:
    for r in csv.DictReader(open(a.lessons, encoding="utf-8")):
        f = []
        for k, flag in (("trigger", "NO_TRIGGER"), ("context", "NO_CONTEXT"), ("reuse_point", "NO_REUSE_POINT")):
            if not r[k].strip(): f.append(flag)
        if f: out.append((r["id"], "lesson", f))

n_acts = len(acts)
print(f"checked {n_acts} actions on {today}; {len(out)} rows flagged")
for i, kind, f in sorted(out):
    print(f"{i} | {kind} | " + " ".join(f))
```

It reads the CSV files and prints flags. It changes no file. The 90-day, 30-day, three-per-owner and five-per-review limits can be changed with `--stale-days`, `--effect-days`, `--max-open-per-owner` and `--max-actions-per-review`.

### Google SRE postmortem practice

- The SRE book defines a postmortem as a written record of an incident's impact, the actions taken to mitigate it, the root causes and the follow-up actions. The public page does not give a template, and says Google uses an internal one with metadata fields added later for trend analysis.
- It recommends monthly or regular practices such as a postmortem of the month and reading clubs. They are Google's practice, not tested elsewhere.

### After action reviews

The ODI document is undated, and its case example is from April 2005. It describes the three question sets used above and a longer retrospect format of 11 questions. It reports failure modes of vague recommendations, a passive audience, fear of mistakes, time pressure, no note-taker, and reviewing only at the end.

### Lessons learned systems

The Newell study (Management Learning, 2006) is behind a repository restriction and only the abstract was read. The Projects@Work article about the APQC study (launched 2009, three organisations) cites nine practices behind a login wall. Those practices were not read, so this skill does not claim them.

### Claude Agent Skills

- `name` is limited to 64 characters and `description` to 1,024. Claude Code truncates `description` plus `when_to_use` at 1,536 characters in its skill listing. Anthropic advises a SKILL.md body under 500 lines. This file is longer because it carries its own script, which can move to `scripts/lessons_check.py`.
- `allowed-tools` pre-approves listed tools for the invoking turn. For this skill, do not pre-approve file-writing or shell tools, so that an approval prompt appears before any file is written.

## Sources

Last verified 23 October 2026 for every source below.

- Google, Site Reliability Engineering, Postmortem culture: https://sre.google/sre-book/postmortem-culture/. Definition, triggers, blameless principle, review and sharing.
- ODI, After Action Reviews and Retrospects: https://media.odi.org/documents/6431.pdf. Question sets, timing, facilitation rules, failure modes. Undated.
- Newell, Bresnen, Edelman, Scarbrough and Swan, Sharing knowledge across projects: limits to ICT-led project review practices, Management Learning 37(2), 2006: https://wrap.warwick.ac.uk/33255/. Abstract only.
- Projects@Work, Where do lessons go to die?: https://www.projectmanagement.com/articles/261723/where-do-lessons-go-to-die-. Summary of an APQC study. Author and date not shown, partly paywalled.
- jamditis/claude-skills-journalism, project-retrospective skill: https://claudeskills.info/skills/jamditis/claude-skills-journalism/project-retrospective/. Timing pattern, specificity test, check-ins. Listing page, read 23 October 2026.
- neurofoo/agent-skills, aar skill: https://skills.cat/skills/neurofoo/agent-skills/aar. Four-question format, three takeaways. Listing page.
- kenneth-liao/ai-launchpad-marketplace, retrospective skill: https://skills.cat/skills/kenneth-liao/ai-launchpad-marketplace/retrospective. Five-finding cap and user confirmation. Listing page.
- Anthropic, Skill authoring best practices: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices. Name and description limits, body size.
- Claude Code documentation, Skills: https://code.claude.com/docs/en/skills. Listing truncation, `allowed-tools`.

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/productivity/knowledge-management/lessons-learned/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.
