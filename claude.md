# 8th Street Solutions — Claude Code Context
## Last updated: March 30, 2026 (claude.md improvements pass)

---

## What This Is

8th Street Solutions builds AI-powered web presence for professionals and businesses. Two people: Jim (technical + strategic build) and John (sales, LinkedIn, client relationships). All execution happens via Claude Code + SSH to SiteGround. No copy/paste through File Manager.

---

## The Six Products

This is the complete product line. Everything on the site, in the skills, and in client conversations maps to one of these:

1. **Foundation** — 50-60 page marketing websites. Fast, clean, PHP-based, no WordPress. Built for ranking and converting. Any professional or business type.
2. **Second Sites** — Parallel Foundation site for clients keeping their existing site. Two models: (A) parallel marketing site on new domain, (B) community resource site where 8th Street owns the domain/traffic and a professional sponsors it.
3. **Porch** — AI chat widget that knows the client's practice/business. 30-day free trial. Converts after-hours visitors.
4. **Curb Appeal** — Ongoing SEO growth. GSC-driven. Content additions, page deepening, keyword expansion.
5. **Google Ads** — Full funnel paid traffic management.
6. **Lead Management** — Tools for handling/nurturing leads post-Porch. Being defined — content last priority.

Claude Consulting is a real service but not a primary product line. Lives in footer first column (inline link) + claude-consulting.php only. Not in nav, not in What We Build list.

---

## Audience

Primary sales target right now: **law firms** — DUI and criminal defense proven, other law firm types in progress. LinkedIn is John's main channel, which skews professional (lawyers, medical, engineering, financial). The product works for any professional or business — the marketing is broadening to match.

**The site speaks to all professionals and businesses now, not law-firms-only.** CD/DUI examples and case studies stay in — they're the proof — but the framing is broader.

---

## Site Structure — 8thstreetsolutions.com

### Nav (live)
Home / Foundation / Second Sites / Porch / Curb Appeal / Google Ads / Lead Management

### Core pages
- `/` — Homepage. Broad hero. "A System Not a Menu" service framing. No showcase section.
- `/foundation` — Foundation product page. Broad. CD/DUI examples stay, other verticals added later.
- `/second-sites` — Second Sites product page.
- `/porch/` — Porch hub. Already broad (57+ industry pages). Do not touch structure.
- `/curb-appeal` — Curb Appeal product page.
- `/google-ads` — Google Ads product page.
- `/lead-management` — Lead Management product page. Content last priority.
- `/about` — Jim and John only. Simple. No product list. Claude Consulting mentioned inline.
- `/faq` — 8 Q&As, open format, FAQPage schema.
- `/claude-consulting` — Page stays, not in nav. Footer first column inline link only.
- `/disclaimer` — Legal/privacy.

Note: `results.php` and `contact.php` are deleted from disk. Active redirects: `/results → /about`, `/results.php → /about`, `/contact → /`, `/contact.php → /`, `/services → /foundation`.

### SEO content hubs — DO NOT RESTRUCTURE
- `/legal-marketing/` — 24 CD/DUI keyword pages. Exact-match URLs, 1,200+ words, open FAQs, FAQPage schema. These are ranking. Leave them alone unless adding new pages or fixing bugs per the operations section below.

#### /legal-marketing/ Page Inventory

