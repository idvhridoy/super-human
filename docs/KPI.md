# KPI — Metric Definitions, Formulas & Report Formats

> Contract for `@checklist-review`, `@achievement-engine`, `@assessment-engine`,
> `@progress-report`, `@weekly-review`, and every pillar/domain `metrics.md`.
> Targets inherit from `prd.md` §10. Numbers are computed from file state only —
> never from memory (guardrail 6: evidence required).

---

## 1. Core Metrics

| Metric | Definition | Formula | Source files | Owner | Target |
|---|---|---|---|---|---|
| Daily plan completion % | Share of planned tasks verified done | `done / planned × 100` over the day's checklist | `routine/daily/YYYY-MM-DD.md` (plan) + evidence in `LOG.md`, `sessions.md`, `night-log.md`, `nutrition/log.md` | `@checklist-review` | ≥ 85%/day |
| Sleep adherence % | Nights where bedtime AND wake fell inside `pillars/sleep/routine.md` target window (±30 min grace) | `nights_in_window / nights_logged × 100` | `pillars/sleep/night-log.md` → `pillars/sleep/metrics.md` | `@sleep-guardian` | ≥ 90% of nights |
| Nutrition adherence % | Planned meals logged as followed | `meals_followed / meals_planned × 100`; hydration logged separately | `pillars/nutrition/log.md` + `meal-plan.md` → `pillars/nutrition/metrics.md` | `@nutritionist` (audit: `@checklist-review`) | ≥ 80% of planned meals |
| Training consistency | Physical sessions completed per week across physical domains | `count(sessions.md entries)` in `pillars/training/` + physical `domains/<slug>/sessions.md` | `pillars/training/sessions.md` → `pillars/training/metrics.md` | `@training-coordinator` | ≥ 5 sessions/week, ≤ 3 consecutive intense days |
| Domain learning hours | Logged session minutes per domain | `Σ duration` from `domains/<slug>/sessions.md` | `domains/<slug>/metrics.md` + `domains/INDEX.md` hours column + `STATE.md` learning-minutes counter | `@domain-mentor` | weekly trend ↑ |
| Domain streak | Consecutive days with ≥ 1 logged session in that domain | `consecutive_days(sessions.md)` | `domains/<slug>/progress.md` streak field + `domains/INDEX.md` | `@checklist-review` | no zero-days without logged reason |
| Assessment score | Latest periodic test result per domain | `% correct` or rubric score → level awarded | `learner/assessments.md` Results Log | `@assessment-engine` | pass ≥ 70% (default; domain may override in `curriculum.md`) |
| Level-up rate | Promotions per active domain per rolling month | `count(progress.md Level History rows in 30d)` | `domains/<slug>/progress.md` + `achievements/` | `@assessment-engine` | ≥ 1/active domain/month early levels; expect slower ≥ L5 |
| Achievement count | Badges/milestones issued | `count(achievements/YYYY-MM-DD-*.md)` | `achievements/` + `achievements/README.md` index | `@achievement-engine` | tracked, no quota |
| Safety incidents | Unresolved safety flags | `count(open flags in STATE.md Safety Flags)` | `STATE.md` | `@safety-guardian` | 0 unresolved |

## 2. Reporting Derivations

- **Week** = sprint window `routine/weekly/YYYY-Www.md` (Sat–Fri, Asia/Dhaka +06).
- **This week / Last week / All-time** columns in every `metrics.md` are recomputed weekly by `@progress-report`, not accumulated manually.
- **Whole-person level** (`STATE.md`): `round(median of active-domain levels)` — pillar adherence is reported separately, never folded into skill levels.
- **Active domain** = `Status: active` in `domains/INDEX.md`. Queued/paused domains are excluded from rate targets.

## 3. Streak Rules

Three tracked streaks in `STATE.md` + per-domain streaks in `progress.md` / `domains/INDEX.md`.

| Streak | Day counts when | Streak preserved (no increment) when | Streak breaks when |
|---|---|---|---|
| Daily plan completion | plan existed and completion ≥ 85% | day declared rest/holiday in the plan itself; `@safety-guardian` mandated pause | zero-day or < 85% with no logged reason |
| Sleep schedule | bedtime in window | travel/illness event logged in `night-log.md` with reason | bedtime outside window, no logged reason |
| Training | scheduled session done, or unscheduled active-recovery/mobility day | no session scheduled that day; rest day mandated by `@safety-guardian`; human-approved break | scheduled session skipped without reason |
| Per-domain | ≥ 1 session logged in `domains/<slug>/sessions.md` that day | domain paused (human decision); protected rest | a day with no session and no logged reason |

Rules:

1. **"No zero-days without a logged reason"** is the honesty target — a missed day *with* a reason preserves standing; a silent miss breaks the streak.
2. **Protected rest overrides metrics** (guardrail 7). A `@safety-guardian` rest flag, burnout signal, or illness note preserves every streak and is recorded in `STATE.md` Safety Flags + `LOG.md`.
3. Streaks are computed by `@checklist-review` at evening audit; `@achievement-engine` issues streak badges (7/30/100/365 days) into `achievements/`.
4. Streaks never reset levels or hours — they are a continuity counter only.

