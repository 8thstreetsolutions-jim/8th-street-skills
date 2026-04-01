<!--
version: 2.1
date: 2026-04-01
source: Added Meta Title and Description Standards section (2026)
-->

---
name: curb-appeal
description: "GSC-driven organic growth loop for any website. Use this skill whenever the
user wants to improve organic search rankings, analyze Google Search Console data, identify
content gaps, deepen existing pages, build new pages, or prioritize what to work on next.
Triggers on GSC, search console, impressions, organic traffic, ranking, pages not getting
clicks, what to add to a page, what pages to build, content gaps, ranking improvements,
site audit. Use for ANY site type — law firms, service businesses, content sites, local
businesses. Primary skill for all on-page and site architecture SEO work. Does NOT cover
GBP posts, directory citations, or review velocity — those are off-page."
---

# Curb Appeal — GSC Organic Growth Loop

**The core motion:** GSC shows what real people search for. Score the opportunities.
Execute the top moves. Reindex. Update the client file. Repeat.

Site-agnostic. Identical workflow for a DUI firm, HVAC company, content hub, or local
service business.

**Scope:** On-page content and site architecture only. GBP, reviews, and directory
citations are off-page — separate skill.

**Client-specific rules and history in:** `clients/[client-id]/curb-appeal.md` on
SiteGround. Read that file before any client session.

---

## The Four Moves

Every GSC opportunity resolves to exactly one of:

1. **Deepen** — page exists, has impressions, not ranking or not getting clicked. Add FAQs,
   citations, credential-in-context content. Target the clustering queries explicitly.
2. **Build** — query cluster has real volume, no page exists. Build the page.
3. **CTR fix** — page is on page 1 but CTR is low. Content is fine. Rewrite title/meta
   description only. Write as click copy.
4. **Differentiate** — page crawled but not indexed. Google sees it as near-duplicate. Fix
   is NOT technical — give the page a genuinely distinct primary angle (different channel,
   audience segment, topic focus). Resubmit after changes.

---

## Live GSC Agent — Monday Morning Feed

The GSC agent runs automatically. This is the session input for every 8th Street
curb-appeal session.

**Report location:** `~/gsc-agent/reports/scored_YYYY-MM-DD.txt`

**Report structure:**
- Lines 1–41: 8th Street data — use these
- Lines 42+: DUI client (Andy) and PathAcross — **ignore entirely in 8th Street sessions**

**Work order:** A second script runs at 6:30 AM Monday and emails a scored work order to
jim@. This is the starting point for the execution session — not the raw GSC export.

**Session trigger:** Jim brings the work order from the Monday email. Session begins with
"here\'s this week\'s work order" — read it, apply vertical filter, execute.

**Manual session fallback:** If work order isn\'t available, Jim exports GSC CSV manually.
Apply same scoring logic.

---

## Session Format

1. **Step 0 (optional):** Site audit if files just uploaded — catch bugs before SEO work
2. **Load work order or GSC export** — review scored opportunities
3. **Apply vertical filter** — read client file for what to build and what to skip
4. **Cluster by geography + topic** — before deciding on moves
5. **Execute all viable moves** — deepens, builds, CTR fixes, differentiations. No
   artificial limit per session.
6. **PHP quality gate** — `php -l` on every changed file. Halt on any failure.
7. **Cache flush** — `touch .htaccess` after all file changes
8. **Post-session debrief** — ask CC: what did claude.md tell you that was useful? What
   was awkward? Fold answers into claude.md same session.
9. **Output URL list** — clean block of new/changed URLs at end for Jim\'s GSC submissions.
   Do NOT submit to GSC — that\'s always the human step.
10. **Update claude.md on server** — page inventory, what was done, what moved, what\'s
    next. Required, not optional.

**GSC submissions are always human-initiated.** 10 URL inspection/day limit per Google account — not per site. All properties share the same daily quota. Jim handles after visual review of live pages.

**Session pacing:** Batch sessions (5–10 page changes, then pause 1–2 days) outperform
daily single edits. Let Google process before the next batch.

**Chat vs CC:** For 3–6 page builds, do the work in Claude Chat — write files, zip, user
uploads via SiteGround. Reserve CC for larger batches (7+ pages) or when SSH execution
is needed.

---

## Step 0 — Site Audit

Run after receiving a new site ZIP or batch of file uploads.

```bash
# PHP lint
php -l *.php includes/*.php locations/*.php

# Sitemap URL count
grep -c '<loc>' sitemap.xml

# Junk files
find public_html -name "*.zip" -o -name "*.bak" -o -name "php_errorlog"
```

**Manual checklist:**
- [ ] City slug matches folder name for every location
- [ ] No duplicate meta_title/meta_description across data arrays (data bleed bug)
- [ ] Sitemap URL count matches actual page count
- [ ] New pages from this session are in sitemap.xml
- [ ] basePath variable has `?: '/'` fallback on all pages

