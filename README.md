<p align="center">
  <img src="assets/banner.png" alt="Super Human OS" width="100%">
</p>

<div align="center">

# Super Human OS

### An autonomous AI staff of world-class mentors — for one human

[![Skills](https://img.shields.io/badge/specialist_skills-32-brightgreen?style=flat-square)](#whats-inside--32-specialist-agents)
[![Domains](https://img.shields.io/badge/learning_domains-34%2B-blueviolet?style=flat-square)](#learning-domains)
[![Pillars](https://img.shields.io/badge/daily_pillars-3-orange?style=flat-square)](#the-three-daily-pillars)
[![Docs](https://img.shields.io/badge/docs-8-blue?style=flat-square)](#docs)
[![Levels](https://img.shields.io/badge/level_system-L0→L10-9cf?style=flat-square)](#metrics--progression)
[![State](https://img.shields.io/badge/state-100%25%20markdown-ff6600?style=flat-square)](#file-based-state--handover)
[![Runtime](https://img.shields.io/badge/runtime-Devin%20CLI-black?style=flat-square)](https://devin.ai)
[![Cadence](https://img.shields.io/badge/cadence-7--day%20sprints-blueviolet?style=flat-square)](#operating-rhythm)
[![GitHub stars](https://img.shields.io/github/stars/idvhridoy/super-human?style=flat-square)](https://github.com/idvhridoy/super-human/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/idvhridoy/super-human?style=flat-square)](https://github.com/idvhridoy/super-human/network/members)
[![Last Commit](https://img.shields.io/github/last-commit/idvhridoy/super-human?style=flat-square)](https://github.com/idvhridoy/super-human/commits/main)

**32 specialist agents · 3 daily pillars · 34 learning domains · 24/7 file-based memory · L0→L10 progression**

[Quick Start](#quick-start) · [The Three Pillars](#the-three-daily-pillars) · [Domains](#learning-domains) · [How It Runs](#how-it-runs) · [Safety](#safety-guardrails) · [Docs](#docs)

</div>

---

> ⚠️ **Human development system — safety-first by design.** No medical claims, ever. Age-gated content, mandatory rest rules, a `safety-guardian` agent with veto power over all training, and human approval required for health, diet, and real-world actions. `learner/private.md` is gitignored. See [Safety guardrails](#safety-guardrails) and `docs/SECURITY.md`.

## The richest education money can buy — rebuilt as software

A billionaire's child gets a running coach, a swim instructor, a nutritionist planning every meal, a sleep consultant guarding every night, and a private tutor for every subject — plus a chief of staff scheduling it all. **Super Human OS gives one learner that entire staff as AI agents.**

Each specialist is a [Devin CLI](https://devin.ai) skill. Every fact — curricula, session logs, sleep reports, meal adherence, assessment scores, badges — lives in git-tracked Markdown. Agents never hold state in memory: they read the files, do their work, append results, and hand off via `## NEXT: @<skill>` markers. A 24/7 coordination layer plans every morning, audits every evening, and reviews every week — the learner only executes and reports.

## Quick start

```bash
git clone https://github.com/idvhridoy/super-human.git
cd super-human
```

```bash
/learner-profile            # onboarding interview — fills learner/*.md (run first)
/assessment-engine          # baseline battery: physical, cognitive, knowledge
/daily-routine              # every morning — time-blocked eat/train/learn/sleep plan
/checklist-review           # every evening — audit vs evidence + streaks
/weekly-review              # every Friday — sprint retro + next-week focus
/progress-report            # weekly KPI rollup + 3 recommendations
```

Everything else routes itself: `/orchestrator` reads `## NEXT:` markers and dispatches the right specialist.

## The three daily pillars

| Pillar | Agent(s) | Owns | What it does |
|---|---|---|---|
| **Eat** | `nutritionist` | `pillars/nutrition/` | Weekly meal plans, macro/hydration targets, adherence tracking — built around health restrictions and training load |
| **Train** | `training-coordinator` + physical coaches | `pillars/training/` | Periodized weekly splits across all physical domains, enforced recovery floors, age-appropriate load caps |
| **Sleep** | `sleep-coach` + `sleep-guardian` | `pillars/sleep/` | Age-appropriate sleep window, wind-down routine, night-watch reports, sleep-debt alerts that downgrade training |

## Learning domains

34 seeded domains, each with a full **L0→L10 curriculum**, 30/60/90-day roadmap, session log, progress tracker, metrics, playbook, and leveled resources. New domains are added by copying `domains/TEMPLATE/` — `interest-scout` proposes them from curiosity signals, humans approve.

| Category | Domains |
|---|---|
| **Physical** | Running · Swimming · Strength & Plyometrics · Kung-Fu · Mobility & Recovery · Driving *(theory-only until legal age)* |
| **Sciences** | Mathematics *(foundational — gates physics/engineering)* · Physics · Chemistry · Biology |
| **World** | Geography · Geopolitics · History · General Law |
| **Mind** | Brain Training · Strategy & Decisions · Psychology · Philosophy · Mindfulness |
| **Expression** | Language · Literature · Drawing · Music · Communication *(public speaking/debate/negotiation)* · Behavior & Etiquette |
| **Practical** | Coding · Digital Literacy · Finance · First Aid · Cooking · Agriculture · Engineering & Making · Survival · Life Skills · *+ any elective you add* |

## What's inside — 32 specialist agents

| Function | Agents | Key capabilities |
|---|---|---|
| **Orchestration** | 5 | `orchestrator` routing · learner onboarding · morning plans · evening audits · Friday retros |
| **Pillars** | 5 | Nutrition · sleep coaching · night watch · training periodization · safety veto power |
| **Coaches & mentors** | 10 | Dedicated coaches (run/swim/strength/kung-fu/driving/first-aid/communication/mindfulness) + `domain-mentor` engine + `project-mentor` cross-domain capstones |
| **Mind & growth** | 3 | `mindset-coach` discipline/habits · `learning-coach` meta-learning · `career-mentor` profession paths |
| **Intelligence** | 7 | Testing & level promotion · badges · KPI reports · interest detection · roadmaps · strategy · resource curation |
| **Operations** | 2 | Activity logging · calendar/conflict authority |

## How it runs

```
Learner: "I ran 5km today and finished my physics worksheet"

  1. /activity-log       → LOG.md entry + STATE.md counters
  2. /run-coach          → domains/running/sessions.md: 5km, pace, RPE
  3. /domain-mentor      → domains/physics/sessions.md: worksheet scored
  4. /assessment-engine  → milestone met → promotes running L3→L4
  5. /achievement-engine → issues "L4 Runner" badge in achievements/
  6. /sleep-guardian     → tonight: debt-check before tomorrow's intensity
  7. /checklist-review   → evening audit vs the morning plan
```

**Without the system**, each step is a forgotten app or notebook. **With it**, the staff never sleeps, never forgets, and every specialist sees the same files.

## File-based state & handover

```
super-human/
├── .devin/skills/<name>/SKILL.md   # 25 specialist agents
├── learner/                        # profile · goals · health · schedule ·
│   │                               # interests · assessments
│   └── private.md                  # gitignored
├── pillars/                        # nutrition/ · sleep/ · training/
├── domains/                        # INDEX.md · TEMPLATE/ · 20 <slug>/ dirs
│                                   #   curriculum · roadmap · sessions ·
│                                   #   progress · metrics · playbook · resources
├── routine/                        # checklists · daily/ · weekly/
├── reports/                        # weekly KPIs · monthly whole-person
├── achievements/                   # badges & level-up records
├── library/                        # curated cross-domain resources
├── STATE.md / LOG.md               # live dashboard · append-only log
└── docs/                           # 8 foundation documents
```

**Handover protocol** — every agent: (1) reads its context files, (2) works, (3) appends a `##` heading with an ISO-8601 timestamp, (4) writes `## NEXT: @<skill>` for the next owner. In doubt → `@orchestrator`.

<details>
<summary><strong>🛡️ Safety guardrails — hard rules, enforced by dedicated agents</strong></summary>

&nbsp;

- **Human approval gates** — health changes, dietary restrictions, injury-adjacent training, sleep-minimum reductions, driving practice, real-world transactions.
- **Age-gating** — all content and physical load filtered by `learner/profile.md` age; driving is theory-only until legal age + licensed human instructor.
- **Recovery veto** — `safety-guardian` can override `training-coordinator`: mandatory rest days, sleep-debt → auto-downgrade to mobility.
- **No medical claims** — health flags always produce `## NEXT: human-consult`.
- **Honesty by design** — `checklist-review` requires file evidence; narrated claims alone never count.
- **Wellbeing over metrics** — burnout signals override streaks; optimized for decades, not days.

</details>

<details>
<summary><strong>🗓️ Operating rhythm — 7-day sprints</strong></summary>

&nbsp;

| Cadence | Agent | Output |
|---|---|---|
| Morning | `/daily-routine` | `routine/daily/YYYY-MM-DD.md` |
| Bedtime + wake | `/sleep-guardian` | `pillars/sleep/night-log.md` entries |
| Evening | `/activity-log` + `/checklist-review` | LOG.md + audited plan + streaks |
| Friday | `/weekly-review` + `/progress-report` | Sprint retro + KPI report + 3 recommendations |
| Continuous | `interest-scout` · `achievement-engine` | Elective proposals · badges |

</details>

<details>
<summary><strong>📊 Metrics &amp; progression — L0 to L10</strong></summary>

&nbsp;

Every domain levels **L0 (unstarted) → L10 (mastery)**, promoted only by `assessment-engine` evidence — never self-report. Tracked per `docs/KPI.md`: daily plan completion ≥85%, sleep adherence ≥90%, nutrition adherence ≥80%, ≥5 training sessions/week, ≥1 level-up per active domain per month.

</details>

## Docs

`docs/ARCHITECTURE.md` · `docs/SECURITY.md` · `docs/ORCHESTRATION.md` · `docs/MASTER-ROADMAP.md` · `docs/HANDOFF.md` · `docs/AUTOMATION.md` · `docs/KPI.md` · `docs/REFERENCES.md` — plus `prd.md` (full product spec) and `AGENTS.md` (the agent contract).

## Owner

**Md Hridoy Hossain** — AI Engineer &amp; automation specialist, Dhaka, Bangladesh.
GitHub: [@hridoythebest](https://github.com/hridoythebest) · [@idvhridoy](https://github.com/idvhridoy) · Email: hridoythebest@gmail.com

## License

Personal project — shared publicly for transparency and portfolio purposes. No open-source license is granted; contact the owner for reuse.

---

<div align="center">

**If this system inspires your own AI orchestration, consider giving it a ⭐**

[⭐ Star](https://github.com/idvhridoy/super-human/stargazers) · [🍴 Fork](https://github.com/idvhridoy/super-human/fork) · [💬 Issues](https://github.com/idvhridoy/super-human/issues)

Built by [Md Hridoy Hossain](https://github.com/idvhridoy) — *"Coding is a passion and not profession"*

</div>
