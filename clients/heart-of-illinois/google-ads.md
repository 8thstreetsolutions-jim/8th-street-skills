# Maureen Williams — Heart of Illinois Criminal Defense
## Google Ads Account State

Last updated: April 2026

---

## Account Config

Site: heartofillinoiscriminaldefense.com
Google Ads ID: AW-870607742
Phone label: Uj1pCKuW8ekZEP7WkZ8D (corrected late March 2026 — was wrong value)
Form label: TUeXCOuT3I4YEP7WkZ8D

Campaigns:
- DUI Defense (campaign name: DUI 9-18-24)
- General Criminal Defense + Post Conviction (campaign name: GD, PC 9-18-24)
- Both on Max Conversions

IMPORTANT: Maureen does NOT offer free consultations. Never use this language in ad copy.

---

## Conversion Tracking

Phone tracking relies entirely on manual trackPhoneClick() via sendBeacon. No Google forwarding numbers — "Calls from Website" will never fire.

Fix applied April 2026: trackPhoneClick() and trackEmailClick() functions moved from footer.php to header.php (<head>). Prior to fix, mobile header/top bar phone clicks were not firing — account was undercounting phone conversions. Data prior to fix is understated.

Phone label was incorrect (V6koCJmAgfkDEP7WkZ8D) — corrected to Uj1pCKuW8ekZEP7WkZ8D in config.php in late March 2026.

Google default conversion actions (Page view, Engagement, Directions, etc.) appear as Primary but cannot be demoted through UI in this account. Campaign-level Goals override attempted — limited by Google UI restrictions.

---

## Performance History

### March 2026
- Spend: $7,056 | Clicks: 396 | Conversions: 57 | $/conv: $123.79 | Conv rate: 14.4%
- Note: phone conversion data prior to late March tracking fix is understated

GD Campaign geographic highlights:
| County | Conv | $/Conv |
|---|---|---|
| Peoria | 15 | $72.78 |
| McLean | 8 | $75.16 |
| Macon | 7 | $65.71 |
| LaSalle | 3 | $36.55 |
| Fulton | 3 | $71.32 |

DUI Campaign underperforming vs GD. DuPage at $247/conv, McLean at $285/conv.
No bid adjustments made — one month not sufficient sample size.

---

## Negative Keywords

NKW lists built and applied. April 2026 additions (from March search terms):

Single words: schwulst, roseberry, pioletti, heyl, royster, leynaud, janssen, dunlap, essig, finegan, miskell, watson, gerber, patton, judici, expungement, expunge, reinstatement, baiid, intoxalock, abogado, abogados

Phrase match: "state's attorney", "states attorney", "probation office", "circuit court", "county warrants", "clean slate", "foid card", "gun rights", "how much", "how much does", "legal aid", "court appointed", "public defender"

Known issue: some excluded terms still firing (campaign-level vs ad group-level). To verify and fix.

---

## Open Items

- Fix excluded terms — verify all applied at campaign level not ad group level
- Ad copy rewrite: deferred until full month of April data (early May)
- Unanswered question to Maureen/Greg: DUI vs CD case value ratio — relevant to budget allocation decision
- Monitor whether phone conversion volume increases now that tracking fix is in place
