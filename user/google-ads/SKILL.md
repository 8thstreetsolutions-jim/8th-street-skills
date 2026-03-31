<!--
version: 2.1
date: 2026-04-01
source: Added Quality Signal Audit section (2026), AI Mode consideration
-->

---
name: google-ads
description: "Manage Google Ads campaigns for professional service clients — CD/DUI law
firms and any other vertical. Use when (1) reviewing monthly performance, (2) analyzing
search terms reports, (3) building or updating negative keyword lists, (4) adjusting
geographic bid adjustments, (5) writing or updating ad copy, (6) building or optimizing
landing pages, (7) setting up or verifying conversion tracking. Always use this skill when
Jim mentions search terms report, negative keywords, bid adjustments, Google Ads,
conversions, campaign performance, or any client\'s ad account. Client-specific data lives
in clients/[client-id]/google-ads.md on SiteGround."
---

# Google Ads Management
## Professional Service Clients — CD/DUI and Beyond

---

## Active Clients

| Client | Site | Campaigns | State | Notes |
|--------|------|-----------|-------|-------|
| Andy Sotiropoulos | illinois-dui-defender.com | DUI + CD | Illinois | DMV = 90 days. Active. |
| Maureen (Heart of Illinois) | heartofillinoiscriminaldefense.com | DUI + CD | Illinois | DMV = 90 days. Active. |

Both on Foundation + Porch + Google Ads. Client-specific history and account IDs in
`clients/[client-id]/google-ads.md` on SiteGround.

---

## Philosophy

**The goal is cost per qualified lead, not cost per click.**

Prospects are scared, searching fast, often on mobile. Every dollar spent on wrong-intent
searches — informational, competitor names, wrong geography — is a dollar not spent on
someone ready to hire.

**The monthly rhythm:** Pull reports → analyze by geography/keyword → adjust bids → clean
negatives → verify tracking → document changes in client file.

---

## Human-in-the-Loop Thresholds

These require Jim review before executing. Not suggestions — hard stops.

- **Bid changes > 20%** — flag and confirm before applying
- **Budget changes > 15%** — flag and confirm before applying
- **Pausing a keyword with active conversions** — always confirm
- **Any change to conversion tracking code** — always confirm and verify after

Changes below these thresholds can be documented in the monthly report and applied
on Jim\'s direction. Never apply changes autonomously above threshold.

---

## What This Skill Produces

- Formatted negative keyword lists ready to paste into Google Ads
- Geographic bid adjustment recommendations with reasoning
- Monthly performance analysis with trend identification
- Ad copy variants following vertical-specific principles
- Conversion tracking implementation code
- Monthly review checklist output per client

**Client data lives in:** `clients/[client-id]/google-ads.md` on SiteGround. Read that
file before any client session.

---

## Monthly Review Process

Run this for every account, every month:

1. **Performance table** — add month, note trends vs prior periods
2. **Search terms report** — pull CSV, analyze, produce formatted negative list
3. **Add negatives** — to shared list in both campaigns
4. **Geographic report** — adjust bids on underperformers (rule below)
5. **Keyword report** — pause keywords with 30+ clicks and 0 conversions
6. **Conversion tracking** — verify all conversion types firing; cross-reference Plausible
7. **Call asset** — check performance split vs site clicks; flag call quality issues
8. **CPC by campaign** — flag outliers
9. **Budget pacing** — check for early-day exhaustion (run out before 5 PM = wasted
   afternoon intent)
10. **Document all changes** in client file with date and reason

---

## Quality Signal Audit (Monthly)

The Big Three that determine Quality Score: ad relevance, expected CTR, landing page experience.

**Ad relevance check:**
- Pull keyword report, filter for "Below average" ad relevance
- Fix: Match ad text language more directly to the keyword. Split ad groups that have too many unrelated keywords.
- Target: All primary keywords at "Average" or above

**Expected CTR check:**
- "Below average" expected CTR = ad copy isn't compelling vs competitors
- Fix: Lead with fight/action language. Include DMV deadline. Make the offer specific.
- Target: Primary keywords at "Average" or above

**Landing page experience check:**
- "Below average" = page doesn't deliver the promise of the ad, or loads slowly
- Fix: Confirm ad message matches page hero. Confirm phone number visible above fold. Check mobile load speed.
- Target: All landing pages at "Average" or above

**Quality Score as trend, not snapshot:**
- Don't optimize for the number. Optimize for the trend.
- If QS trending down on a converting keyword — something changed. Investigate.
- If QS is low but $/conv is good — don't restructure just to improve the score.

**Conversion data quality (monthly check):**
- Verify all three conversion types fired at least once in the past 7 days
- Cross-reference with Plausible — if Plausible shows phone clicks but Google Ads doesn't, sendBeacon is broken
- The conversion data is what Google's AI optimizes toward. Bad data = bad optimization.

