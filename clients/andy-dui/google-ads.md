# Andy Sotiropoulos — Illinois DUI Defender
## Google Ads Account State

Last updated: April 2026

---

## Account Config

Site: illinois-dui-defender.com
Google Ads ID: AW-17655117408
GA4: G-SVNCXQHC9G
Phone label: gU_-CO78vb0bEODszuJB
Email label: 029xCL6ixb0bEODszuJB
Form label: ThyfCJ_O6IMcEODszuJB

Campaigns:
- DuPage County (campaign name: DuPage)
- Multicounty: Will, Kane, Kendall (campaign name: Will, Kane, Kendall)
- Shared budget between both campaigns
- Both on Max Conversions + Broad Match (switched mid-March 2026 from Max Clicks + phrase match)

---

## Conversion Tracking

Three conversion actions — all confirmed firing as of late March 2026:
- Phone Click: trackPhoneClick() via sendBeacon — fires on all tel: links in header, footer, contact page
- Email Click: trackEmailClick() via sendBeacon — fires on mailto: links
- Form Submit: fires on /contact?success=1 page load via gtag() — avoids PHP redirect race condition

All labels centralized in config.php. Source attribution set in header.php early so window.trafficSource is available before any click fires.

One open item as of April 2026: live form submission test to confirm Form Submit fires in both Google Ads and Plausible. Code is correct — test not yet completed.

---

## Performance History

### March 2026
- Spend: $4,869 | Clicks: 225 | Conversions: 22 | $/conv: $221 | Conv rate: 9.8%
- First 9 days zero conversions — normal Max Conversions warm-up pattern
- Conversion volume accelerated last week of month

Geographic breakdown:
| County | Clicks | Conv | $/Conv |
|---|---|---|---|
| DuPage | 94 | 10 | $232 |
| Kane | 55 | 7 | $147 |
| Will | 71 | 5 | $277 |
| Kendall | 5 | 0 | — |

Kane emerging as strong performer. Will underperforming. Kendall negligible volume.
No bid adjustments made — need more data. Revisit May 2026.

---

## Negative Keywords

NKW lists built and applied to both campaigns. April 2026 additions (from March search terms):
Single words: tedone, fioti, himel, fotopoulos, deluca, drwencke, mccubbin
Phrase match: "dui assessment", "treatment centers", "care clinics", "clean slate", "public defenders", "will county attorneys", "dui counseling", "penalty chart", "dui laws", "state's attorney's office", "circuit clerk", "court house", "epay", "ilcourthelp", "prairie legal", "low income"

Known issue: some excluded terms still firing (campaign-level vs ad group-level). To verify and fix.

---

## Open Items

- Complete live form submission test
- Fix excluded terms — verify all applied at campaign level not ad group level
- Ad copy rewrite: deferred until full month of April data available (early May)
- Kane County: watch through April, bid adjustment decision in May
- Will County: if still underperforming at May review, consider budget reallocation toward DuPage