## 4. Level System — L0 → L10

Recorded in `domains/<slug>/progress.md` (`Current level` + `Level History`) and mirrored in `domains/INDEX.md` and `STATE.md`.

| Level | Name | Meaning | Default promotion criteria (ALL required) |
|---|---|---|---|
| L0 | Unstarted | Registered only | baseline not yet run |
| L1 | Novice | First contact | baseline assessment logged; ≥ 5 sessions |
| L2 | Beginner | Fundamentals forming | L1 fundamentals checklist in `curriculum.md` passed; ≥ 10 h |
| L3 | Developing | Guided practice works | assessment ≥ 60%; ≥ 25 h; ≥ 2 sessions/wk for 2 consecutive wks |
| L4 | Competent | Performs unaided | assessment ≥ 70%; ≥ 40 h |
| L5 | Proficient | Reliable under variation | assessment ≥ 75%; ≥ 80 h; teach-back or novel-context transfer shown |
| L6 | Advanced | Handles real complexity | assessment ≥ 80%; ≥ 150 h |
| L7 | Expert | Can coach a novice | assessment ≥ 85%; ≥ 250 h; supervised teach-back logged |
| L8 | Master | Externally validated | assessment ≥ 90%; ≥ 400 h; external evidence where applicable (cert, competition, publication, real-world deliverable) |
| L9 | Authority | Sustained excellence | ≥ 6 months at L8 with continued logged practice; contributes curriculum improvements to `domains/<slug>/` |
| L10 | Mastery | Self-directing; sets own curriculum | human-confirmed; rare by design |

Promotion rules:

- `@assessment-engine` runs the assessment, writes the score to `learner/assessments.md` Results Log, awards the level, and appends `Level History` in `progress.md`. `@achievement-engine` writes `achievements/YYYY-MM-DD-<slug>-lN.md`.
- **Domain overrides:** `domains/<slug>/curriculum.md` may define stricter criteria (e.g., physical domains add technique-form gates; `driving` adds legal-age + licensed-instructor gates). Stricter always wins.
- **Physical domains:** promotion additionally requires `@safety-guardian` clearance — no level-up gated on intensity escalation without it.
- **No demotion.** Levels never decrease. > 30 days without a logged session adds a `rusty` note to `domains/INDEX.md` Status; re-entry starts with a refresh assessment that cannot award below current level.
- **Whole-person level** = median of active-domain levels (§2), shown in `STATE.md`.

## 5. Weekly Report Format — `reports/weekly/YYYY-Www.md`

Written by `@progress-report` every Friday (per `routine/checklists/weekly.md`). Required sections, in order:

```markdown
# Weekly Report — YYYY-Www (Sprint N, days X–Y)

## KPI Table
| Metric | This week | Target | Status (met/missed) | Trend vs last week |

## Pillar Summary
- Eat / Train / Sleep: adherence numbers + one-line state each (from pillars/*/metrics.md)

## Domain Rollup
| Domain | Level | Hours | Sessions | Streak | Delta |

## Streaks & Achievements
- Current streaks; badges/level-ups issued this week (links to achievements/)

## Flags & Carry-overs
- Safety flags (open/resolved); tasks carried into next sprint

## Recommendations
1. …  2. …  3. …

## NEXT: @weekly-review
```

**The "3 recommendations" rule:** exactly three — no more, no less. Each must (a) cite its evidence as a real file path or metric value, (b) name the skill that owns the fix, (c) be actionable inside next sprint. If fewer than three issues exist, recommendations may be growth levers (new elective, harder progression, deload timing) — never filler.

## 6. Monthly Report Format — `reports/monthly/YYYY-MM.md`

Whole-person review written by `@progress-report` + `@strategy-advisor`. Required sections:

```markdown
# Monthly Report — YYYY-MM

## Executive Summary        # ≤ 5 lines: trajectory, biggest win, biggest risk
## KPI Trend                # table: each §1 metric × the month's 4–5 weekly values
## Pillar Deep-Dives        # Eat / Train / Sleep: trend, notes, one adjustment each
## Domain Level Map         # full domains/INDEX.md snapshot: level, hours, status
## Level-ups & Achievements # monthly totals + links
## Safety & Wellbeing       # flags raised/resolved; sleep-debt trend; burnout signals
## Goals Check              # vs learner/goals.md — on track / behind / ahead
## Roadmap Adjustments      # changes applied to 30/60/90 roadmaps (roadmap-planner)
## Next-Month Priorities    # 3–5 items, each with owning skill

## NEXT: @roadmap-planner
```

## 7. Computation Integrity

- Every number in a report must trace to a file path listed in §1. If evidence is missing, report `n/a` — never estimate.
- Missed weeks are reported as gaps, not zeros.
- `@checklist-review` is the only writer of done/missed verdicts; other agents supply evidence only.
