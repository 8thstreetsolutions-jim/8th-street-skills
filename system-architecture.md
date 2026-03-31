# 8th Street / PathAcross — System Architecture
version: 1.0
date: 2026-04-01

## The Big Picture

Jim is the conductor, not the operator. The system runs. Jim sets direction, reviews exceptions, approves what goes live, and tunes the WisdomBase. Everything else is automated or agent-executed.

## The Stack

### Jim — conductor
Sets direction. Approves what goes live. Tunes WisdomBase. Reviews exception reports. Makes judgment calls that require a human. Not in the loop for routine execution.

### Claude.ai — strategic synthesis
Research. Architecture decisions. Skill design. Synthesizes research findings before they become skill updates. Strategic conversations happen here. CC executes, Claude.ai thinks.

### Skills on SiteGround — the harness
Location: ~/skills/user/ on ssh.8thstreetsolutions.com
GitHub: https://github.com/8thstreetsolutions-jim/8th-street-skills
The harness is as important as the model. A 6x performance gap is achievable just by improving the harness. Skills self-improve via overnight loop.

### Claude Code — execution
Builds. Deploys. Runs agents. SSH to SiteGround. Commits to GitHub. Reads skills at session start. SiteGround is the source of truth — CC never maintains its own copies.

### Session start rule (every CC session)
1. Read ~/skills/user/work-smarter/SKILL.md
2. Read the skill(s) relevant to today's task
3. Read ~/skills/claude.md (or ~/www/8thstreetsolutions.com/public_html/claude.md)
Then execute.

### Live overnight agents
- Porch daily agent — LIVE. 6am Mountain. Reviews logs, surfaces patterns, emails report.
- GSC Monday agent — LIVE. Monday 6am. Pulls data, scores opportunities, emails work order.
- Research agent — BUILDING. Monitors sources, routes findings to skill updates.
- Skills self-improvement loop — DESIGNING. Reads performance signals, proposes updates, commits to GitHub for morning review.
- Curb Appeal overnight — DESIGNING. GSC pull, score, draft pages, commit to review branch.

### Performance signals — the feedback loop
GSC rankings, Porch conversation outcomes, research findings feed back into skill improvements overnight. Each cycle the harness gets better without Jim managing it.

### GitHub — the safety net
Every skill change committed. Full history. Portable. The overnight loop cannot run safely without this.

## PathAcross — parallel system
Same harness architecture. Blue runs through WisdomBase. Spaces feed PathAcross. Jim tends WisdomBase only — never delegated to agents. Separate server. Separate session.

## The Self-Improvement Loop
Signals → CC proposes skill update → commits to GitHub → Jim reviews in morning → approved changes go live → next cycle runs on improved harness.

## What Jim Does
- Sets strategic direction
- Reviews morning exception reports
- Approves content before it goes live
- Tunes WisdomBase (PathAcross only)
- Makes client relationship decisions
- Decides when to expand to new verticals or agents

## What Never Gets Automated
- WisdomBase additions
- Content going live on client sites without review
- Client relationship decisions
- Strategic direction changes
- Google Ads changes above threshold (>20% bid, >15% budget)
- Any email going to a real person without review

## Version Control Rule
After every skill or claude.md edit:
cd ~/skills && git add -A && git commit -m "[what changed and why]" && git push
