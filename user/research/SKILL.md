<!--
version: 1.1
date: 2026-04-01
source: Added Evaluation Standard section — sources evaluated not relayed
-->

---
name: research
description: "Monitor high-signal sources and route findings to skill updates. Use when running the weekly research cycle, when Jim shares a transcript or article to process, when evaluating whether a new tool or technique should change how we operate, or when designing the research agent. Always use when Jim mentions staying current, watching something, or asks what has changed in any area we operate in. Produces actionable skill update proposals, not reading lists."
---

# Research
## Staying Current Without Jim in the Middle

---

## The Design Principle

Research is not a reading list. It is an input to the harness improvement loop.

Every finding routes to one of three outputs:
1. A proposed skill update — specific text change to a specific skill
2. A new eval criterion — adds to what the overnight loop tests
3. A flagged decision — requires Jim's judgment before acting

If a finding doesn't map to one of those three outputs, it's interesting but not actionable. Don't surface it.

---

## What This Skill Produces
- Output type: Proposed skill updates (specific text, specific file), new eval criteria, or flagged decisions for Jim
- Always includes: Which skill is affected, what the proposed change is, why it matters
- Never includes: General summaries, reading lists, or "things to be aware of"
- Handoff format: For overnight loop — structured list of proposed changes ready to commit to GitHub for Jim's morning review

---

## Source List — High Signal Only

### Claude / Anthropic
- Anthropic release notes: https://docs.anthropic.com/en/release-notes/overview
- Claude Code changelog
- Anthropic blog: https://www.anthropic.com/blog

### SEO / Search
- Google Search Central blog: https://developers.google.com/search/blog
- Google Ads announcements: https://blog.google/products/ads/
- Search Engine Land (major updates only)

### AI Agent / Harness
- Andrej Karpathy GitHub and posts
- Matthew Berman YouTube (transcripts)
- Key GitHub repos: AutoResearch, OpenSpace, oh-my-claudecode

### Business / Product
- Anthropic product updates (new CC features, new models)
- Any source Jim explicitly flags

---

## What to Look For

For each source, the question is: does this change how we should operate?

Signals that warrant a skill update:
- New CC feature that replaces a manual step we document
- Google algorithm change that affects meta/schema standards
- New Google Ads feature or policy that affects our accounts
- A harness pattern that's better than what we're doing
- A model change that affects Porch (api.php model string)

Signals that warrant a new eval criterion:
- Research showing a specific metric predicts outcome (e.g. CTR by title length)
- A benchmark or standard we could test our work against

Signals that warrant a flagged decision:
- Strategic direction change (new product type, new vertical)
- Pricing or competitive shift
- Platform risk (a tool we depend on changing significantly)

---

## Session Format

1. Pull sources — check each source for updates since last research session
2. Filter — apply "does this change how we operate?" test to everything found
3. Route — assign each finding to skill update / eval criterion / flagged decision
4. Draft — write the specific proposed change for each skill update
5. Output — structured list ready for Jim review or GitHub commit

---

## Research Agent Design (overnight)

**Trigger:** Weekly, Sunday night before Monday GSC run
**Inputs:** Source list above, date of last research run
**Steps:** Pull sources → filter → route → draft proposals → commit to GitHub as research-YYYY-MM-DD.md
**Human touchpoint:** Jim reviews Monday morning alongside GSC work order
**Status:** Designing — needs CC web fetch capability confirmed

---

## What Research Is Not

- Not a summary of everything that happened in AI this week
- Not a list of interesting articles
- Not competitive analysis (that's a different workflow)
- Not strategic direction (Jim sets that)

The research skill exists to make the harness smarter. Nothing else.

---

## Evaluation Standard

Sources are evaluated, not relayed. A YouTuber citing a paper is a pointer to the paper, not a conclusion. Process:

1. Find the primary source — the actual paper, release note, or documented research
2. Evaluate it independently — does the underlying finding hold up?
3. Apply it to our context — does it specifically change how we operate?
4. Decide together — Jim and Claude.ai assess strategic implications before anything changes

Berman, Patel, Nate Long and similar voices are useful signal detectors. They are not decision-makers. We read what they point to and decide for ourselves.

---

*Research — 8th Street Solutions*
*Findings route to skills, not to Jim's reading list.*
