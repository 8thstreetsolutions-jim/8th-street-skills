<!--
version: 2.1
date: 2026-04-01
source: Major update — three-layer KB architecture, active accounts table, monthly refresh
        process, email suppression ops, demo subdomain architecture, no-Porch-on-Ads rule,
        self-serve flow, dashboard URL, KB FAQ writing depth
v2.1: Added output contract
-->

---
name: porch
description: "Build, configure, update, and manage Porch AI chat widgets. Use when (1)
writing or updating a client Porch config JSON, (2) troubleshooting the widget or api.php,
(3) building a new client config from scratch, (4) reviewing dashboard metrics, (5) writing
KB or prompt content for any client, (6) working on the self-serve flow, (7) diagnosing
widget behavior issues. Always use this skill when Jim mentions Porch, a widget, a client
config, api.php, widget.js, the dashboard, or any client\'s chat setup. Client config
details in clients/[client-id]/porch.md on SiteGround."
---

# Porch
## AI Chat Widget System

---


## What This Skill Produces
- Output type: Porch client config JSON (new or updated), KB content, or api.php/widget.js fixes deployed to SiteGround
- Always includes: Config validated against three-layer KB architecture; changes logged in `clients/[client-id]/change-log.md`; no-Porch-on-Ads rule checked
- Never includes: Pushing for contact info in widget responses; configs going live without Jim review; changes to the dashboard or billing layer
- Handoff format: Updated config file path on SiteGround + summary of KB changes + any client-specific rules added

---

## What Porch Is

Porch is the first act of care, not lead capture. A scared person searching at 2am will not
fill out a form. They might type a question into a chat window if it feels safe.

The widget is the foot in the door. The relationship is the product.

**Core principle:** Help first. Never push for contact info. Scared people don\'t read
paragraphs — 2–3 sentences max. Match their energy.

**Client config details in:** `clients/[client-id]/porch.md` on SiteGround. Read that file
before any client session.

---

## Active Accounts

| Account | Site | Config ID | Status | Notes |
|---------|------|-----------|--------|-------|
| 8th Street | 8thstreetsolutions.com | 8thstreet | Active | Self-demo config |
| Andy (Illinois DUI Defender) | illinois-dui-defender.com | illinois-dui-defender | Active | DUI, Illinois |
| Heart of Illinois (Maureen) | heartofillinoiscriminaldefense.com | heart-of-illinois | Active | CD+DUI, Illinois |
| Aquarius Lawyers | demo only | aquariouslawyers | Demo | AU firm. Demo sent Mar 2026. Awaiting response. |
| Melnick Timmerman | demo only | melnicktimmerman | Demo | CO law firm. Demo delivered, awaiting response. |

**Winding down (email suppression active — no open/interaction emails to jim@):**
- Grand Medical Specialists — staying live as courtesy
- John N. Campbell MD — staying live as courtesy

**Email suppression control:** `OPEN_EMAIL_SUPPRESSED` array in `api.php`. Add clientId to
suppress open/interaction notifications without removing the config.

---

## System Architecture

```
widget.js (client-side, v2.17)
  ↓ fetches config + sends messages
gateway/api.php (server-side, v1.28)
  ↓ loads client config
gateway/clients/free/[clientid].json  ← config schema v2.0
  ↓ calls
Anthropic Claude API (claude-sonnet-4-20250514)
  ↓ logs to
gateway/logs/[clientid]_YYYY-MM-DD.log
  ↓ analytics to
gateway/analytics/[clientid]_YYYY-MM-DD.json
  ↓ reviewed daily by
gateway/porch-daily-agent.php (cron 6 AM Mountain / 1 PM UTC)
  ↓ daily email to jim@ with summary, patterns, flagged issues
```

**Single gateway:** `8thstreetsolutions.com/gateway/` — all clients share one gateway.
No base.json — every client requires its own config file.