**AI Mode consideration (2026):**
- Google's AI Mode (March 2025) shows ads inside AI-generated summaries.
- Conversational ad copy performs better in this context — "Can a DUI be fought?" outperforms "Experienced DUI Attorney."
- Test: Does the ad answer a question the prospect is actually asking?

---

## Negative Keywords

### Format Rules
- Single word: no quotes, no brackets — `smith`
- Multiple words: quotes for phrase match — `"public defender"`
- Competitor last names: single word only — captures all name variations
- One per line for copy/paste into Google Ads

### Universal Negative Categories (All Clients)

**Wrong intent — informational:**
`meaning`, `youtube`, `reddit`, `"what is"`, `"what does"`, `"penalty chart"`,
`"laws 2026"`, `"laws 2025"`, `statistics`, `"how long"`

**Price shoppers / not hiring:**
`cheap`, `cheapest`, `affordable`, `cost`, `price`, `free`, `"how much"`, `"pro bono"`,
`"legal aid"`, `"court appointed"`, `"public defender"`

**Wrong services:**
`classes`, `evaluation`, `reinstatement`, `jobs`, `salary`, `become`, `school`,
`definition`, `quiz`

**Spanish (unless client serves Spanish speakers):**
`abogado`, `abogados`

### CD/DUI Specific Negatives
`expungement`, `immigration`, `"what is a dui"`, `"is dui a felony"`,
`"secretary of state"`, `"appellate defender"`, `"circuit court"`,
`"felony dui"` (if client doesn\'t handle felonies), `"dui laws"`, `"dui statistics"`

Add courthouse and county clerk terms specific to client geography — these appear
regularly in search terms reports and waste budget consistently.

### Building the Competitor Negative List
Pull search terms monthly. Any competitor firm name or attorney name that appears → add
as single-word negative. Build this list over time — it compounds. Document in client file.

---

## Geographic Bid Adjustments

**The rule:** Any location with 20+ clicks and 0 conversions gets a bid cut. Any location
below target $/conv with 10%+ conversion rate gets held or increased.

**Adjustment increments:** Start at ±15–25%. Revisit after 30 days minimum before further
changes. Changes > 20% require Jim confirmation.

**Location setting:** Always "Presence" not "Presence or Interest" — prevents ads showing
outside target geography regardless of search query.

**Important for urban markets:** Do not negative major city names when clients serve
surrounding suburbs — residents routinely search city-name variations even when located
in suburbs.

**Illinois-specific:** Chicago metro searchers frequently hire suburban attorneys. Kane,
DuPage, Will, Lake County clients get Chicago-variant traffic — do not negative Chicago
unless client explicitly refuses Chicago cases.

---

## Conversion Tracking

### Standard Implementation

```javascript
// In header.php — define functions globally
function trackPhoneClick() {
    navigator.sendBeacon('https://www.googleadservices.com/pagead/conversion/[ACCOUNT_ID]/?label=[LABEL]&guid=ON&script=0');
    gtag('event', 'conversion', {'send_to': 'AW-[ACCOUNT_ID]/[LABEL]', 'transport_type': 'beacon'});
    return true;
}
function trackEmailClick() {
    navigator.sendBeacon('https://www.googleadservices.com/pagead/conversion/[ACCOUNT_ID]/?label=[LABEL]&guid=ON&script=0');
    gtag('event', 'conversion', {'send_to': 'AW-[ACCOUNT_ID]/[LABEL]', 'transport_type': 'beacon'});
    return true;
}

// In footer.php — form submit listener
document.querySelector('form')?.addEventListener('submit', function() {
    gtag('event', 'conversion', {'send_to': 'AW-[ACCOUNT_ID]/[LABEL]', 'transport_type': 'beacon'});
});
```

### sendBeacon is Non-Negotiable for Mobile
Standard `gtag()` fires async and fails when browser hands off to phone dialer. Always use
`sendBeacon` on phone click events. No exceptions.

### Form Submit
Fire on thank-you page load, not submit event — more reliable. If no thank-you page,
fire on submit event with sendBeacon.

### Verification Protocol
1. Pull conversion labels directly from Google Ads Conversions UI — never trust notes
2. Confirm each label character by character — typos are common
3. Cross-reference conversion counts with Plausible analytics weekly
4. Phone conversion goal must be set as Primary manually after any account restructure —
   Google defaults phone clicks to Secondary

### Plausible Cross-Reference
Plausible tracks all site visitors and events independently of Google. Use it to:
- Verify conversion counts aren\'t inflated by bot traffic
- Catch tracking gaps (Plausible showing form submits that Google didn\'t record)
- Identify real traffic patterns that Google Ads data alone misses

---

## Porch / Ads Relationship

Porch and Google Ads serve different intent windows on the same prospect pool.

**Key insight:** Porch captures after-hours and weekend visitors who find the site
organically or via direct traffic. Google Ads runs during core hours. Together they
provide near-24/7 coverage without 24/7 staff.

**When reviewing Porch daily agent reports alongside Ads data:**
- After-hours Porch conversations = leads Google Ads cannot capture (runs on schedule)
- High Porch engagement on a specific page + low Ads conversion on that keyword →
  page may be strong organically but not landing page optimal
- Porch KB gaps flagged by the daily agent may indicate FAQ content missing from
  landing pages too — fix both simultaneously

**Do not add Porch widget to dedicated Google Ads landing pages.** Porch on landing pages
creates a competing conversion path and dilutes the primary CTA.

---

## Ad Copy Principles

### CD/DUI Vertical
- Lead with fight and action, not credentials
- DMV deadline urgency in headlines and descriptions
  - Illinois: 90 days from arrest to request hearing
  - Verify deadlines per state before writing — they change
- "Free Case Review" or "Free Consultation" always present
- Describe the outcome (dismissed, reduced, license saved) not the service
- Evening/weekend availability removes a major objection
- Specific courthouse and county references build local trust

**Proven:** Fight-forward messaging consistently outperforms credential messaging on CTR
and $/conv. "Fight Your DUI" beats "Experienced DUI Lawyer." Test and document results in
client file.

### General Professional Services
- Lead with the outcome the prospect wants
- Specificity beats generic — name the problem, the geography, the timeline
- Trust signals next to the claim that needs them (not buried below fold)
- One clear CTA per ad

---

## Landing Pages

### Required Elements (All Verticals)
- Hero answers three questions immediately: What is this? Is this for me? Can I trust it?
- Phone number click-to-call always visible above fold
- Mobile-first — professional service search is majority mobile
- Form collects: name, phone, email, message minimum
- Conversion tracking on phone click + email click + form submit
- **No Porch widget** — keep conversion path clean
- Explicit canonical set — prevents UTM parameter variant indexing issues

### CD/DUI Specific
- Reference specific courthouse and county in hero — local trust signal
- DMV deadline prominent above fold
- Separate landing pages per campaign/geography when budgets justify it

---

## Bidding Strategy

**Max Conversions** — use when account has sufficient conversion history (10+
conversions/month). Let Google optimize.

**tCPA** — use cautiously. Has underperformed in low-volume accounts. Do not enable until
conversion volume is consistent over 60+ days.

**Manual CPC** — use for new campaigns with no history. Switch to Max Conversions once
data exists.

**Broad match + Max Conversions** has outperformed phrase match in tested accounts — lower
CPC, higher volume. Requires tighter negative keyword discipline. Document performance in
client file when switching match types.

---

## Call Asset

- Enable on all campaigns — call asset drives significant lead volume in professional services
- Monitor performance split: call asset leads vs site click leads
- Call asset leads (one tap) are lower intent than deliberate site visitors — flag if client
  reports call quality drop
- Always verify call asset is active after any campaign restructure

---

## Seasonality (CD/DUI)

- **January weeks 1–2:** Post-holiday arrest spike. Plan higher budget going into new year.
- **Summer (May–August):** DUI volume increases with outdoor events, July 4th, Labor Day
- **End of month:** Court appearance searches spike — higher urgency, higher intent
- **January–February:** Court dates from holiday arrests = defense search spike

---

## Budget Pacing Check

Run weekly, not just monthly. A campaign exhausting budget by noon wastes afternoon
high-intent searches.

Signs of exhaustion:
- Impression share drops sharply after midday
- Avg position degrades progressively through the day
- CPC spikes in the morning (bidding full budget against competition early)

Fix: increase daily budget OR set ad scheduling to front-load on peak hours only.

---

## Agent Automation Direction

Google Ads has a full API. This is the documented manual workflow — the foundation for
automation once the manual process is proven stable.

**Planned automations (not yet built):**
- Monthly search terms report pulled automatically — negative candidates flagged without
  manual CSV export
- Geographic performance auto-analyzed — bid adjustment recommendations in monthly report
- Conversion tracking verification — automated check all conversion types are firing
- Performance anomaly alerts — flag $/conv spikes or conversion rate drops week over week
- Budget exhaustion detection — alert if campaign running out before 3 PM local time

**Build order:** Document manual workflow fully first (this skill is that document) →
connect Google Ads API → automate data pull → automate analysis → automate report →
human reviews and approves actions.

Current state: Manual. Using this skill as the operating manual for all account work.

---

*Google Ads — 8th Street Solutions*
*Methodology only. Client data in clients/[client-id]/google-ads.md*
*Scales to any number of accounts without skill changes.*
