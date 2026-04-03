# Foundation — Clients
## Active Client Builds and Intake Standard

---

## Active Clients

| Client | client-id | Domain | Products | Status |
|--------|-----------|--------|----------|--------|
| Andy Sotiropoulos | andy-dui | illinois-dui-defender.com | Foundation + Porch + Google Ads | Live — 20+ guide pages, 30 city pages, 8 profession pages |
| Heart of Illinois / Maureen | heart-il | heartofillinoiscriminaldefense.com | Foundation + Google Ads + Porch | Live |
| Concierge Couples Counseling (Kendra Capalbo) | kendra-cc | conciergecouplescounseling.com | Foundation | Rewrite in progress |
| Inreach Virtual (Emily Spruill) | inreach-virtual | inreachvirtual.com | Foundation | Live — 28 pages |
| Peoria Criminal Defense | peoria-cd | peoriacriminaldefense.com | Foundation (resource site) | Live — 19 pages |
| 8th Street Solutions | 8th-street | 8thstreetsolutions.com | Foundation + Porch + Curb Appeal | Live |
| Sharon Gibney Taylor | sharon | 8thstreetsolutions.com/sharon (demo) | Foundation + Porch | Demo live — awaiting client approval |

## Prospects / Demos

| Name | Demo Status | Notes |
|------|-------------|-------|
| Aquarius Lawyers | Awaiting response | — |
| Melnick Timmerman | Awaiting response | — |

## Reference Builds

| Site | Notes |
|------|-------|
| peoriacriminaldefense.com | Track 3 community resource — March 2026 |

**Client files live at:** `~/www/8thstreetsolutions.com/public_html/clients/[client-id]/`

Each client folder contains:
- `context.md` — master client context (business info, products, status, notes)
- `change-log.md` — dated record of every change made
- `curb-appeal.md` — GSC history and SEO notes (if applicable)
- `google-ads.md` — account details, conversion labels, history (if applicable)

Read `context.md` before any client session. Log every change in `change-log.md`.

---

## Client Intake

When a new client says yes, collect this before building anything:

```
business_name:
owner_name:
phone:
email:
address (city/state minimum):
domain (existing or desired):
services offered (list):
primary geographic area served:
any existing brand colors:
owner photo available: yes/no
any existing content to preserve: yes/no
Porch: yes/no
Google Ads: yes/no
```

**Research CC runs after intake:**
- Google Business Profile — confirm details, note reviews
- LinkedIn — owner credentials, background
- Professional directory listings (Avvo, Justia, Martindale, state bar)
- Competitor sites in the same market — note gaps and opportunities
- Current site (if exists) — note what works, what to improve

**After intake:** Create `clients/[client-id]/context.md` on SiteGround. This is the
single source of truth for everything client-specific.

---

## Client File Standard

Every `clients/[client-id]/context.md` must contain:

```
# [Client Name] — Context

## Business
- Name:
- Owner:
- Domain:
- Phone / Email:
- Address:
- Services:
- Service area:
- Products: Foundation | Porch | Google Ads | Curb Appeal

## Site
- Foundation type: Main Site / Second Home / Resource Site
- Launch date:
- Page count:
- Stack: PHP, no CMS

## Status
- Current phase:
- Open items:

## Notes
[Anything client-specific that affects how we work with them]
```

`change-log.md` format:
```
## [YYYY-MM-DD]
- [What changed and why]
```