**Dashboard:** `pathacross.com/dashboard.php?token=pa-monitor-2026` (fed by dashboard-api.php)

**Dashboard API correct URL:** `https://8thstreetsolutions.com/gateway/dashboard-api.php`  
(not `/dashboard-api.php` — that path 404s)

**Self-serve flow:** `/get-porch.php` → `/generate-porch.php` → `/porch-ready.php`
This is John\'s LinkedIn workflow. `build.php` on demo subdomain is the demo generator.

---

## Log File Format

Each client log file follows this exact structure:

```
========== HH:MM:SS ==========
Page: https://example.com/page-url
Lead Qualified: YES|NO
[Email: address@example.com]  ← only present if lead captured

[12:34 PM] Porch:
[greeting text]

[12:35 PM] Visitor:
[visitor message]

[12:35 PM] Porch:
[bot reply]
```

A session with no `[Visitor:]` line means the widget was opened but nobody typed — do not count as a conversation.

**Conversation definition:** A log session containing at least one `[Visitor:]` line. Auto-open sessions where only Porch speaks do not count.

---

## The Three-Layer KB Architecture

**Never duplicate information across layers.** Each layer has a distinct job.

### Layer 1: Config Fields (structured facts)
Programmatically readable by api.php. The bot draws from these automatically.
- `phone`, `email`, `address`, `hours`, `companyName`
- `greeting` / `greetings` (time-aware)
- Colors, position, sizing

**Never duplicate config fields in the KB.** If hours are in the config, they\'re not in
the KB. If phone is in the config, it\'s not in the KB. The system reads config directly.

### Layer 2: KB (knowledge base)
Conversational knowledge the bot draws from in response to questions. Built from the live
site\'s core pages — not SEO pages, not old drafts, not memory.

**For 8th Street KB source pages:**
index, foundation, second-sites, porch, curb-appeal, google-ads, lead-management, about,
faq, claude-consulting

**For law firm KBs:**
homepage, practice area pages, about, FAQ, any team/bio pages

**KB structure:**
```
## ABOUT [FIRM]
[2-3 sentences — what they do, who they serve, where]

## SERVICES
[List with 1-2 sentence descriptions each]

## WHY [FIRM]
[Credentials, experience, differentiators]

## FAQ
[15-25 Q&A pairs covering common questions]

## CONTACT AND HOURS
[Not needed if phone/email/address/hours are in config fields]
```

### Layer 3: Prompt (behavior rules)
Universal behavior the bot follows regardless of what\'s in the KB. Tone, length, what to
do, what not to do.

```
You are [role] for [Business Name].

## RESPONSE LENGTH
2-3 sentences max. Plain prose only — no bullet points, no bold, no headers.

## WHAT YOU CAN DO
[specific helpful actions]

## WHAT YOU CANNOT DO
[specific restrictions — no legal advice, no medical opinions, no case predictions]

## CONTACT
[how to connect — always offer both call AND callback option]
After hours: [what happens]

## TONE
[specific tone guidance for this client\'s audience]

## EXAMPLES
[5-8 sample Q&A pairs tuned to this client\'s actual questions]
```

---

## Monthly KB Refresh Process

**The rule:** Site copy is the source of truth. Rebuild from live files, not from memory
or old drafts.

**Process:**
1. Export public_html as zip (or use zip already on hand)
2. Extract core pages, strip PHP/HTML with Python, get clean text
3. Rebuild KB sections from actual page content
4. Validate JSON
5. Deploy to `gateway/clients/free/[clientid].json`

**Why zip → extract → rebuild (not manual editing):** Pulling from live files catches drift
automatically. Manual KB editing creates divergence from the actual site over time.

**What not to update manually:** Config fields. Those update via direct config edit. The KB
refresh is for the `"kb"` string only (and the `"prompt"` string if tone needs adjustment).

---

## Config File Structure (Schema v2.0)

All configs live at: `gateway/clients/free/[clientid].json`

