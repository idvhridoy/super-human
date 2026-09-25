# PRD — Super Human OS
### Autonomous AI Mentorship & Life-Orchestration System

| Field | Value |
|---|---|
| **Project** | `super-human` |
| **Version** | 1.0 |
| **Status** | Draft — ready for build |
| **Owner** | Md Hridoy Hossain (`hridoythebest`) |
| **Pattern source** | `client-hunt-agency` — file-based multi-agent orchestration |
| **Runtime target** | Devin CLI skills (agentskills-compatible `SKILL.md` format) |

---

## 1. Vision

> **What if a person had a world-class specialist for every skill, a nutritionist for every meal, a sleep expert for every night, and a chief-of-staff managing every hour — available 24/7, with a perfect memory of everything ever learned?**

A wealthy family can hire a running coach, a swim coach, a nutritionist, a sleep consultant, a language tutor, a kung-fu master, and a personal secretary — one elite mentor per domain. **Super Human OS replicates that entire staff as an AI agent orchestration**: every specialist is a skill file, every fact lives in git-tracked Markdown, and a 24/7 coordination layer turns the learner's time and effort into compounding, measurable growth — physically, mentally, intellectually, and socially.

The goal is not "a chatbot that gives advice." It is an **operating system for building a super human**: planned days, coached sessions, logged progress, enforced recovery, leveled skills, and a permanent record of the entire journey.

## 2. Problem Statement

Human potential dies in the same gaps as freelance income: the workout skipped, the lesson never reviewed, the sleep debt ignored, the interest never explored, the progress never measured. A single learner cannot simultaneously be their own coach, nutritionist, sleep scientist, tutor across 15+ subjects, scheduler, and analyst.

Existing tools solve fragments — a fitness app, a flashcard app, a habit tracker — but none coordinate the whole human. Super Human OS coordinates all of it through one orchestration layer where every specialist shares the same file-based state and hands off work through an explicit protocol.

## 3. Goals & Non-Goals

### Goals
- **G1** — Every domain of human development has a dedicated specialist agent with a curriculum, sessions, and progress tracking.
- **G2** — The three daily pillars (Eat, Train, Sleep) are managed as first-class subsystems, not afterthoughts.
- **G3** — One coordination layer produces the daily plan, audits it every evening, and reviews it every week — the learner only executes and reports.
- **G4** — 100% of state is Markdown in git: profile, curricula, session logs, metrics, achievements, health flags.
- **G5** — 24/7 availability: any agent can be invoked any time; state survives restarts, devices, and model changes.
- **G6** — Safety by design: age-appropriate gating, injury/overtraining prevention, human approval for anything medical, legal, or physical-risk.

### Non-Goals
- **NG1** — Not a medical device: no diagnosis, prescriptions, or clinical claims. Health flags escalate to humans/professionals.
- **NG2** — Not a replacement for real-world practice: agents plan, instruct, and review — the human performs the reps.
- **NG3** — No surveillance or covert tracking: every log is visible to the learner/guardian; nothing is hidden.
- **NG4** — No auto-execution of real-world actions (buying, booking, messaging) without an explicit human approval gate.

## 4. Design Principles (inherited from client-hunt-agency)

| Principle | Rule |
|---|---|
| **One skill per specialist** | `.devin/skills/<name>/SKILL.md` — each agent has exactly one job |
| **File-based state** | All state in `.md` files; agents hold nothing in memory between runs |
| **Handover protocol** | Agents read context files, do work, append `##` + ISO-8601 timestamp, then write `## NEXT: @<skill>` |
| **Human-in-the-loop** | Physical-risk, medical, dietary-restriction, and schedule-breaking changes require human approval |
| **7-day sprints** | Weekly rhythm: plan → execute → audit → review → adapt |
| **Template-first scaling** | New learning domains are created by copying `domains/TEMPLATE/`, not by writing new code |

## 5. Personas

