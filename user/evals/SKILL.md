<!--
version: 1.0
date: 2026-04-01
source: New skill — eval criteria for overnight self-improvement loop
-->

---
name: evals
description: "Defines success criteria for every skill so the overnight self-improvement loop knows what better looks like. Use when designing or running the overnight improvement loop, when adding a new eval criterion based on research, when auditing whether a skill is actually producing good outputs, or when a client result prompts the question 'why did that not work.' This is the measurement layer — without it, the loop is guessing."
---

# Evals
## What "Better" Looks Like for Each Skill

---

## Why This Exists

The MetaHarness principle: you cannot improve what you cannot measure. Evals turn skill improvement from intuition into something testable. Every overnight loop run checks outputs against these criteria. If outputs pass, the skill is working. If they fail, the skill needs updating.

---

## What This Skill Produces
- Output type: Pass/fail criteria per skill, measurable from available data
- Always includes: The signal source (GSC, Porch logs, Google Ads), the threshold, and what failure triggers
- Never includes: Subjective criteria that can't be measured from data
- Handoff format: For overnight loop — structured eval results per skill with pass/fail and proposed fix if failed

---

## Eval Criteria by Skill

### Curb Appeal
**Primary eval:** CTR on pages built in last 90 days
- Pass: CTR ≥ industry average for position (use GSC position vs CTR benchmarks)
- Fail trigger: Page has 100+ impressions, position 1-20, CTR below 2% → meta title/description needs rewrite
- Secondary: Position movement — pages built 30+ days ago should show position improvement vs baseline
- Fail trigger: Position worse than day-of-build → content depth issue, flag for review

**Meta title/description specific:**
- Pass: Title 50-60 chars, front-loaded keyword, action verb present
- Pass: Description 140-160 chars, includes primary keyword in first 120 chars, has CTA
- Fail: Google rewrote the meta (visible in GSC) → our version wasn't compelling enough, rewrite

### Google Ads
**Primary eval:** Cost per qualified lead trend (month over month)
- Pass: $/conv stable or improving
- Fail trigger: $/conv up >20% month over month → flag for review, check search terms report

**Conversion tracking eval:**
- Pass: All three conversion types firing (phone click, email click, form submit)
- Fail: Any conversion type at 0 for 7+ days → tracking broken, flag immediately

**Search terms eval:**
- Pass: <15% of spend on negative-candidate terms
- Fail: Any single term with 10+ clicks and 0 conversions → immediate negative candidate

**Quality signal eval (monthly):**
- Check: Landing page experience rating for primary keywords
- Check: Ad relevance rating for top ad groups
- Fail: Either rated "Below average" → specific fix required (see google-ads skill)

### Porch
**Primary eval:** Conversation-to-contact rate (conversations that result in phone/email shared)
- Pass: >25% of conversations result in contact info or callback request
- Fail trigger: Rate drops below 15% → KB or prompt needs review

**Response quality eval (daily agent):**
- Pass: Average response length 2-3 sentences
- Fail: Responses consistently >5 sentences → prompt drift, tighten
- Pass: No legal/medical advice given
- Fail: Any instance of specific legal or medical opinion → immediate prompt fix

### Foundation / Curb Appeal — Page Quality
**Every page built must pass before GSC submission:**
- php -l passes with 0 errors
- Title 50-65 chars (measure, don't eyeball)
- Description 140-160 chars
- No duplicate title or description vs existing pages
- Page in sitemap.xml
- FAQPage schema present
- Internal link from hub page confirmed

### Skills Self-Improvement
**Weekly eval — did the skill produce better outputs this cycle vs last?**
- Curb Appeal: Are pages ranking faster than 90 days ago?
- Google Ads: Is $/conv trending down?
- Porch: Is conversation-to-contact rate stable or improving?
- If degrading: Read last 3 versions of that skill in GitHub, identify what changed, propose rollback or fix

---

## Eval Cadence

| Eval | Frequency | Data Source | Who Reviews |
|------|-----------|-------------|-------------|
| Page CTR | Weekly (Monday) | GSC | Overnight agent → Jim |
| Ads $/conv | Monthly | Google Ads | Monthly review |
| Conversion tracking | Weekly | Google Ads | Overnight agent → alert if fail |
| Search terms waste | Monthly | Google Ads | Monthly review |
| Porch contact rate | Daily | Porch logs | Daily agent → Jim |
| Page quality gates | Every build | php -l + manual | CC before GSC submit |
| Skill improvement | Weekly | GSC + Porch logs | Overnight agent → Jim |

---

## Adding New Eval Criteria

When research surfaces a metric that predicts outcome, add it here:
1. Name the metric
2. Define pass/fail threshold
3. Identify the data source
4. Set the review cadence
5. Commit to GitHub

Evals compound. Each one added makes the overnight loop smarter.

---

*Evals — 8th Street Solutions*
*Measurement is what turns the loop from guessing to improving.*
