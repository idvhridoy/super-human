# Master Roadmap — Sprint System, Build Phases, Learner Roadmaps

The time model of Super Human OS. Three nested rhythms govern everything: the **7-day sprint** (execution cadence), the **30/60/90-day + yearly horizon** (domain progression), and the **project build phases** (system construction, prd.md §11).

---

## 1. The 7-Day Sprint

The weekly rhythm: plan → execute → audit → review → adapt. Sprint `N` is recorded in `STATE.md` (`## Sprint`) and reviewed in `routine/weekly/YYYY-Www.md`. All timestamps ISO-8601, Asia/Dhaka (+06).

| Day | Phase | Owner(s) | Output |
|---|---|---|---|
| **1 (Sat)** | Planning & baseline | `@daily-routine` + `@roadmap-planner` + `@schedule-manager` | `routine/daily/YYYY-MM-DD.md` + sprint goals block in `STATE.md`; baseline measurements scheduled/pend for new domains |
| **2–5** | Execution | learner + `@domain-mentor` / coaches + `@nutritionist` + `@sleep-coach` | `sessions.md` entries, `pillars/*/log.md` entries, `LOG.md` narration via `@activity-log` |
| **6** | Consolidation | `@checklist-review` + `@safety-guardian` | Weekly carry-over audit; safety check before review |
| **7 (Fri)** | Review & adapt | `@weekly-review` → `@progress-report` → `@strategy-advisor` | `routine/weekly/YYYY-Www.md`, `reports/weekly/YYYY-Www.md`, adjusted roadmap files, next-sprint goals |

Every evening inside Days 1–6: `@checklist-review` audits the day's plan (`routine/daily/YYYY-MM-DD.md`) against evidence, ticks `- [x]`, records misses with reasons, and carries them forward. Every night: `@sleep-guardian` watch.

### Day 1 — Planning / Baseline
- `@roadmap-planner` refreshes `domains/<slug>/roadmap.md` and the overall plan from last week's review.
- `@daily-routine` reads `learner/schedule.md`, `pillars/training/periodization.md`, active domain priorities in `domains/INDEX.md`, and writes Day 1's time-blocked plan.
- `@schedule-manager` reconciles conflicts (fixed commitments vs. sessions vs. rest blocks).
- New domains get a baseline assessment queued: `@assessment-engine` sets the `L` level that anchors all progression.

### Days 2–6 — Execution
- The learner executes the plan; agents log, coach, and audit. No new planning unless `@orchestrator` is invoked or a flag fires.
- `@safety-guardian` may veto any training block at any point (sleep debt, injury flag, overtraining signal).

### Day 7 (Friday) — Review
- `@weekly-review`: sprint KPI vs target (prd.md §10 — ≥85% plan completion, ≥90% sleep adherence, ≥80% nutrition adherence, ≥5 training sessions/week), streak summary, wins/misses with reasons.
- `@progress-report`: writes `reports/weekly/YYYY-Www.md` — per-domain table + whole-person summary + exactly 3 recommendations.
- `@strategy-advisor`: adjusts next sprint's priorities; writes carry-over and next goals into `STATE.md` and affected `roadmap.md` files.

## 2. Per-Sprint Goals Template

Sprint goals live in `STATE.md` under `## Sprint` and are expanded in `routine/weekly/YYYY-Www.md`. Convention:

```markdown
## Sprint Goals — Sprint <N> (<YYYY-MM-DD> → <YYYY-MM-DD>)
- [ ] Pillar goal 1 — e.g., "Sleep ≥ 90% nights within window"
- [ ] Pillar goal 2 — e.g., "5 training sessions, 2 running"
- [ ] Domain goal — e.g., "Running: reach L2 (assessed); Physics: finish mechanics unit"
- [ ] Stretch — e.g., "First 30-min drawing session"
```

Rules: max 4 goals per sprint; every goal maps to at least one file (`pillars/<name>/`, `domains/<slug>/`); unfinished goals are carried over with an explicit reason, never silently dropped.

## 3. Project Build Phases (prd.md §11 — mandatory sequence)