| Persona | Description | Interaction |
|---|---|---|
| **The Learner** | The human being developed (child, teen, or adult). Executes plans, reports results, asks questions. | Tells agents what happened; receives plans, lessons, feedback |
| **The Guardian/Owner** | Parent/guardian or the learner themselves. Approves risky changes, sets priorities, reviews reports. | Approves gates; reads weekly reviews |
| **Specialist Agents** | Coach, nutritionist, mentor, etc. One domain each. | Read/write their domain's state files only |
| **The Orchestrator** | Routes requests, resolves conflicts between agents (e.g., training vs. sleep), enforces guardrails. | First and last stop for every request |

## 6. System Architecture Overview

### 6.1 The Three Daily Pillars (non-negotiable subsystems)

| Pillar | Agent | Responsibility |
|---|---|---|
| **Eat** | `nutritionist` | Daily meal plan, macros/micros, hydration, dietary restrictions, weekly menu, growth-phase adjustments |
| **Sleep** | `sleep-coach` + `sleep-guardian` | Sleep schedule, wind-down routine, environment checks; guardian monitors bedtime compliance, wake events, and sleep-debt flags |
| **Train** | `training-coordinator` + domain coaches | Periodized physical training across movement skills; recovery enforcement; session scheduling |

### 6.2 Learning Domains (mentor layer)

Each domain is a **directory** (`domains/<slug>/`) with its own curriculum, roadmap, session log, and metrics — the same way `client-hunt-agency` uses `platforms/<slug>/`. A shared **mentor engine** skill (`domain-mentor`) runs lessons for any domain, while high-risk physical domains get dedicated specialist skills.

**Core starter domains:**

| Category | Domains |
|---|---|
| **Physical (dedicated skills)** | Running · Swimming · Strength/Plyometrics (jumping) · Kung-Fu/Martial Arts · Mobility & Recovery |
| **Physical (age-gated)** | Driving (theory from day one; practical only at legal age with licensed human instructor) |
| **Languages & Expression** | Language mastery · Literature · Drawing & Visual Arts · Behavior/Etiquette/Communication |
| **Sciences** | Physics · Chemistry · Biology |
| **World Knowledge** | Geography · Geopolitics · General Law |
| **Mind & Strategy** | Brain training (memory, logic, speed) · Strategy & Decision-making |
| **Practical Arts** | Agriculture · Engineering/Making · + any interest/hobby discovered by `interest-scout` |

New domains (music, chess, coding, public speaking, finance…) are added by copying `domains/TEMPLATE/` — no new code required; `interest-scout` proposes them, `roadmap-planner` schedules them.

### 6.3 Operations Layer (the "chief of staff")

| Skill | Function |
|---|---|
| `orchestrator` | Master router — reads request + `## NEXT` markers, resolves cross-agent conflicts, dispatches specialists |
| `learner-profile` | Onboarding interviewer — fills `learner/*.md` (age, baseline assessments, health notes, interests, goals). **Run first.** |
| `daily-routine` | Morning planner — writes a time-blocked plan balancing eat/train/learn/sleep to `routine/daily/YYYY-MM-DD.md` |
| `checklist-review` | Evening auditor — ticks plan vs evidence, records done/missed + reasons, carries over misses, tracks streaks |
| `schedule-manager` | Calendar/conflict management — sessions, rest blocks, fixed commitments |
| `roadmap-planner` | 30/60/90-day + seasonal roadmaps per domain and overall |
| `assessment-engine` | Tests, quizzes, practical evaluations → levels each domain skill (L0–L10) |
| `achievement-engine` | Milestones, badges, streaks, level-ups → `achievements/` |
| `progress-report` | Weekly/monthly KPI reports per domain + whole-person summary with 3 recommendations |
| `activity-log` | Learner narrates what they did → LOG.md entry, STATE.md counters, routes evidence to domains |
| `interest-scout` | Detects aptitude/curiosity signals in logs, proposes new domains and electives |
| `safety-guardian` | Overtraining detection, injury flags, sleep-debt alerts, age-appropriateness gates, escalation to humans |
| `knowledge-librarian` | Curated books/videos/exercises per domain per level → `library/` |
| `nutritionist` | Meal plans, macros, hydration, logs — `pillars/nutrition/` |
| `sleep-coach` | Sleep hygiene, routines, schedule — `pillars/sleep/` |
| `sleep-guardian` | Night watch — bedtime compliance, wake logging, morning sleep report |
| `training-coordinator` | Cross-domain physical periodization — `pillars/training/` |
| `run-coach` / `swim-coach` / `strength-coach` / `kungfu-coach` | Dedicated physical specialists (technique, drills, session plans, safety) |
| `domain-mentor` | Generic mentor engine — teaches any `domains/<slug>/` using its curriculum files |
| `strategy-advisor` | Long-horizon path decisions — what to prioritize this quarter/year and why |
| `weekly-review` | Friday retro — sprint review, streak summary, next-week adjustments |

