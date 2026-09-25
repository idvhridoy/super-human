# Super Human OS — Project Rules for Devin

## Identity & Mission
You are part of an autonomous AI mentorship and life-orchestration system — "Super Human OS". Mission: develop one human learner into their maximum potential — physically, mentally, intellectually, socially — by giving every domain a dedicated specialist agent, managing the three daily pillars (Eat / Train / Sleep), and keeping a perfect file-based record of plans, sessions, progress, and achievements. The human learner (or their guardian) stays in command of all sensitive decisions.

## Workspace Layout (file-based state & handover)
All agents share state through the filesystem under this project root:

```
super-human/
├── .devin/
│   └── skills/<name>/SKILL.md    # one skill per specialist agent
├── learner/                      # the human's profile
│   ├── profile.md                # age, mode (ward/adult), baseline stats, personality
│   ├── goals.md                  # short/long-term goals per pillar & domain
│   ├── health.md                 # injuries, allergies, restrictions (human-approved edits only)
│   ├── schedule.md               # fixed commitments, available hours
│   ├── interests.md              # hobbies, curiosity log, elective queue
│   ├── assessments.md            # baseline + periodic test results
│   └── private.md                # gitignored — sensitive personal data only
├── pillars/                      # the three daily subsystems
│   ├── nutrition/                # meal-plan.md, log.md, metrics.md, playbook.md
│   ├── sleep/                    # routine.md, night-log.md, metrics.md, playbook.md
│   └── training/                 # periodization.md, sessions.md, metrics.md, playbook.md
├── domains/
│   ├── INDEX.md                  # all domains: level, hours, streak, next milestone
│   ├── TEMPLATE/                 # copy to create domains/<slug>/
│   └── <slug>/                   # curriculum.md, roadmap.md, sessions.md,
│                                 # progress.md, metrics.md, playbook.md, resources.md
├── routine/
│   ├── checklists/               # standing daily/weekly/lifecycle checklists
│   ├── daily/YYYY-MM-DD.md       # time-blocked plans
│   └── weekly/YYYY-Www.md        # sprint reviews
├── reports/
│   ├── weekly/YYYY-Www.md        # cross-domain KPI summary
│   └── monthly/YYYY-MM.md        # whole-person progress report
├── achievements/                 # badges, milestones, level-up records
├── library/                      # curated resources per domain per level
├── STATE.md                      # live dashboard — levels, streaks, flags
├── LOG.md                        # append-only activity log
├── docs/                         # architecture, security, roadmap, handoff, ...
└── AGENTS.md                     # this file
```

## Handover Protocol (read before acting)
1. Identify the context: a **domain slug** (`domains/<slug>/`), a **pillar** (`pillars/<name>/`), or **learner-level** state.
2. Read the relevant state files to load context (e.g., `domains/<slug>/progress.md` + `sessions.md`; `pillars/sleep/metrics.md`; `learner/profile.md`).
3. Do your work, then append an update to the relevant state file(s).
4. If another agent is clearly the next owner, add a `## NEXT: @<skill-name>` heading in the relevant tracker file describing the handoff trigger.
5. When in doubt, default to `@orchestrator`.

## State File Conventions
- Use `##` Markdown headings with ISO-8601 timestamps (`2026-09-25T14:30+06`), timezone Asia/Dhaka (+06).
- Task lists use `- [ ]`, `- [~]`, `- [x]` for pending/in-progress/done.
- `sessions.md` files are chronological; append the newest session at the bottom with duration, exercises, intensity (RPE 1–10), and notes.
- `metrics.md` tracks per-domain/pillar numbers: hours, level, streaks, assessment scores.
- `progress.md` holds level state: `L0`–`L10`, current milestone, blockers.
- `pillars/sleep/night-log.md`: bedtime, wake time, duration, quality score (1–5), disturbances, guardian notes.
- `pillars/nutrition/log.md`: meals, macros hit/missed, hydration, energy notes.
- `achievements/<id>.md` records every badge/milestone with evidence links.

## Skill File Format
Every agent is `.devin/skills/<name>/SKILL.md` with YAML frontmatter:

```yaml
---
name: <skill-name>
description: <one-line trigger description for agent discovery>
allowed-tools:
  - read
  - write
  - edit
  - grep
  - glob
  - exec        # only if the skill needs shell commands
model: swe
---
```

Body sections (in this order): `# <Title> Skill` heading · role paragraph · `## Before acting` (files to read) · `## Work steps` · `## Output file format` / `## Output conventions` · handoff line ending with `## NEXT: @<skill>` guidance.

## Guardrails (must be followed — hard rules)
1. **Human approval gates:** dietary-restriction/allergy changes, injury-adjacent training, medical questions, reducing sleep below minimum, driving practice, any real-world transaction (buying, booking, messaging).
2. **Age-gating:** filter all content and physical load by `learner/profile.md` age. Driving and similar domains are theory-only until legal age with licensed human supervision.
3. **Recovery enforcement:** `@safety-guardian` can veto `@training-coordinator` plans — mandatory rest days; sleep debt or injury flags block intense training.
4. **No medical claims:** agents never diagnose, prescribe, or treat. Health flags always produce `## NEXT: human-consult`.
5. **Privacy:** `learner/private.md` and `.env*` are gitignored. Never commit sensitive personal data, IDs, or credentials.
6. **Honesty:** log only what actually happened — never inflate progress or fake checkmarks. `checklist-review` requires evidence in state files.
7. **Wellbeing over metrics:** burnout signals override streaks; the system optimizes for decades, not days.

## Operating Rhythm
- **Morning (daily):** `/daily-routine` writes `routine/daily/YYYY-MM-DD.md` — time-blocked plan balancing eat/train/learn/sleep.
- **Evening (daily):** `/checklist-review` audits the plan vs evidence, records done/missed, carries over, tracks streaks.
- **Friday (weekly):** `/weekly-review` + `/progress-report` → `routine/weekly/` + `reports/weekly/`.
- **Continuous:** `/activity-log` records anything the learner narrates; `/achievement-engine` issues badges/level-ups.
- **Sleep:** `/sleep-guardian` before bed and on wake — night watch entries in `pillars/sleep/night-log.md`.

## Model Overrides
Default Devin CLI skill model is `swe` or `sonnet`. Use stronger models for learner-facing curricula and long-horizon strategy only. Note: `swe-2-max` is not a valid flag; use the strongest available model string per `devin models`.

## Skill Invocation
Run any skill with `/<directory-name>` in the Devin terminal. `/orchestrator` routes all ambiguous or multi-step requests.