> Builders: **no `SKILL.md` until all Phase-0 documents exist.** Verify wave N before starting wave N+1.

| Phase | Name | Contents | Gate to exit |
|---|---|---|---|
| **0** | Foundation docs (blocking) | `prd.md`, `AGENTS.md`, `docs/` (ARCHITECTURE, SECURITY, MASTER-ROADMAP, ORCHESTRATION, HANDOFF, KPI, AUTOMATION, REFERENCES), `domains/INDEX.md` + `TEMPLATE/`, `routine/checklists/`, `learner/*.md`, `STATE.md`, `LOG.md`, `.gitignore`, `README.md` | All 15 items exist and cross-link |
| **1** | Core operations (wave A) | `orchestrator`, `learner-profile`, `daily-routine`, `checklist-review`, `activity-log`, `schedule-manager` + STATE wiring | Morning plan + evening audit run end-to-end |
| **2** | Pillars (wave B) | `nutritionist`, `sleep-coach`, `sleep-guardian`, `training-coordinator`, `safety-guardian` | All three pillars log + guardian veto works |
| **3** | Domain layer (wave C) | `domain-mentor` engine, `run-coach`, `swim-coach`, `strength-coach`, `kungfu-coach`; seed 20+ domains from `TEMPLATE/` | One full domain lifecycle works |
| **4** | Intelligence & polish (wave D) | `assessment-engine`, `achievement-engine`, `progress-report`, `interest-scout`, `roadmap-planner`, `strategy-advisor`, `knowledge-librarian`, `weekly-review` + README | Full lifecycle per prd.md §12 acceptance criteria |

Execution rule per prd.md §11: **maximum sub-agent parallelism within a wave** — one agent per skill, disjoint file ownership; merge via `LOG.md`.

## 4. Learner Roadmap Conventions (30/60/90-day + Yearly)

Every `domains/<slug>/roadmap.md` follows the `TEMPLATE/` structure — maintained by `@roadmap-planner`, adjusted from evidence, not wishes.

| Section | Horizon | Content convention |
|---|---|---|
| `## Next 30 days` | Tactical | Concrete sessions/units; each item `- [ ]` with a measurable finish line and target date |
| `## 60 days` | Building | Competency milestones (e.g., "swim 50m unassisted", "physics L2 assessment passed") |
| `## 90 days` | Strategic | Quarter-level outcome per domain; feeds `@strategy-advisor` prioritization |
| `## This year` | Vision | One-line north star per domain (e.g., "run a 10k", "hold a conversation in target language") |

Rules:
- Roadmap dates are **revised by evidence** — `sessions.md` velocity, `metrics.md` trends, and `learner/assessments.md` results — never padded to look good.
- Level targets are `L0`–`L10`, set by `@assessment-engine` only; roadmaps propose, assessments confirm.
- New domains proposed by `@interest-scout` enter as `queued` in `domains/INDEX.md`; `[HUMAN]` approval required to activate (see `docs/AUTOMATION.md`).
- Whole-learner long-horizon priorities live in `learner/goals.md`; domain roadmaps must serve them, not contradict them.

## 5. Cadence Summary

| Frequency | Trigger | Owner | Artifact |
|---|---|---|---|
| Continuous | learner narrates activity | `@activity-log` | `LOG.md`, `STATE.md` counters |
| Daily (morning) | `/daily-routine` | `@daily-routine` | `routine/daily/YYYY-MM-DD.md` |
| Daily (evening) | `/checklist-review` | `@checklist-review` | day file audit, streaks in `STATE.md` |
| Nightly | bedtime/wake | `@sleep-guardian` | `pillars/sleep/night-log.md` |
| Weekly (Fri) | `/weekly-review` + `/progress-report` | `@weekly-review` | `routine/weekly/`, `reports/weekly/` |
| Monthly | `/progress-report` | `@progress-report` | `reports/monthly/YYYY-MM.md` |
| Per sprint boundary | Day 1/Day 7 | `@roadmap-planner`, `@strategy-advisor` | `roadmap.md`, `STATE.md` goals |