**If PHP errors found:** Fix before any SEO work. Broken pages return blank to visitors
and Googlebots.

**Common PHP parse error causes:**
- Smart apostrophes (`’`) inside single-quoted PHP strings — must be `\'`
- Unclosed string or array in `$schemaMarkup` variable
- `$current_page` set to wrong value (see Known Bugs below)

---

## Server Logs — Standard Weekly Input

Bring logs every session alongside the GSC export. Decisions without logs are missing
ground truth.

**What to send each session:**
- `logs/` folder — access log and error log
- `webstats/` folder — AWStats data
- Any `php_errorlog` found inside public_html subfolders

**What logs reveal that GSC cannot:**
- Pages gaining impressions but returning 500 errors
- Zero-traffic pages not worth deepening yet
- Broken includes and PHP warnings damaging crawlability
- Old URLs still being hit — redirect opportunities

**Review logs first, before scoring GSC. Fix errors before content work.**

```bash
# 500 errors by URL
grep ' 500 ' access.log | grep -oP '"GET [^"]*"' | sort | uniq -c | sort -rn | head -20

# 404 clusters
grep ' 404 ' access.log | grep -v "favicon\|\\.png\|\\.jpg\|\\.css" | grep -oP '"GET [^"]*"' | sort | uniq -c | sort -rn | head -20
```

---

## Scoring Opportunities

Score every query in the GSC export. Bucket by:

| Bucket | Criteria | Move |
|--------|----------|------|
| WIN | Position 1–10, CTR < expected | CTR fix |
| CLIMB | Position 11–30, impressions > 50 | Deepen existing page |
| BUILD | Position 31+, impressions > 100, no page | Build new page |
| GAP | Impressions > 50, position > 50, no page | Build new page |
| SKIP | Out of vertical, already done, zero impressions | Skip |

**Cluster before acting.** Multiple queries pointing to the same topic = one page, not
multiple.

**Check page inventory before building.** If a page exists for that slug pattern, skip the
build and consider a deepen instead. Claude.md page inventory table is the source of truth.

---

## Content Standards

**Every new page minimum:**
- 1,200+ words
- 10 FAQs with FAQPage schema (10 `Question` objects in schema + matching open h3/p pairs
  in HTML body)
- At least one statute citation or authoritative reference (law firm verticals)
- Internal link from hub index page
- Added to sitemap.xml same session

**Every deepen minimum:**
- At least 3 new FAQs targeting the specific clustering queries
- Credential-in-context where relevant (not just bio section)
- Meta title and description reviewed — rewrite if not click copy

**Title/meta standard:** At positions 50–85 the title and meta description are what get
evaluated when a page breaks page 1. Write them as click copy from day one.

**`p.section-deck` pattern (Foundation PHP sites):**
- Every H2 gets a `<p class="section-deck">` immediately below it
- Selector is `p.section-deck` not `.section-deck` — specificity fix for conflict with
  `.problem-text p`
- `font-weight: 700 !important` required — broad `p {}` rule overrides without it

---

## Hub Index Pattern

For any site with a content hub (e.g., `/legal-marketing/`, `/cities/`, `/porch/`):

**Stable insertion anchor in hub `index.php`:**
```html
<!-- NEW_CARDS_INSERT_BEFORE -->
```
Insert new `<a>` card blocks immediately before this comment. Never append to end of
grid — ordering breaks.

**When adding a page to a hub, always:**
1. Add the PHP file in the hub subdirectory
2. Add the card to `index.php` using the insertion anchor
3. Update the deck line count in `index.php` (e.g., "24 guides" → "25 guides")
4. Add URL to `sitemap.xml` using correct priority:
   - Primary query pages: `priority 0.9`
   - Secondary/supporting pages: `priority 0.8`
5. Update the page inventory table in `claude.md`

---

## Sitemap Standards

```xml
<url>
  <loc>https://[domain]/[slug]</loc>
  <lastmod>[YYYY-MM-DD]</lastmod>
  <priority>[0.9 or 0.8]</priority>
</url>
```

Priority guide:
- Homepage: 1.0
- Main hub pages (e.g., `/legal-marketing/`): 0.9
- Primary query pages: 0.9
- Secondary/supporting pages: 0.8
- Legal/utility pages: 0.5 or omit

Add new entries after the last existing entry in the same section — don\'t scatter.

---

## Known PHP Bugs — Check Every Session (Foundation Sites)

### `$current_page = \'porch\'` Bug
Several pages were built with `$current_page = \'porch\'` instead of `\'services\'`.
This affects nav highlighting.