| Slug | Primary target query | Added |
|------|---------------------|-------|
| `criminal-attorney-marketing` | criminal attorney marketing | original |
| `criminal-defense-attorney-marketing` | criminal defense attorney marketing | original |
| `criminal-defense-google-ads` | criminal defense google ads | Mar 2026 |
| `criminal-defense-law-firm-marketing` | criminal defense law firm marketing | original |
| `criminal-defense-lawyer-marketing` | criminal defense lawyer marketing | original |
| `criminal-defense-lawyer-marketing-agency` | criminal defense lawyer marketing agency | original |
| `criminal-defense-lawyer-seo` | criminal defense lawyer seo | original |
| `criminal-defense-marketing` | criminal defense marketing | original |
| `criminal-defense-marketing-agency` | criminal defense marketing agency | Mar 30 2026 |
| `criminal-law-firm-marketing` | criminal law firm marketing | original |
| `criminal-lawyer-advertising` | criminal lawyer advertising | original |
| `criminal-lawyer-marketing` | criminal lawyer marketing | original |
| `criminal-lawyer-marketing-agency` | criminal lawyer marketing agency | original |
| `digital-marketing-criminal-defense-attorneys` | digital marketing for criminal defense attorneys | original |
| `dui-attorney-marketing` | dui attorney marketing | original |
| `dui-law-firm-marketing` | dui law firm marketing | Mar 30 2026 |
| `dui-lawyer-google-ads` | dui lawyer google ads | Mar 2026 |
| `dui-lawyer-marketing` | dui lawyer marketing | original |
| `dui-lawyer-search-engine-optimization` | dui lawyer search engine optimization | original |
| `dui-lawyer-seo` | dui lawyer seo | Mar 30 2026 |
| `internet-marketing-for-criminal-defense-lawyers` | internet marketing for criminal defense lawyers | original |
| `marketing-for-criminal-defense-attorneys` | marketing for criminal defense attorneys | original |
| `search-engine-optimization-criminal-defense-attorneys` | search engine optimization for criminal defense attorneys | original |
| `index` | criminal defense & dui lawyer marketing (hub) | original |

**When adding a new page:** add a row here with the date, add a card to `index.php` grid (see anchor below), add URL to `sitemap.xml` (priority 0.9 for primary query pages, 0.8 for secondary).
- `/cities/` — 10 CD/DUI city pages. Leave them alone.
- `/porch/` — 57+ Porch SEO pages. Template-driven. Leave structure alone.

### Future hubs (plan only, don't build yet)
- `/medical-marketing/` — equivalent to /legal-marketing/ for medical/healthcare
- Additional city hubs for non-law verticals

---

## Legal Marketing Curb Appeal — Operations