## 7. Workspace Layout

```
super-human/
├── .devin/
│   └── skills/<name>/SKILL.md      # one skill per agent (~24 specialists)
├── learner/                        # the human's profile (like owner/)
│   ├── profile.md                  # age, baseline stats, health notes, personality
│   ├── goals.md                    # short/long-term goals per pillar & domain
│   ├── health.md                   # injuries, allergies, restrictions (human-approved only)
│   ├── schedule.md                 # fixed commitments, available hours
│   ├── interests.md                # hobbies, curiosity log, elective queue
│   ├── assessments.md              # baseline + periodic test results
│   └── private.md                  # gitignored — sensitive personal data only
├── pillars/                        # the three daily subsystems
│   ├── nutrition/                  # meal-plan.md, log.md, metrics.md, playbook.md
│   ├── sleep/                      # routine.md, night-log.md, metrics.md, playbook.md
│   └── training/                   # periodization.md, sessions.md, metrics.md, playbook.md
├── domains/
│   ├── INDEX.md                    # all domains, levels, hours logged, next milestone
│   ├── TEMPLATE/                   # copy to create domains/<slug>/
│   └── <slug>/                     # curriculum.md, roadmap.md, sessions.md,
│                                 # progress.md, metrics.md, playbook.md, resources.md
├── routine/
│   ├── checklists/                 # standing daily/weekly/lifecycle checklists
│   ├── daily/YYYY-MM-DD.md         # time-blocked plans
│   └── weekly/YYYY-Www.md          # sprint reviews
├── reports/
│   ├── weekly/YYYY-Www.md          # cross-domain KPI summary
│   └── monthly/YYYY-MM.md          # whole-person progress report
├── achievements/                   # badges, milestones, level-up records
├── library/                        # curated resources per domain per level
├── STATE.md                        # live dashboard — levels, streaks, flags
├── LOG.md                          # append-only activity log
├── docs/                           # architecture, security, roadmap, handoff, ...
└── AGENTS.md                       # project rules for agents
```

## 8. State File Conventions

- `##` Markdown headings with ISO-8601 timestamps (`2026-09-25T14:30+06`).
- `tasks.md`-style checklists: `- [ ]` pending · `- [~]` in-progress · `- [x]` done.
- `sessions.md` is chronological — newest session appended at bottom with duration, exercises, RPE/notes.
- `metrics.md` tracks per-domain numbers: hours, level, streaks, assessment scores.
- `progress.md` holds level state: `L0–L10`, current milestone, blockers.
- `pillars/sleep/night-log.md`: bedtime, wake time, quality score, disturbances, guardian notes.
- `pillars/nutrition/log.md`: meals, macros hit/missed, hydration, energy notes.
- `## NEXT: @<skill>` marker = explicit handoff; in doubt route to `@orchestrator`.

