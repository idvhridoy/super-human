<p align="center">
  <img src="assets/banner.png" alt="Super Human OS" width="100%">
</p>

<div align="center">

# Super Human OS

### An autonomous AI staff of world-class mentors — for one human

[![Specialists](https://img.shields.io/badge/specialist_agents-33-brightgreen?style=flat-square)](#-05--33-specialist-agents)
[![Domains](https://img.shields.io/badge/learning_domains-53%2B-blueviolet?style=flat-square)](#-04--53-learning-domains)
[![Pillars](https://img.shields.io/badge/daily_pillars-3-orange?style=flat-square)](#-03--the-three-daily-pillars)
[![Levels](https://img.shields.io/badge/progression-L0→L10-9cf?style=flat-square)](#-07--progression-system)
[![State](https://img.shields.io/badge/state-100%25%20markdown-ff6600?style=flat-square)](#-09--file-based-state)
[![Runtime](https://img.shields.io/badge/runtime-Devin%20CLI-black?style=flat-square)](https://devin.ai)
[![Cadence](https://img.shields.io/badge/cadence-7--day%20sprints-blueviolet?style=flat-square)](#-08--operating-rhythm)
[![Docs](https://img.shields.io/badge/docs-8-blue?style=flat-square)](#-10--documentation--quick-start)
[![GitHub stars](https://img.shields.io/github/stars/idvhridoy/super-human?style=flat-square)](https://github.com/idvhridoy/super-human/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/idvhridoy/super-human?style=flat-square)](https://github.com/idvhridoy/super-human/network/members)
[![Last Commit](https://img.shields.io/github/last-commit/idvhridoy/super-human?style=flat-square)](https://github.com/idvhridoy/super-human/commits/main)

**33 specialist agents · 3 daily pillars · 53 learning domains · 24/7 file-based memory · L0→L10 progression**

[01 Vision](#-01--the-vision) · [02 Architecture](#-02--architecture) · [03 Pillars](#-03--the-three-daily-pillars) · [04 Domains](#-04--53-learning-domains) · [05 Agents](#-05--33-specialist-agents) · [06 Orchestration](#-06--orchestration--handover) · [07 Progression](#-07--progression-system) · [08 Rhythm](#-08--operating-rhythm) · [09 State](#-09--file-based-state) · [10 Docs](#-10--documentation--quick-start)

</div>

---

> ⚠️ **Human development system — safety-first by design.** No medical claims, ever. Age-gated content, mandatory rest rules, a `safety-guardian` agent with veto power over all training, and human approval required for health, diet, and real-world actions. `learner/private.md` is gitignored. See `docs/SECURITY.md`.

## 🧭 01 — The Vision

A billionaire's child gets a running coach, a swim instructor, a nutritionist planning every meal, a sleep consultant guarding every night, and a private tutor for every subject — plus a chief of staff scheduling it all. **Super Human OS gives one learner that entire staff as AI agents.**

It is not a chatbot that gives advice. It is an **operating system for building a super human**: planned days, coached sessions, logged progress, enforced recovery, leveled skills across 53 domains, and a permanent record of the entire journey. The human learner only executes and reports — the staff handles everything else, 24/7.

Two flagship tracks sit above the rest: **Body Engineering** — audit the human machine (resting HR, VO2max, lipids, composition), then rebuild it on proven science (zone-2 base, progressive overload, combined aerobic+resistance, commando-grade endurance benchmarks as far targets) — and **Brain Engineering** — audit the mind (reaction, working memory, stress profile), then rebuild it (exercise→BDNF scheduling, dual-n-back, military-validated Stress Inoculation Training) until the best reaction is the default reaction.

## 🏗️ 02 — Architecture

Markdown-only multi-agent orchestration, same proven pattern as an autonomous agency:

- **Every specialist is a skill** — `.devin/skills/<name>/SKILL.md`, one job each
- **All state is files** — git-tracked Markdown; agents hold nothing in memory between runs
- **Explicit handover** — agents append `##` + ISO-8601 timestamp and write `## NEXT: @<skill>` markers
- **Human in command** — approval gates for health, money, legal, and physical-risk actions
- **Template scaling** — new domains spawn from `domains/TEMPLATE/`, no new code

## 🍎🏋️🌙 03 — The Three Daily Pillars

The non-negotiable subsystems everything else is scheduled around:

| Pillar | Agent(s) | Owns | What it does |
|---|---|---|---|
| **Eat** | `nutritionist` | `pillars/nutrition/` | Weekly meal plans, macro/hydration targets, adherence tracking — built around health restrictions and training load |
| **Train** | `training-coordinator` + physical coaches | `pillars/training/` | Periodized weekly splits across physical domains, enforced recovery floors, age-appropriate load caps |
| **Sleep** | `sleep-coach` + `sleep-guardian` | `pillars/sleep/` | Age-appropriate sleep window, wind-down routine, night-watch reports, sleep-debt alerts that downgrade training |

## 📚 04 — 53 Learning Domains

Each domain is a full workspace: **L0→L10 curriculum · 30/60/90 roadmap · sessions log · progress tracker · metrics · playbook · leveled resources**. New domains spawn from `domains/TEMPLATE/` — `interest-scout` proposes, humans approve.

🏆 **Flagship domains:** `body-engineering` + `brain-engineering` — the assess→engineer→exceed pipelines for the human machine, built on exercise-physiology and cognitive-science research.

| Category | Domains |
|---|---|
| **Physical** (8) | 🏆 **Body Engineering** *(audit → zone-2 base → VO2max → rucking → commando standards)* · Running · Swimming · Strength & Plyometrics · Kung-Fu · Mobility & Recovery · Team Sports · Driving *(theory-only until legal age)* |
| **Sciences** (5) | Mathematics *(foundational — gates physics/engineering)* · Physics · Chemistry · Biology · Astronomy |
| **World** (6) | Geography · Geopolitics · History · General Law · Statecraft *(the smart ruler)* · Situational Mastery *(read & handle any situation smoothly)* |
| **Mind** (13) | 🏆 **Brain Engineering** *(cognitive audit → BDNF/sleep levers → stress inoculation → best-reaction training)* · Brain Training · Mind Hacks · Critical Thinking · Judgment *(the judge — evidence only)* · Observation *(360° awareness)* · Fact-Checking *(misinformation defense)* · Creativity · Strategy & Decisions · Psychology · Philosophy · Mindfulness · Purpose *(ikigai)* |
| **Expression** (8) | Language · Literature · Drawing · Music · Communication *(speaking/debate/negotiation)* · Behavior & Etiquette · Leadership · Teaching |
| **Practical** (13) | Technology · Coding · Digital Literacy · Finance · Health Literacy · First Aid · Cooking · Agriculture · Engineering & Making · OSINT *(ethical public-source intel)* · Troubleshooting *(reproduce→isolate→fix)* · Survival · Life Skills |

## 🤖 05 — 33 Specialist Agents

| Function | Count | Agents & capabilities |
|---|---|---|
| **Orchestration** | 5 | `orchestrator` master router · `learner-profile` onboarding · `daily-routine` morning plans · `checklist-review` evening audits · `weekly-review` Friday retros |
| **Pillars** | 5 | `nutritionist` · `sleep-coach` · `sleep-guardian` · `training-coordinator` · `safety-guardian` (veto power) |
| **Coaches & mentors** | 11 | `run-coach` · `swim-coach` · `strength-coach` · `kungfu-coach` · `driving-mentor` · `first-aid-instructor` · `communication-coach` · `mindfulness-coach` · `scenario-coach` · `domain-mentor` engine (teaches any domain) · `project-mentor` (cross-domain capstones) |
| **Mind & growth** | 3 | `mindset-coach` habits/discipline · `learning-coach` meta-learning (Feynman, spaced repetition) · `career-mentor` profession paths |
| **Intelligence** | 7 | `assessment-engine` levels · `achievement-engine` badges · `progress-report` KPIs · `interest-scout` · `roadmap-planner` · `strategy-advisor` · `knowledge-librarian` |
| **Operations** | 2 | `activity-log` · `schedule-manager` |

## 🔀 06 — Orchestration & Handover

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

`/orchestrator` resolves every ambiguous request via the routing table, enforces file-ownership rules for parallel waves, and defaults safety conflicts to `safety-guardian`.

## 📈 07 — Progression System

- **Levels** — every domain runs **L0 (unstarted) → L10 (mastery)**; promotion requires `assessment-engine` evidence, never self-report
- **Assessments** — baseline battery at onboarding + periodic re-tests logged in `learner/assessments.md`
- **Achievements** — `achievement-engine` issues evidence-linked badges: consistency, mastery, effort, breadth, pillars
- **Metrics** — per-domain hours/streaks/KPIs roll up to `STATE.md` and `reports/weekly/`
- **Streaks** — tracked daily; burnout signals override streaks (wellbeing > numbers)

## 🗓️ 08 — Operating Rhythm

| Cadence | Agent | Output |
|---|---|---|
| Morning | `/daily-routine` | `routine/daily/YYYY-MM-DD.md` time-blocked plan |
| Bedtime + wake | `/sleep-guardian` | `pillars/sleep/night-log.md` entries |
| Evening | `/activity-log` + `/checklist-review` | LOG.md + audited plan + streaks |
| Friday | `/weekly-review` + `/progress-report` | Sprint retro + KPI report + 3 recommendations |
| Continuous | `interest-scout` · `achievement-engine` · `safety-guardian` | Electives · badges · safety flags |

## 🛡️ 09 — File-Based State

```
super-human/
├── .devin/skills/<33 specialists>/SKILL.md
├── learner/                 # profile · goals · health · schedule ·
│   │                        # interests · assessments · career-exploration
│   └── private.md           # gitignored
├── pillars/                 # nutrition/ · sleep/ · training/
├── domains/                 # INDEX.md · TEMPLATE/ · 53 <slug>/ workspaces
├── routine/                 # checklists · daily/ · weekly/
├── reports/                 # weekly KPIs · monthly whole-person
├── achievements/            # badges & level-up records
├── projects/                # cross-domain capstones (project-mentor)
├── library/                 # curated cross-domain resources
├── STATE.md / LOG.md        # live dashboard · append-only log
├── docs/                    # 8 foundation documents
├── prd.md                   # full product spec
└── AGENTS.md                # the agent contract
```

**Guardrails enforced in state:** human approval gates · age-gating · recovery veto · no medical claims · privacy gitignore · evidence-based checkmarks · wellbeing over metrics.

## 📖 10 — Documentation & Quick Start

**Docs:** `prd.md` · `AGENTS.md` · `docs/ARCHITECTURE.md` · `docs/SECURITY.md` · `docs/ORCHESTRATION.md` · `docs/MASTER-ROADMAP.md` · `docs/HANDOFF.md` · `docs/AUTOMATION.md` · `docs/KPI.md` · `docs/REFERENCES.md`

**Quick start:**

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