**Check every Foundation page you touch.** Fix before any other changes.
Confirmed fixed as of March 30, 2026 on 8th Street:
- `search-engine-optimization-criminal-defense-attorneys.php`
- `criminal-lawyer-advertising.php`
- `criminal-defense-lawyer-marketing-agency.php`

Others may still have this bug — audit when touching them.

### Schema as PHP Array (Wrong)
`$schemaMarkup` must be a raw JSON string, NOT a PHP array passed to `json_encode()`.
The header include outputs it directly. Wrong pattern causes malformed JSON.

### Apostrophes in Single-Quoted Strings
All apostrophes in PHP strings must be escaped: `\'`. This includes `$page_title`,
`$page_description`, schema strings, and any variable assignment.

---

## Vertical Patterns

### Law Firms (CD/DUI)
- Action/fight messaging outperforms comfort messaging
- Deadline urgency on every relevant page (DMV hearing deadlines)
- Professional license implications underserved — nurses, teachers, CDL, pilots
- City pages: one per city, courthouse reference, action messaging in hero
- County pages rank for county queries; city pages rank for city queries — need both
- Hub-and-spoke Tier 3: city + offense intersection pages unlock specific clusters
  county pages can\'t cover
- Guide pages: one topic, 10 FAQs, statute citations, 3–5 internal links

**Vertical filter for 8th Street /legal-marketing/ hub:**
Build pages only for queries containing:
- DUI, DUI attorney, DUI lawyer, DUI law firm
- criminal defense, criminal lawyer, criminal attorney, criminal law firm

Skip everything else — chatbot, healthcare, Wix, and other verticals are noise from
Porch hub pages. Not in scope.

### Service Businesses
- City + service = primary keyword structure ("furnace repair naperville")
- Seasonal clusters: AC repair for summer, heating for winter
- FAQ focus: cost, timeline, what-to-expect

### Content/Information Sites
- Topic clusters: one pillar, multiple supporting pages
- Intent match: informational vs. transactional need different page structures

---

## claude.md Standard (Every Site)

Every site\'s claude.md must contain:
- Page inventory table (slug, primary target query, date added)
- Page template spec — exact PHP structure so CC builds without reading an example
- Known anchor strings for hub page inserts (`<!-- NEW_CARDS_INSERT_BEFORE -->`)
- GSC report filter note — which section of scored report applies to this site
- Vertical filter rule — explicit build/skip decision, not left to inference
- Known bug patterns — recurring errors to check every session

---

## Deployment Format

- Lead with exact URL/file path above each change
- Single-line swaps: state file, line number, show find/replace
- Full replacements: download artifact
- Zip multiple files together for user upload via SiteGround File Manager
- Always run `php -l` + `touch .htaccess` after deployment

---

## Agent Automation Direction

**Live:** GSC fetch + scoring + work order runs every Monday 6:00–6:30 AM Mountain via
cron. Output delivered to jim@ by email before the workday starts.

**Near-ready:** Automated page inventory check — does a page exist for this query cluster?
Remaining gap: reliable slug matching (scored query → does /[slug].php exist on server?).

**Automation targets (not yet built):**
- Automated page inventory check vs server files
- Post-session claude.md update drafted automatically for human review
- PHP lint failure stops session and alerts — not just prints a warning
- First-draft page generation from work order + keyword cluster (after legal-marketing
  pages show click movement in GSC)

**What cannot be automated yet:** Content quality judgment — knowing what questions a
specific vertical\'s clients search, what differentiates one page angle from another.
Requires content brief template per page type.

---

## Meta Title and Description Standards (2026)

Google rewrites 62%+ of meta descriptions. Write them as ideal — compelling enough to keep, clear enough that Google's rewrite still serves the page well.

**Title formula:** Primary keyword (front-loaded) + value or action + | 8th Street (or client brand)
- Target: 50-60 chars. When competition clusters at 50-55 chars, a punchy 35-45 char title can stand out.
- Never: keyword + keyword + keyword. Write for the human scanning results.
- Test: Would you click this if you saw it in search results?

**Description formula:** Problem or situation + what the page answers + soft CTA
- Target: 140-160 chars. Place primary keyword in first 120 chars (mobile cutoff).
- Treat as ad copy — this is the moment between impression and click.
- Avoid: Generic ("Learn more about..."), passive voice, restating the title.

**The CTR signal:** High impressions + low CTR = meta problem, not content problem. Pages at position 1-20 with CTR below 2% are the first target for rewrites. GSC shows this directly.

**Google rewrite signal:** If GSC shows a different title or description than what's on the page, Google rejected ours. Read what Google chose — it usually reveals what they think the page is actually about.

---

*Curb Appeal — GSC Organic Growth Loop*
*Methodology only. Client-specific rules and history in clients/[client-id]/curb-appeal.md*
*Scales to any number of sites without skill changes.*

---
*Always follow the Session Close Routine in the work-smarter skill at the end of every session.*