### GSC Report Filter
The weekly scored report is at `~/gsc-agent/reports/scored_YYYY-MM-DD.txt`.
The **8thstreet section is lines 1-41** of the report. Lines 42+ are DUI (Andy's site) and PathAcross — ignore those entirely for 8th Street sessions.

### Vertical Filter Rule
Build new `/legal-marketing/` pages **only** for queries containing:
- DUI, DUI attorney, DUI lawyer, DUI law firm
- criminal defense, criminal lawyer, criminal attorney, criminal law firm

Skip everything else regardless of score — chatbot, healthcare, wix, and other verticals are noise from the Porch pages and are not in scope for Curb Appeal sessions on this site.

### New Page Build Process
1. Check query against the page inventory table above — if a page already exists for that slug pattern, skip it
2. Write the PHP file locally using the template spec below, SCP to server, execute write script
3. Fix `$current_page` to `'services'` (see known bug below)
4. Add card to `legal-marketing/index.php` at the `<!-- NEW_CARDS_INSERT_BEFORE -->` marker
5. Add URL to `sitemap.xml` after the last existing `legal-marketing/` entry
6. Update the page inventory table above with slug + date
7. Run `php -l` on every changed file — halt on any failure
8. `touch .htaccess` to flush SiteGround dynamic cache

### index.php Grid Insert Anchor
New guide cards go inside the 2-column grid div. The stable insertion point is the comment marker:
```
<!-- NEW_CARDS_INSERT_BEFORE -->
```
Insert the new `<a>` card block immediately **before** this comment. Card template:
```html
                <a href="/legal-marketing/[slug]" style="display:block; padding:24px; border:1px solid #e2e8f0; border-radius:8px; text-decoration:none; color:inherit;" onmouseover="this.style.borderColor='#1a3a52'" onmouseout="this.style.borderColor='#e2e8f0'">
                    <strong style="display:block; color:#1a3a52; margin-bottom:6px;">[Card Title]</strong>
                    <span style="font-size:0.88rem; color:#64748b;">[One-sentence description]</span>
                </a>
```
Also update the deck line count: `p.section-deck` under "Marketing Guides for Criminal Defense & DUI Firms" H2 — increment the guide count.

### /legal-marketing/ Page Template Spec
Every page in this hub follows this exact pattern:

```php
<?php
$basePath = '../';
$current_page = 'services';        // ALWAYS 'services' — never 'porch'
$page_title = '[Title] | 8th Street';   // 50-65 chars
$page_description = '[Desc]';           // 140-155 chars, no em dashes
$canonical_url = 'https://8thstreetsolutions.com/legal-marketing/[slug]';

$schemaMarkup = '{
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [ ... 10 Question objects ... ]
}';    // raw JSON string — NOT json_encode(), NOT a PHP array

require_once '../includes/header.php';
?>

<section class="hero hero-short">
    <div class="container">
        <div class="service-label">[Short Label]</div>
        <h1>[H1]</h1>
        <p class="hero-sub">[2-3 sentence lede]</p>
    </div>
</section>

<section class="section bg-white">
    <div class="container">
        <div class="problem-text">

            <h2>[Section heading]</h2>
            <p class="section-deck">[Bold deck line — one sentence]</p>
            <p>[Body paragraph]</p>
            ...

            <h2>Frequently Asked Questions About [Topic]</h2>
            <p class="section-deck">[FAQ intro deck]</p>

            <h3>[FAQ question]</h3>
            <p>[Answer — open, no accordion, 2-4 sentences]</p>
            ... (10 FAQ pairs minimum) ...

        </div>
    </div>
</section>

<?php require_once '../includes/footer.php'; ?>
```

**Content requirements:**
- 1,200+ words minimum
- 10 FAQs in schema + matching open h3/p pairs in HTML body
- `p.section-deck` immediately below every H2 (selector is `p.section-deck`, not `.section-deck` — specificity fix)
- At least 2 internal links to related pages (`/legal-marketing/dui-lawyer-google-ads`, `/porch`, `/foundation`, etc.)
- No PHP closing `?>`
- Apostrophes in single-quoted strings must be escaped: `'`

### Known Bug: $current_page = 'porch'
Several pages in `/legal-marketing/` were built with `$current_page = 'porch'` instead of `'services'`. This affects nav highlighting. **Check every page you touch.** Pages confirmed fixed as of March 30 2026: `search-engine-optimization-criminal-defense-attorneys.php`, `criminal-lawyer-advertising.php`, `criminal-defense-lawyer-marketing-agency.php`. Audit others if touching them.

## GSC / SEO Rules

- **Minimal disruption.** Existing pages do not get rewritten or moved.
- URL changes = 301 redirects in .htaccess. Never break a URL.
- New pages are additions only.
- Every content page: 1,200+ words, open FAQs (no accordion), FAQPage schema, internal links.
- AISEO: FAQ sections are where AI citation happens. Keep them open and specific.
- Do not cross-link between controlled sites in YMYL territory.
- GSC submission: 10 URL inspections/day limit. Prioritize new pages getting impressions but not indexed.
- GSC automation: cron Monday 6am, scored report to jim@8thstreetsolutions.com.

---

## Header / Footer State (live as of March 27, 2026)

### header.php — Organization Schema (live)
```
description: "AI-powered websites and marketing for professionals and businesses. We build fast, clean web presence that ranks, converts, and compounds — and we embrace AI so our clients don't have to."
slogan: "We embrace AI so you don't have to."
knowsAbout: Foundation sites, Curb Appeal SEO, Porch AI Chat, Second sites, Lead management,
            Google Ads, Claude AI implementation, criminal defense and DUI attorney marketing
            (retained as proof point), meeting clients at vulnerable threshold moments
```

### footer.php (live as of March 27, 2026)
- **3-column layout** — inline style: `grid-template-columns: 1.4fr 1fr 1fr`
- Column 1: Site name + Claude mention with inline link to /claude-consulting + location
- Column 2: What We Build (6 services, Claude Consulting removed from list)
- Column 3: Contact (phone/email) + FAQ + About Us + small dimmed links for Legal Marketing Guides (/legal-marketing) and Service Areas (/cities)
- "Built with Claude AI by Anthropic" line — **removed**
- Textarea font fixed: `font-family:inherit` added to inline style

### index.php (live as of March 27, 2026)
- Hero sub shortened — one sentence, free build offer only
- "The Problem We Solve" section tightened — deck line removed, two paragraphs, no repetition
- Service section renamed "A System, Not a Menu" with framing paragraph above cards
- "See What We've Built" showcase section — **removed**
- hasOfferCatalog WebPage schema block added

### about.php (live as of March 27, 2026)
- Full rewrite — simple, industry-agnostic
- Jim + John intro paragraphs only
- Team grid preserved
- All old sections (Why Foundation, Why Porch, We Work With Others) — **removed**
- One closing line pointing to reach out, footer form handles the rest

---

## March 27, 2026 — Full Site Meta/Deck/Schema Pass

All 31 pages (main nav + legal-marketing + porch hub) received:
- Rewritten meta titles (50-60 chars, keyword-led, click copy, | 8th Street brand signal)
- Rewritten meta descriptions (140-155 chars, problem-focused, mild CTA)
- Deck lines under every H2 via `.section-deck` CSS class

### section-deck CSS (in assets/styles.css)
```css
p.section-deck {
    font-size: 1rem;
    font-weight: 700 !important;
    color: #475569;
    margin-top: 0.4rem;
    margin-bottom: 0.4rem;
    line-height: 1.5;
    display: block;
}
```
**Note:** Selector is `p.section-deck` not `.section-deck` — specificity fix for conflict with `.problem-text p`. font-weight requires `!important` due to broad `p {}` rule in stylesheet.

### Schema added March 27
- Service schema block on all 6 hub pages (before footer include)
- WebPage + hasOfferCatalog schema on index.php
- FAQPage schema already present on all 21 legal-marketing pages — none duplicated
- Porch index SoftwareApplication schema preserved

### GSC reindex — 31 URLs submitted (3-day batch, 10/day)
Porch subfolder pages (/porch/*.php) NOT touched — scope for future session once legal-marketing pages show click movement.

---

## Skills Architecture

### Single Source of Truth
All skill files live at: ~/skills/user/ on this server.
Full path: /home/customer/skills/user/

Read skills from there at the start of every session. Never rely on /mnt/skills/user/ — that is a session-dependent mount that is not consistent or reliable.

### Active Skills
| Skill | Location | Purpose |
|-------|----------|---------|
| foundation | ~/skills/user/foundation/SKILL.md (routing brain) + clients.md, new-build.md, schema.md, standards.md, verticals.md | All Foundation site builds |
| foundation-second-sites | ~/skills/user/foundation/SKILL.md | Second site builds (consolidated) |
| foundation-community-resource | ~/skills/user/foundation/SKILL.md | Community resource sites (consolidated) |
| porch | ~/skills/user/porch/SKILL.md | Porch widget — configs, KB, prompt |
| curb-appeal | ~/skills/user/curb-appeal/SKILL.md | GSC-driven SEO growth |
| google-ads | ~/skills/user/google-ads/SKILL.md | Paid traffic management |
| work-smarter | ~/skills/user/work-smarter/SKILL.md | Automation lens — read every session |
| clean-project | ~/skills/user/clean-project/SKILL.md | End of cycle cleanup |
| pathacross | ~/skills/user/pathacross/SKILL.md | PathAcross strategic work |
| spaces | ~/skills/user/spaces/SKILL.md | PathAcross Spaces builds |
| source-alignment | ~/skills/user/source-alignment/SKILL.md | North star — consult at decision points |
| skill-creation | ~/skills/user/skill-creation/SKILL.md | Building and updating skills |

### Source of Truth Hierarchy
Skills on SiteGround > Live site > Thread content > Everything else

### Session Start Rule
Every CC session begins with:
1. Read ~/skills/user/work-smarter/SKILL.md
2. Read the skill(s) relevant to today's task
3. Read claude.md (this file)
Then execute. Do not rely on memory from prior sessions.

---

## Active Clients

| Client | Site | Products | Notes |
|--------|------|----------|-------|
| Andy Sotiropoulos | illinois-dui-defender.com | Foundation + Porch + Google Ads | DUI, Illinois |
| Heart of Illinois / Maureen | heartofillinoiscriminaldefense.com | Foundation + Porch + Google Ads | CD+DUI, Illinois |
| Aquarius Lawyers (AU) | demo at 8thstreetsolutions.com/demo/aquarious/ | Porch demo | Julie Bargenquast (PM), Katherine Hawes (principal). Demo sent Mar 2026. Uses `aquariouslawyers` config. |
| Melnick Timmerman | demo at 8thstreetsolutions.com/demo/melnicktimmerman/ | Foundation demo | CO law firm. Demo delivered, awaiting response. |

### Winding down (email suppression active — no open/interaction emails to jim@)
- Grand Medical Specialists — staying live as courtesy
- John N. Campbell MD — staying live as courtesy

---

## Porch Platform State

- **gateway api.php v1.28**, widget.js **v2.17**
- Model: claude-sonnet-4-20250514
- Configs: `/gateway/clients/free/[clientId].json`
- Config schema: **v2.0** — all configs include `phone`, `email`, `address`, `hours` (structured) fields
- Filename must exactly match clientId in widget script tag
- Self-serve: `/get-porch.php → /generate-porch.php → /porch-ready.php`
- Dashboard: PathAcross dashboard at pathacross.com/dashboard.php?token=pa-monitor-2026 (fed by dashboard-api.php)
- Exit intent: **disabled globally** in widget.js v2.17 — mouseout listener removed, handleExitIntent() inert
- Pricing: 30-day free trial → paid tiers. No permanent free tier.

## Porch KB Architecture

Three accounts where KB maintenance matters: 8thstreet, heart-of-illinois, illinois-dui-defender.

**Three-layer split:**
- **Config fields** — structured facts the system reads programmatically (phone, email, address, hours, colors). Never duplicate these in the KB.
- **KB** — conversational knowledge the bot draws from. Built from live site core pages only (not SEO pages). For 8th Street: index, foundation, second-sites, porch, curb-appeal, google-ads, lead-management, about, faq, claude-consulting.
- **Prompt** — universal behavior rules (tone, length, what to do/not do).

**Monthly refresh process:**
1. Export public_html as zip (or use zip already on hand for the session)
2. Extract core pages, strip PHP/HTML with Python, get clean text
3. Rebuild KB sections from actual page content — not from memory or old drafts
4. Validate JSON, deploy to `gateway/clients/free/[clientid].json`

**Why zip → extract → rebuild (not manual editing):**
Site copy is the source of truth. The KB should reflect what the site actually says, not what we think it says. Pulling from the live files catches drift automatically.

**Config editor tool for John** — deferred. Build after account base grows (~1 week from March 2026). Will live in PathAcross dashboard. Lets John update hours, tone, or add a new service without touching files directly.

### Porch Daily Agent (live as of March 25, 2026)
- File: `gateway/porch-daily-agent.php`
- Cron: `0 13 * * *` (6:00 AM Mountain / 1:00 PM UTC)
- Reads: yesterday's conversation logs + analytics for all active accounts
- Filters: bot traffic removed before open rate calculation
- Output: daily email to jim@ with summary, patterns, flagged issues, ready-to-paste CC prompt for any config changes
- Retry logic: up to 4 attempts on API 529/500/503, with 15/30/60s delays
- To add account: add one line to `$ACTIVE_ACCOUNTS` array in porch-daily-agent.php

### Cron jobs on server (all active)
| Job | Schedule | Purpose |
|-----|----------|---------|
| porch-daily-agent.php | `0 13 * * *` | Daily Porch log review + email report |
| gsc-agent/fetch_gsc.py + score + email | `0 6 * * 1` | Monday GSC pull, score, email report |
| gsc-agent/work_order.py | `30 6 * * 1` | Monday GSC work order (runs after fetch) |

### Config schema v2.0 — new fields added this cycle
All active configs now include:
- `phone` — top-level, programmatically readable
- `email` — top-level
- `address` — top-level
- `hours` — structured object with per-day hours + notes field
- `companyName` — now required on all configs

---

## Build Rules

- Match existing site exactly — same includes, same CSS classes — before modifying anything
- Build ONE test page first, get approval, then batch
- Always create `claude.md` at project root first time opening a project
- PHP include paths: root files use `include 'header.php'`, subfolder files use `include '../header.php'`
- basePath: always include `?: '/'` fallback for home links
- Apostrophes must be escaped in ALL single-quoted PHP strings — this includes schema strings, $page_title, $page_description, and any other variable assignment. Unescaped apostrophes (straight or curly) break PHP parsing. Use \' inside single-quoted strings.
- Email senders: use alerts@/leads@/logs@ — never personal mailboxes
- Batch changes weekly (Monday Tuneup) over piecemeal when possible
- Full file replacements safer than incremental for Porch configs

---

## Technical Reference

**SSH:** u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com port 18765, key ~/.ssh/id_ed25519

**File structure:**
- Core pages: `~/www/8thstreetsolutions.com/public_html/[page].php`
- Porch configs: `~/www/8thstreetsolutions.com/public_html/gateway/clients/free/[clientId].json`
- Porch agent: `~/www/8thstreetsolutions.com/public_html/gateway/porch-daily-agent.php`
- Agent log: `~/www/8thstreetsolutions.com/public_html/gateway/logs/agent.log`
- Demo sites: `~/www/8thstreetsolutions.com/public_html/demo/[slug]/`
- GSC agent: `~/gsc-agent/`
- CC context file: `~/www/8thstreetsolutions.com/public_html/claude.md` (this file)

**Note on server path:** SiteGround home is `~` = `/home/u3063-yznvsscqgtmo/`. Public HTML is at `~/www/8thstreetsolutions.com/public_html/` — NOT `/home/8thstreet/public_html/`. Use the `~/www/` path in all SSH commands.

**Demo subdomain:** `demo.8thstreetsolutions.com`
- `build.php` — John's self-serve Porch demo generator (LinkedIn workflow)
- `robots.txt` — blocks all indexing on the subdomain
- Each demo at `demo.8thstreetsolutions.com/[slug]/`
- Canonical template source: `demo/_porch-demo-template/` — copy from here for manual builds

---

## March 30, 2026 — Curb Appeal Session

### New pages built
- `/legal-marketing/dui-lawyer-seo` — GAP score 143 (highest priority). Targets: "dui lawyer seo" (143 impr), "seo for dui attorneys" (49), "seo for dui lawyers" (35), "seo for dui lawyer" (22). 10 FAQs, FAQPage schema, internal links to dui-lawyer-google-ads, porch, foundation.
- `/legal-marketing/criminal-defense-marketing-agency` — GAP score 35. Targets: "criminal defense marketing agency". 10 FAQs, FAQPage schema, internal links to criminal-defense-google-ads, porch.
- `/legal-marketing/dui-law-firm-marketing` — GAP score 22. Targets: "dui law firm marketing". 10 FAQs, FAQPage schema, internal links to dui-lawyer-google-ads, porch.

### Fixes on existing pages
- `search-engine-optimization-criminal-defense-attorneys.php`: fixed `$current_page = 'porch'` → `'services'`
- `criminal-lawyer-advertising.php`: fixed `$current_page = 'porch'` → `'services'`

### Supporting updates
- `legal-marketing/index.php`: added 3 new guide cards; updated deck from "20 guides" to "24 guides"
- `sitemap.xml`: added 3 new URLs (priority 0.9 for dui-lawyer-seo, 0.8 for the other two)
- php -l: 7 files, 0 errors
- Cache flushed via .htaccess touch
- Do NOT submit to GSC — Jim handles that after visual review

### Remaining GAP opportunities (CD/DUI legal marketing only, deferred)
- "dui attorney marketing" (101 impr, pos 73) — page exists at dui-attorney-marketing.php, but ranking at 73. Needs content audit/deepening.
- "criminal defense lawyer seo" (73 impr, pos 79) — page exists at criminal-defense-lawyer-seo.php but not covering this well. Consider adding "seo for criminal defense lawyers" as alternate h1/h2 angle.
- "digital marketing for criminal defense attorneys" (61 impr, pos 56) — page exists, pos 56, possible depth issue.
- ALMOST_THERE: "dui lawyer google ads" (44 impr, pos 22), "dui attorney google ads" (40 impr, pos 28) — existing pages likely need inbound link authority, not content changes.

## What's Parked (don't build without a dedicated thread)

- /medical-marketing/ and other vertical hubs
- Second Sites pricing + positioning
- Lead Management product definition
- Prospecting automation upgrade (full auto-build + delivery pipeline)
- Porch subfolder pages meta/deck/schema pass (57+ pages — same meta/deck/schema treatment as March 27 main site pass — do after legal-marketing shows click movement in GSC)
- Testimonials in footer — John collecting, add when ready
- Depression vertical (PathAcross/Spaces)
- Config validator agent (Agent 2 — deferred, do after daily agent proves out)
- Porch open-rate trend dashboard (analytics data exists, display layer not built yet)