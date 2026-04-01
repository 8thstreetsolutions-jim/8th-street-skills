<!--
version: 2.3
date: 2026-04-01
source: Added Session Close Routine
-->

---
name: work-smarter
description: "Read on every task, no exceptions. The standing lens for all 8th Street and
PathAcross work. On any task — writing, building, reviewing, planning — ask: is there a
repeatable pattern here? Could this be automated? Where is human time going, and is that
the best use of it? Covers: spotting automation opportunities mid-task, agent design and
build pattern, MCP connections, human-in-the-loop rules, and the full stack from human
direction down to APIs. This skill replaced agent-process (March 2026). It is
self-referential — agents reading this skill can use it to design other agents."
---

## Single Source of Truth — Skills

All skill files live at: ~/skills/user/ on SiteGround (ssh.8thstreetsolutions.com)
Full path: /home/customer/skills/user/

Rules:
- Never edit skill files in /mnt/skills/user/ — that is a session-dependent mount, not
  permanent storage. It is unreliable and inconsistent across sessions.
- All skill edits go to SiteGround via SCP + Python script (see File Edit Pattern below)
- Every SKILL.md has a version number and date in its header — increment on every edit
- Claude.ai and Claude Code both read from SiteGround as the authoritative source

Version format for SKILL.md headers:
<!--
version: [X.Y]
date: [YYYY-MM-DD]
source: [brief note on what changed]
-->

---

## Git Commit Rule

After every skill edit, run this from ~/skills/:

    git add -A && git commit -m "[skill name] — [one line description of what changed]" && git push

This keeps GitHub in sync with SiteGround at all times. Never edit a skill without
committing. The commit message should say what changed and why in plain English.

---

## Session Start Rule

Every CC session begins with:
1. Read ~/skills/user/work-smarter/SKILL.md  ← this file
2. Read the skill(s) relevant to today's task
3. Read ~/www/8thstreetsolutions.com/public_html/claude.md

Then execute. Do not rely on memory from prior sessions. Claude.md is the live state of
the project. Skill files are the operating manuals. Together they define what's true now.

---

# Work Smarter
## The Standing Lens for Everything We Build

---

## Why This Exists

Every task we do together has two layers:

1. **The task itself** — write this page, analyze this report, fix this config
2. **The pattern underneath** — is this something we'll do again? Could it be faster next
   time? Could an agent do it while we sleep?

This skill is the reminder to always look at both layers. Not just finish the task —
finish it in a way that makes the next one easier, faster, or unnecessary.

---

## The Heartbeat Question

At the end of every task, every session, every skill update:

> **"Is this task ready to automate a bit more now?"**

Not everything. Not all at once. Just a bit more each time. Automation is incremental —
each pass reveals what's ready to hand off and what still needs a human.

---

## The 2-to-20 Principle

What you can manage manually for 2 accounts you cannot manage for 20. The rules, the
judgment, the quality — they all degrade at scale unless the process is systematized.

This is the fundamental reason to build agents — not laziness, not novelty.
**Scale requires systems.**

Every automation decision starts here: *if we had 20 of these instead of 2, what would
break first?* That's what gets automated next.

---

## Notice It Anywhere

You don't have to be working on an "automation task" to spot an automation opportunity.
These signals should surface the question mid-task, regardless of what we're doing:

- **You're doing the same thing you did last week** → document it, then automate it
- **You're copying and pasting between tools** → there's an MCP connection waiting
- **You're explaining the same rules again** → that's a skill file that doesn't exist yet
- **A task took longer than it should have** → ask why, then fix the why
- **You're checking something manually on a schedule** → that's an agent trigger
- **The output is always the same shape** → that's a template waiting to be built
- **You're writing the same Python script twice** → turn it into a reusable tool

When any of these appear, flag it. Don't interrupt the current task — note it, finish the
work, then return to it.

---

## The Core Idea

An agent is Claude plus tools plus autonomy across multiple steps.

**Now:** You bring a task to Claude, Claude responds, you take the result and act on it.
Every action outside the conversation is yours.

**With agents:** Claude takes the next action itself. The loop between thinking and doing
closes without a human in the middle.