## 9. Guardrails (hard requirements)

| # | Rule |
|---|---|
| 1 | **Human approval gates:** dietary restrictions/allergy changes, injury-adjacent training, medical questions, schedule reductions below minimum sleep, driving practice, any real-world transaction. |
| 2 | **Age-gating:** content and physical load are filtered by learner age; driving/firearms-like domains are theory-only until legal age + licensed human supervision. |
| 3 | **Recovery enforcement:** `safety-guardian` can veto `training-coordinator` plans — mandatory rest days, sleep-debt blocks intense training. |
| 4 | **No medical claims:** agents never diagnose; health flags always produce `## NEXT: human-consult`. |
| 5 | **Privacy:** `learner/private.md` and `.env*` gitignored; no sensitive data in committed files. |
| 6 | **Honesty:** agents log what actually happened — never inflate progress or fake checkmarks. `checklist-review` requires evidence. |
| 7 | **Wellbeing over metrics:** burnout signals override streaks; the system optimizes for decades, not days. |

## 10. Metrics & Success Criteria

| Metric | Source | Target |
|---|---|---|
| Daily plan completion | `checklist-review` | ≥ 85% tasks done per day |
| Sleep adherence | `pillars/sleep/metrics.md` | ≥ 90% nights within schedule window |
| Nutrition adherence | `pillars/nutrition/metrics.md` | ≥ 80% planned meals followed |
| Training consistency | `pillars/training/metrics.md` | ≥ 5 sessions/week across physical domains |
| Skill level-ups | `assessment-engine` | ≥ 1 level-up per active domain per month |
| Learning hours | `STATE.md` counters | tracked per domain, weekly trend ↑ |
| Streaks | `achievement-engine` | no zero-days without logged reason |
| Safety incidents | `safety-guardian` log | 0 unresolved flags |

## 11. Build Plan — MANDATORY SEQUENCE

> ⚠️ **Builders must follow this order exactly. Do not write skills before the foundation documents exist.**

### Phase 0 — Foundation documents FIRST (blocking)

Create **all** of the following `.md` files **before** writing a single `SKILL.md`. These define the contract every agent follows:

| Order | File | Purpose |
|---|---|---|
| 1 | `prd.md` | This document — product requirements |
| 2 | `AGENTS.md` | Project rules: workspace layout, handover protocol, state conventions, guardrails |
| 3 | `docs/ARCHITECTURE.md` | Agent topology, data flow, file ownership matrix |
| 4 | `docs/SECURITY.md` | Privacy model, guardrails enforcement, escalation paths |
| 5 | `docs/MASTER-ROADMAP.md` | 7-day sprint system + project phases |
| 6 | `docs/ORCHESTRATION.md` | How orchestrator routes, conflict-resolution rules, NEXT protocol |
| 7 | `docs/HANDOFF.md` | Agent-to-agent handoff contract + examples |
| 8 | `docs/KPI.md` | Metric definitions, formulas, report formats |
| 9 | `docs/AUTOMATION.md` | What is automated vs. human-gated |
| 10 | `docs/REFERENCES.md` | Methodologies per domain (periodization, spaced repetition, sleep science…) |
| 11 | `domains/INDEX.md` + `domains/TEMPLATE/` | Domain registry + copy-template |
| 12 | `routine/checklists/` | Standing daily/weekly/lifecycle checklists |
| 13 | `learner/*.md` | All learner profile files with TODO markers |
| 14 | `STATE.md` + `LOG.md` | Initialized dashboard + empty log |
| 15 | `.gitignore` + `README.md` | `private.md`, `.env*` excluded; professional README |

### Phase 1 — Core operations skills (parallel wave A)

`orchestrator` · `learner-profile` · `daily-routine` · `checklist-review` · `activity-log` · `schedule-manager` · `STATE.md` wiring

### Phase 2 — Pillar skills (parallel wave B)