```json
{
  "plan": "legal",
  "name": "Widget header name",
  "companyName": "Full company name",
  "status": "AI • Confidential",
  "timezone": "America/Chicago",
  "phone": "(000) 000-0000",
  "email": "contact@firm.com",
  "address": "123 Main St, City, IL 00000",
  "hours": {
    "monday": "08:30-17:00",
    "tuesday": "08:30-17:00",
    "wednesday": "08:30-17:00",
    "thursday": "08:30-17:00",
    "friday": "08:30-17:00",
    "saturday": "closed",
    "sunday": "closed",
    "notes": "After-hours handling note"
  },
  "greeting": "Single greeting string OR use greetings object",
  "greetings": {
    "business_hours": { "hours": "08:30-17:00", "message": "..." },
    "evening": { "hours": "17:00-21:00", "message": "..." },
    "overnight": { "hours": "21:00-08:30", "message": "..." }
  },
  "placeholder": "Type your question...",
  "primaryColor": "#hex",
  "accentColor": "#hex",
  "position": "right",
  "bubbleSize": 60,
  "windowWidth": 380,
  "windowHeight": 520,
  "notifyEmail": "alerts@8thstreetsolutions.com, client@email.com",
  "notifyPhone": "",
  "bubbleText": "Short tap driver — 2-4 words",
  "exitGreeting": "inactive — v2.17",
  "exitQuickReplies": ["Option 1", "Option 2", "Option 3"],
  "quickReplies": ["Reply 1", "Reply 2", "Reply 3"],
  "pageTeasers": {
    "/page-slug": "Teaser text specific to this page\'s topic"
  },
  "topic": "DUI Defense",
  "adviceType": "legal",
  "exampleQuestion": "Can my DUI be challenged?",
  "prompt": "Full system prompt...",
  "kb": "Full knowledge base..."
}
```

**Schema v2.0 rules:**
- `phone`, `email`, `address`, `hours`, `companyName` required on all new configs
- `api.php` accepts both `prompt` and `systemPrompt` field names
- Time-aware greetings use `greetings` object — business_hours/evening/overnight keys
- Single `greeting` string still works if no time-awareness needed
- `notifyEmail`: always use authenticated sender addresses (alerts@, leads@, logs@) —
  never personal mailboxes. Comma-separated for multiple recipients.
- Config filename must exactly match clientId in widget script tag
- Full file replacements are safer than incremental edits for configs

---

## Widget Embed Code

Standard embed (place in `footer.php` for site-wide presence):
```html
<script src="https://8thstreetsolutions.com/gateway/widget.js"
        data-client="[clientid]"
        data-gateway="https://8thstreetsolutions.com/gateway/api.php">
</script>
```

Demo pages with auto-open:
```html
<script src="https://8thstreetsolutions.com/gateway/widget.js"
        data-client="[clientid]"
        data-gateway="https://8thstreetsolutions.com/gateway/api.php"
        data-auto-open="true">
</script>
```

**No Porch on Google Ads landing pages.** Dedicated landing pages keep conversion path
clean — Porch creates a competing CTA. Standard site pages with Porch are fine; dedicated
Ads landing pages are not.

---

## Prompt Writing Guide

**Before writing any Porch responses, prompts, or FAQ content:** read `bad-examples.md` in this directory. It shows what failure looks like across 8 common patterns.

### Core Rules
- **2–3 sentences max** — scared people don\'t read paragraphs
- No markdown in responses — no bold, bullets, headers, numbered lists. Plain prose only.
- Help first, never push for contact info
- Never give legal advice, medical advice, or case/treatment opinions
- Match their energy — casual if casual, detailed if detailed
- Sound human, not like a bot reading a script
- Never say "I\'m an AI" unless directly asked. Always redirect to being helpful.

### The Callback Pattern (All Hours)
Always offer two paths — call them OR they leave a number. Both options, every time.