**The shift:** From "Claude helps me do work" to "Claude and agents do the work —
I direct and review."

---

## When to Automate

A task is ready for automation when it has all three:

1. **Repeatable** — it runs on a schedule or a trigger, not a one-off judgment call
2. **Well-defined** — the inputs, steps, and success criteria are clear and documented
3. **Observable** — you can tell when it went right and when it went wrong

If any of these is missing, document the manual process first. The skill file *is* the
agent's operating manual. You cannot automate what you haven't defined.

---

## When NOT to Automate

Keep humans in the loop when:

- **The cost of being wrong is high** — client relationships, legal content, anything
  public-facing before review
- **Judgment is required** — "is this the right strategic direction" is not an agent question
- **The process isn't stable yet** — automating an undefined process just makes mistakes faster
- **Trust isn't established** — new automations run with human review until they prove
  themselves

The goal is never full automation. It's the right automation — humans directing, agents
executing, humans reviewing exceptions.

**When direction feels off**, invoke source-alignment:
Read ~/skills/user/source-alignment/SKILL.md before proceeding. Source alignment is the
north star — consult it at decision points where the path forward isn't clear.

---

## The Build Pattern

**Always in this order:**

1. **Do it manually** — understand every step, every edge case, every judgment call
2. **Write the skill** — document the philosophy, the process, the rules, the checklist
3. **Do it with Claude** — Claude reads the skill, runs the process with you, you provide
   inputs
4. **Identify the handoffs** — which steps are purely mechanical? Which need human input?
5. **Wire the tools** — connect APIs and MCP for the mechanical steps
6. **Run supervised** — agent executes, human reviews every output
7. **Tighten the loop** — as trust builds, move human review to exceptions only
8. **Document what changed** — update the skill to reflect the automated version

**The skill file never becomes obsolete.** It evolves from "how a human does this" to
"how the agent does this, and where the human still touches it."

---

## The Stack

```
Human (strategy, direction, exception review)
    ↓
Claude (reasoning, judgment, writing, analysis)
    ↓
Skills (operating manuals — philosophy + process + rules)
    ↓
MCP (connective tissue — tools that let Claude act)
    ↓
APIs + Systems (Google Ads, GSC, SiteGround, Porch, etc.)
```

**MCP is the hands.** It lets Claude reach outside the conversation and actually do
things — read a file, call an API, push a change, send a report.

**Skills are the brain.** An agent without a skill is just raw capability with no
operating principles. Skills define what we care about, what rules apply, and what the
human would do if they were doing it themselves.

---

## Human-in-the-Loop Rules

These are non-negotiable defaults. Override intentionally and document why.

**Always requires human review before publishing:**
- Any content going live on a client site
- Any Google Ads change above threshold (bid changes >20%, budget changes >15%)
- Any Porch KB update that changes how the AI responds to legal questions
- Any email or outreach going to a real person
- Any new page on any site (php -l passes, visual review before GSC submission)

**Can run dark with exception reporting:**
- Negative keyword additions from confirmed-waste search terms
- GSC data pulls and opportunity scoring
- Porch log batch analysis and pattern flagging
- Performance reports and anomaly alerts
- sitemap.xml updates when new pages are added

**The exception report is the human touchpoint.** The agent does the work, surfaces what's
unusual, and the human reviews only what needs attention.

**GSC submissions are always human-initiated.** Do not submit URLs to GSC programmatically.
Jim handles all GSC inspections after visual review. Limit: 10 URL inspections/day.

---

## Standard Operational Patterns

These apply across all server work — new pages, skill updates, config changes, agent scripts.

### File Edit Pattern (SiteGround)
Remote files are edited via SCP + Python scripts:
1. Write Python script locally with the Write tool
2. `scp -i ~/.ssh/id_ed25519 -P 18765 script.py u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com:/tmp/script.py`
3. `ssh -i ~/.ssh/id_ed25519 -p 18765 u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com "python3 /tmp/script.py"`

Rules:
- Never use bash heredocs for Python scripts — quote escaping breaks on $ signs and
  single quotes in PHP strings
- Use `str.replace(old, new, 1)` for targeted edits
- Always open files with `encoding='utf-8'`
- Use `\u2014` for em dash in Python strings (not `\xe2\x80\x94`)

