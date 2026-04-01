# 8th Street Solutions — Client Context

**client_id:** 8th-street
**domain:** 8thstreetsolutions.com
**site_type:** main
**vertical:** professional
**owner:** Jim Ingham
**phone:** (904) 330-5800
**email:** john@8thstreetsolutions.com
**address:** Denver, Colorado
**colors:** [confirm from styles.css]
**fonts:** [confirm]
**GSC:** [confirm status]
**Porch:** Active (client_id: 8thstreet)
**SSH:** ~/www/8thstreetsolutions.com/public_html/

**Notes:** Primary 8th Street marketing site. All six products presented as one integrated system. Living site model — not a brochure. Agents to be added as product offering.

**Products active:** Foundation, Porch, Curb Appeal

---

## Session Log

### 2026-03-28 — Foundation Standards Pass
- **Schema:** Upgraded to @graph standard. BreadcrumbList added to all interior pages.
- **config.php:** Expanded to full Foundation standard variables ($site_name, $site_url, $phone, $email, $metrics[], $calculator[], $schema_data, etc.)
- **Log blocking:** .htaccess updated to block log files from public access.
- **Favicon:** favicon.svg created — gold 8 on navy. Wired into header include.
- **Includes migration:** header.php and footer.php moved to includes/. 52 files updated to new include paths.
- **robots.txt:** Updated — Disallow: /includes/ added.
- **sameAs:** Left empty pending GBP and LinkedIn account creation.
- **Deferred:** form-handler.php relative path issue on subdirectory pages — not yet resolved.
- **Safety net:** Root header/footer stubs left in place. Remove after spot check confirms includes/ working across all pages.