```
Always offer:
- "Call [phone]" — they reach out
- "Leave your number here and [name] will call you" — they get called

Don\'t push one option. Don\'t assume they want to call.
```

### Greeting Calibration
The greeting is the first impression. It sets tone and determines whether they type.

**Overnight pattern:**
- Acknowledge the time — they know it\'s late
- Name what they\'re probably dealing with — shows you understand
- Offer both paths: answer questions now OR leave number for callback
- Never ask them to wait until morning without giving something useful first

**Business hours pattern:**
- Warmer, slightly more formal
- Direct offer to help
- Same two-path CTA

**Audit opening lines first when tuning.** The greeting is always the highest-leverage
change.

### Writing FAQs That Work
- Use exact language prospects use — not legal/medical terminology
- Cover: what happens, what it costs, what options exist, what to do next
- Include objection-handling: "I can\'t afford a lawyer", "Is it worth fighting?", "Do I
  have a chance?"
- Every FAQ answer ends with a natural next step or offer to help further
- Answers stand alone — no "as I mentioned" or forward references

---

## Common Issues

**Widget not loading:**
- Check browser console for errors
- Verify data-client matches config filename exactly (case-sensitive)
- Check config JSON is valid (no trailing commas, no unescaped apostrophes)

**Exit intent disabled globally (v2.17):**
- widget.js v2.17 removed exit intent entirely
- Do not troubleshoot exit intent firing — it won\'t fire
- `exitGreeting` and `exitQuickReplies` fields are harmless but inactive

**SVG icons not visible:**
- Add `!important` to fill rules in widget.js CSS — some sites have aggressive CSS resets

**Lead emails not sending:**
- Verify `notifyEmail` in config uses authenticated sender
- Check PHP mail() log on server
- Verify account is not in `OPEN_EMAIL_SUPPRESSED` array in api.php

**5xx on generate-porch.php:**
- Check `porch-debug.log` at public_html root

**SiteGround server path (always use this):**
- Home: `~` = `/home/u3063-yznvsscqgtmo/`
- Public HTML: `~/www/8thstreetsolutions.com/public_html/`
- Never use `/home/8thstreet/public_html/` — wrong path

**Crontab:**
- Managed through SiteGround Site Tools → Devs → Cron Jobs
- Do not attempt `crontab -e` — it will fail on SiteGround shared hosting

---

## Batch Update Workflow

**Monday Tuneup:** Batch changes weekly — no piecemeal edits. Full file replacements safer
than incremental. Always test after deploy.

**Testing checklist after any config change:**
1. Widget loads on site
2. Opening message appears correctly
3. Time-aware greeting shows right message for current time of day
4. Send test message — verify tone and length
5. Verify lead email received if test conversion submitted
6. Check dashboard — confirm conversation logged

---

## Demo Architecture

**Demo subdomain:** `demo.8thstreetsolutions.com`
- `robots.txt` blocks all indexing on subdomain
- Each demo at `demo.8thstreetsolutions.com/[slug]/`
- `build.php` — John\'s self-serve demo generator (LinkedIn workflow)

**Two demo types — both live under `demo/[slug]/`:**

**Type 1 — Porch-Only Demo** (prospect has a site, evaluating Porch only)
Single `index.php` + `assets/styles.css` + client logo. Fast to build. Porch is the whole
story.

**Type 2 — Full Foundation Demo** (prospect needs site AND Porch)
Multi-page Foundation site with Porch embedded. More build time, shows full package.

**Canonical template:** `demo/_porch-demo-template/` — source of truth for Type 1.
Copy from here. Never copy from `demo/aquarious/` (deployed client config, not template).

**To deploy a new demo:**
1. Create `/gateway/clients/free/[clientid].json` config first
2. Create `/demo/[slug]/` folder on SiteGround
3. Upload `index.php` (filled config block) + `assets/styles.css` + `assets/logo.jpg`
4. Test: widget loads, responds, colors match brand
5. Send Jim the URL — Jim sends to prospect