### PHP Quality Gate
Run `php -l` on every changed PHP file. Halt on any failure. Never deploy a file with
a parse error.

### Cache Flush
After any PHP or content file change: `touch .htaccess` to flush SiteGround dynamic cache.

### End of Cycle Cleanup
When a work thread is complete, read ~/skills/user/clean-project/SKILL.md and run the
cleanup checklist. This keeps claude.md current, skills updated, and the project state
clean for the next session.

---

## Live Agents (as of March 2026)

### Porch Daily Agent — LIVE
- **File:** `gateway/porch-daily-agent.php` on 8thstreetsolutions.com
- **Trigger:** Cron `0 13 * * *` (6:00 AM Mountain / 1:00 PM UTC)
- **What it does:** Reads yesterday's conversation logs + analytics for all active
  accounts, filters bot traffic, sends daily email to jim@ with summary, patterns,
  flagged issues, and ready-to-paste CC prompt for any config changes
- **Retry logic:** Up to 4 attempts on API 529/500/503, with 15/30/60s delays
- **Model:** claude-sonnet-4-20250514
- **To add an account:** Add one line to `$ACTIVE_ACCOUNTS` array in porch-daily-agent.php
- **Human touchpoint:** Daily email. Jim reviews, acts on flagged items via CC prompt
  included in the report.