`nutritionist` · `sleep-coach` · `sleep-guardian` · `training-coordinator` · `safety-guardian`

### Phase 3 — Domain layer (parallel wave C)

`domain-mentor` engine + `domains/TEMPLATE/` + dedicated physical coaches (`run-coach`, `swim-coach`, `strength-coach`, `kungfu-coach`) + seed all starter domains from §6.2 by copying the template.

### Phase 4 — Intelligence & polish (parallel wave D)

`assessment-engine` · `achievement-engine` · `progress-report` · `interest-scout` · `roadmap-planner` · `strategy-advisor` · `knowledge-librarian` · `weekly-review` + professional README (badges, banner, tables) + `docs/` cross-link verification.

**Execution rule:** use **maximum sub-agent parallelism in waves** — launch all skills in a wave concurrently as background agents with disjoint file ownership; verify wave N before starting wave N+1.

## 12. Acceptance Criteria

- [ ] All 15 Phase-0 foundation documents exist and are cross-linked
- [ ] ~24 `SKILL.md` files in `.devin/skills/`, each following the agentskills format
- [ ] 20+ starter domains seeded under `domains/` from `TEMPLATE/`
- [ ] `learner/` complete with TODO markers; `/learner-profile` interview fills it
- [ ] Full lifecycle works: `/daily-routine` → execute → `/checklist-review` → `/weekly-review` → `/progress-report`
- [ ] Every skill writes `## NEXT:` markers; `@orchestrator` resolves all conflicts
- [ ] Guardrails §9 enforced in every relevant skill's SKILL.md
- [ ] `git init` + `.gitignore` protecting `private.md` and `.env*`
- [ ] Professional README with banner, badges, and tables (same standard as client-hunt-agency)

## 13. Open Questions

1. Learner's starting age — drives age-gating thresholds and curriculum levels.
2. Fixed weekly commitments (school/work hours) — needed by `schedule-manager`.
3. Languages: which language(s) beyond English?
4. Budget for tools — are paid APIs (nutrition DBs, workout libs) allowed, or free-only?
5. Who holds guardian approval — the learner themselves (adult mode) or a parent (ward mode)? Support both via a `mode:` flag in `learner/profile.md`.

---

## 14. Builder Kickoff Prompt (copy-paste into the build agent)

```
Build the "Super Human OS" multi-agent orchestration system in this workspace
according to prd.md (in the repo root). It follows the same architecture as a
file-based agent agency: every specialist is a .devin/skills/<name>/SKILL.md,
all state lives in git-tracked Markdown, and agents hand off via
"## NEXT: @<skill>" markers.

MANDATORY SEQUENCE:
1. FIRST create all 15 Phase-0 foundation documents (§11 table) — AGENTS.md,
   docs/ARCHITECTURE.md, docs/SECURITY.md, docs/MASTER-ROADMAP.md,
   docs/ORCHESTRATION.md, docs/HANDOFF.md, docs/KPI.md, docs/AUTOMATION.md,
   docs/REFERENCES.md, domains/INDEX.md + TEMPLATE/, routine/checklists/,
   learner/*.md, STATE.md, LOG.md, .gitignore, README.md.
   Do NOT write any SKILL.md until all 15 exist.
2. THEN build skills in 4 parallel waves (§11 Phases 1-4) using maximum
   background sub-agent parallelism — one agent per skill, disjoint files.
3. Seed 20+ starter domains under domains/ from TEMPLATE/ (§6.2).
4. Enforce all 7 guardrails (§9) inside every relevant SKILL.md.
5. Finish with a professional README (banner image ref + shields.io badges +
   tables, same standard as a top open-source repo) and verify every
   acceptance criterion in §12.

Style: compact Markdown, ISO-8601 timestamps, no secrets, no comments unless
needed. Owner: Md Hridoy Hossain (hridoythebest), Dhaka, Bangladesh (UTC+6).
```