**CSS is shared across all demos** — same `styles.css`, colors overridden via CSS variables
in `<style>` block. No per-client CSS file needed.

---

## api.php Version History

| Version | Change |
|---------|--------|
| v1.32 | First conversation email — jim@ only, minimal format, John removed |
| v1.31 | No-lead conversation summary emails — jim@ only on session close |
| v1.30 | message_sent analytics event — is_first_message flag included |
| v1.29 | Spam keyword filtering — SPAM_KEYWORDS, isSpamMessage() before Claude API |
| v1.28 | Email suppression per account (OPEN_EMAIL_SUPPRESSED array) |
| v1.27 | Exit intent email suppression |
| v1.26 | Widget open emails on every deliberate open + first interaction email |
| v1.25 | Medical and legal guardrails (HOUSE_RULES #9, #10) |
| v1.24 | Spam email filtering (BLOCKED_EMAILS array) |
| v1.23 | IP filtering for internal testing (EXCLUDED_IPS) |
| v1.22 | Config endpoint expansion + page context in system prompt |
| v1.20 | Time-aware greetings by client timezone |
| v1.19 | Removed "Clicked to open" emails; logs only if user typed |
| v1.18 | Rolled back From addresses to authenticated senders |
| v1.13 | Conversation tracking for usage/billing |

**Current:** v1.32 (April 1, 2026) · Widget: v2.17 · Model: claude-sonnet-4-20250514

---

## Porch Daily Agent (Live)

**File:** `gateway/porch-daily-agent.php`
**Trigger:** Cron `0 13 * * *` (6:00 AM Mountain / 1:00 PM UTC)
**Retry logic:** Up to 4 attempts on API 529/500/503, with 15/30/60s delays
**To add an account:** Add one line to `$ACTIVE_ACCOUNTS` array in porch-daily-agent.php
**Output:** Daily email to jim@ with summary, patterns, flagged issues, and ready-to-paste
CC prompt for any config changes needed

**What the daily agent surfaces:**
- Conversation volume and open rates (bot traffic filtered out)
- Common question patterns not covered by KB
- Prospect questions that got weak or no answers
- After-hours volume (useful for Ads scheduling decisions)
- Any config anomalies

**Human touchpoint:** Jim reviews email, acts on flagged items via CC prompt included in
report. KB updates that come from daily agent are deployed via full config replacement.

---

## Agent Automation — Next Steps

**Live:** Porch daily agent — log review, pattern flagging, daily email
**Near-ready:** KB update drafts in the daily email, ready for Jim to review and deploy
**Deferred:**
- Config validator agent — validates JSON schema before deploy
- Porch open-rate trend dashboard — analytics data exists, display layer not built
- Config editor tool for John — build after account base grows (~5+ accounts)
  Will live in PathAcross dashboard. Lets John update hours, tone, add a service without
  touching files directly.

---

## Porch SEO Hub (8thstreetsolutions.com/porch/)

57+ pages organized by template. Six data files + six templates + stub files.

**Categories:** Core features, use cases, free/pricing, by industry (18+ pages), by
platform (5), comparisons (10).

**Template architecture:**
- `/porch/data/` — 6 data files
- `/porch/templates/` — 6 template files
- Stub pages include the right template with `$slug` set

**Leave structure alone.** Meta/deck/schema pass deferred until legal-marketing shows
click movement in GSC. CTA: all Porch SEO pages point to homepage signup form.

---

## PHP File Style (for patch scripts)

Section headers use box-drawing characters:

```python
# // ─── SECTION NAME ──────────────────────────────────────────
```

Use regex to match these anchors, not exact string counts:

```python
re.search(r'// ─{3} SECTION ─+', src)
```

Never hardcode the exact number of dashes — counts vary by section.

---

*Porch — AI Chat Widget System*
*Methodology only. Client configs in clients/[client-id]/porch.md*
*Scales to any number of clients without skill changes.*