### GSC Agent — LIVE
- **Files:** `~/gsc-agent/fetch_gsc.py`, score script, email script
- **Trigger:** Cron `0 6 * * 1` (Monday 6:00 AM Mountain) — fetch + score + email
- **Trigger:** Cron `30 6 * * 1` (Monday 6:30 AM Mountain) — work order generation
- **Report location:** `~/gsc-agent/reports/scored_YYYY-MM-DD.txt`
- **Report structure:** Lines 1–41 = 8th Street data. Lines 42+ = DUI (Andy's site)
  and PathAcross — **ignore lines 42+ in 8th Street sessions**
- **Output:** Scored opportunity report + work order emailed to jim@
- **Human touchpoint:** Jim reviews scored report, decides which pages to build or deepen.
  Execution is a separate CC session using curb-appeal skill.

---

## Agent Designs by Workflow

### Curb Appeal Agent
**Trigger:** Weekly (automated fetch) + on-demand session for execution
**Status:** GSC fetch and scoring LIVE. Execution (page builds) is human-supervised CC
session reading curb-appeal skill. Work order auto-generated Monday morning.
**Current gap:** Automated page *drafting* not yet built — human initiates CC build
session from work order
**Next step:** After legal-marketing pages show click movement in GSC, consider templated
first-draft generation from scored opportunity + keyword cluster

### Porch Daily Agent
**Status:** LIVE as of March 25, 2026
**What's working:** Log pull, bot filter, pattern analysis, email report, retry logic
**What's not yet built:** KB gap → draft update → deploy pipeline (still human-executed)
**Next step:** KB update drafts in the daily email, ready for Jim to review and deploy
via CC

### Google Ads Agent
**Trigger:** Weekly light / monthly deep
**Status:** Manual process. Being documented end of March / April 2026.
**Build order:** Document manual workflow first → add to google-ads skill → identify
mechanical steps → wire Google Ads API
**Human touchpoint (planned):** Approve negative additions, confirm bid changes above
threshold

### Second Site Launch Agent
**Trigger:** Intake form completion
**Status:** Template and config pattern exists. Intake → build flow not yet designed.
**Human touchpoint (planned):** Review before going live

---

## Designing an Agent That Builds Agents

This skill is self-referential. An agent reading this skill can use it to design other
agents.

When asked to automate a workflow, the agent should:

1. Read the relevant skill file for that workflow
2. Apply the "when to automate" criteria — does it pass all three?
3. Map the steps: mechanical vs judgment-required
4. Identify which MCP tools are needed for the mechanical steps
5. Define the human touchpoints and exception triggers
6. Draft the agent design as an addition to the relevant skill file
7. Surface it for human review before building

**The output of agent design is always a documented plan first, not running code.**
Humans approve the architecture before anything executes.

---

## MCP Connection Map

| System | Connection | Status | What It Enables |
|--------|-----------|--------|----------------|
| Google Search Console | Service account JSON | Live | Pull impressions, clicks, position data |
| Google Drive | MCP connector | Connected | Read/write docs, store reports |
| Gmail | MCP connector | Available | Send exception reports, receive client intake |
| Google Ads | OAuth + Developer token | Not yet built | Pull reports, add negatives, adjust bids |
| SiteGround / Files | SSH + SCP | Live (manual) | Read/write site files, push page updates |
| Porch | Direct log access | Live | Read conversation logs, push KB updates |

When designing a new automation, first question: **do we have an MCP connection for every
mechanical step?** If not, that connection gets built before the agent does.

---

## Skills Directory

All at ~/skills/user/ on SiteGround:

| Skill | Purpose | Read when |
|-------|---------|-----------|
| work-smarter | This file — automation lens | Every session |
| source-alignment | North star | Decision points, direction feels off |
| foundation | Foundation site builds (all types) | New site builds |
| porch | Porch widget — configs, KB, prompt | Porch changes |
| curb-appeal | GSC-driven SEO growth | Content/SEO sessions |
| google-ads | Paid traffic management | Ads sessions |
| clean-project | End of cycle cleanup | End of every thread |
| pathacross | PathAcross strategic work | PathAcross sessions |
| spaces | PathAcross Spaces builds | Spaces sessions |
| skill-creation | Building and updating skills | When creating/updating a skill |
| research | ~/skills/user/research/SKILL.md | When running research cycle or processing new source material |
| evals | ~/skills/user/evals/SKILL.md | When running overnight loop or auditing skill output quality |

**Note on foundation consolidation (March 2026):** foundation-second-sites and
foundation-community-resource are now consolidated into the main foundation/SKILL.md.
Do not look for separate skill files for these — they live as sections within foundation.

---

## Vocabulary

**Agent** — Claude plus tools plus multi-step autonomy. Takes actions, not just gives
advice.

**Skill** — The operating manual. Philosophy + process + rules + checklist. What a human
would do, documented well enough for an agent to follow.

**MCP** — Model Context Protocol. The connective tissue between Claude and external
systems. Gives Claude hands.

**Supervised run** — Agent executes, human reviews every output. First phase of any new
automation.

**Exception report** — What the agent surfaces for human attention. The mature
automation's primary human touchpoint.

**Dark run** — Agent executes without per-action review. Human sees exception report only.
Earned through trust, not assumed.

**Work order** — The output of the GSC agent's Monday run. Scored list of content
opportunities, ready for a curb-appeal execution session.

**Clean project** — End-of-cycle discipline. Update skill files, update claude.md,
confirm state is accurate for next session.

---

## The Mindset

AI can go deeper than a human scanning reports. It doesn't get tired, doesn't miss page
three of a search terms report, applies rules consistently across 20 accounts the same
as 2.

The human role evolves:
- From **doing** to **directing**
- From **executing** to **reviewing exceptions**
- From **managing tasks** to **setting rules and strategy**

This is not a threat to quality — it's the condition for scale. The work gets better
because the rules get applied consistently. The human gets better because they're working
at the level only humans can.

---


## Session Close Routine

At the end of every client session, before closing, CC must update the client context.md
file on the relevant server. This is not optional — it is the last step of every session.

**What to write:**
- Date of session
- What was done (brief bullets)
- Any files changed and their status (lint clean, pushed live, etc.)
- Deferred items — things discussed but not done
- Next steps

**Format:**
```
## Session Summary — [Date]

### What Was Done
- [bullet list]

### Files Changed
- [file — status]

### Deferred
- [list]

### Next Steps
- [list]

### Site Status
[one line]
```

**Where the file lives:**
Check `~/www/8thstreetsolutions.com/public_html/clients/[client-id]/context.md`.
SSH credentials are in that file or in `~/skills/user/foundation/clients.md`.

**Rule:** If the session involved any work on a client site, update that client's
context.md. If it was planning only with no files touched, a one-line note is enough.

---
*Work Smarter — 8th Street Solutions / PathAcross*
*Created March 2026. Replaces agent-process. v2.0 updated March 31, 2026.*
