<!--
version: 0.1
date: 2026-04-01
source: Stub — overnight loop designed, build in progress
-->

---
name: overnight-loop
description: "The nightly automation loop that reads all data sources, runs evals, generates proposal files, and emails the morning report. Status: designed, not yet built. Read this skill when building or modifying the overnight loop scripts. Do not confuse with individual agents already live (Porch daily agent, GSC Monday agent) — those are components that will feed into this orchestrator."
---

# Overnight Loop
## Status: Designed — Build In Progress

---

## What This Will Do

Nightly at 2am Mountain, one orchestrator script:

1. Reads Porch logs (all clients + PathAcross Blue conversations)
2. Reads latest GSC report from ~/gsc-agent/reports/
3. Reads Plausible via plausible-api.php
4. Checks research sources for new findings
5. Runs evals skill criteria against all data
6. Generates proposal files in ~/overnight-reports/
7. Emails consolidated morning report to jim@

## Build Order

- v1: Porch + GSC + email (build next)
- v2: Add research agent
- v3: Add skills self-improvement loop
- v4: Add GSC data-event trigger (replace fixed Monday cron)

## Components Already Live

- Porch daily agent — gateway/porch-daily-agent.php, cron 6am Mountain
- GSC Monday agent — ~/gsc-agent/, cron Monday 6am Mountain

## Proposal File Format

~/overnight-reports/YYYY-MM-DD/
  curb-appeal-proposals.md
  ads-proposals.md
  porch-proposals.md
  research-findings.md
  skill-proposals.md
  morning-report.md (consolidated — this gets emailed)

## Human Touchpoints

Jim reviews morning-report.md each morning.
Approves, dismisses, or flags each proposal.
CC executes approved items same day.

---

*Overnight Loop — 8th Street Solutions*
*Build v1 before applying to client accounts.*
