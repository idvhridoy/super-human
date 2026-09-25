# Orchestration — Routing & Handoff Contract

How `@orchestrator` maps requests to skills, the `## NEXT:` marker protocol, conflict-resolution precedence, parallel wave rules, and error recovery. Binding on every `SKILL.md`. Companions: `docs/ARCHITECTURE.md` (ownership), `docs/SECURITY.md` (gates/escalation).

---

## 1. Routing Contract

Every request enters through `/orchestrator` unless the user invokes a skill directly (`/<skill-name>`). Direct invocation is allowed — the invoked skill then obeys this contract itself.

Orchestrator decision procedure:

1. **Load context** — read `STATE.md` (flags, HUMAN queue, next wave), `LOG.md` tail, and any `## NEXT:` marker that triggered this run.
2. **Check halts** — unresolved `block` Safety Flag or unanswered `human-consult` in scope → do not dispatch; surface the halt.
3. **Classify intent** → pick exactly one owning skill from §2 (file ownership decides ties — see `docs/ARCHITECTURE.md` §3).
4. **Dispatch** — write routing entry to `LOG.md` (`## <ts> ROUTE <intent> → @<skill>`) and invoke.
5. **Follow-through** — after the skill runs, read emitted NEXT markers; continue routing until `## NEXT: done` or a gate.

A skill invocation is a bounded transaction: read context → write owned files → append NEXT marker → exit. Skills never hold state between runs.

---

## 2. Routing Table

Intent → owning skill → primary write targets. When in doubt: `@orchestrator` re-classifies.

| Intent / trigger | Skill | Writes |
|---|---|---|
| Ambiguous, multi-step, conflict, "route this" | `orchestrator` | `LOG.md` route entries, `STATE.md` wave/HUMAN sections |
| Onboarding, "set up my profile", baseline interview | `learner-profile` | `learner/profile.md`, `goals.md`, `schedule.md`, `interests.md`, `assessments.md` |
| "Plan my day", morning kickoff | `daily-routine` | `routine/daily/YYYY-MM-DD.md`, `STATE.md` pillar status |
| "Audit today", evening review, tick checkboxes | `checklist-review` | `routine/daily/` audit append, `STATE.md` counters/streaks, `LOG.md` |
| "I did X", narrated activity, evidence drop | `activity-log` | `LOG.md` append, `STATE.md` counters, session-appends via merge |
| Calendar clash, commitments, free hours, "reschedule" | `schedule-manager` | `learner/schedule.md`, plan annotations via merge |
| 30/60/90-day plans, "what's my roadmap" | `roadmap-planner` | `domains/<slug>/roadmap.md`, `domains/INDEX.md` |
| Test, quiz, "evaluate me", level check | `assessment-engine` | `learner/assessments.md`, `domains/<slug>/progress.md` level, `metrics.md` scores |
| Badge, milestone, streak reward, level-up record | `achievement-engine` | `achievements/<id>.md`, `STATE.md` counters, `INDEX.md` level (merge) |
| "How am I doing", KPIs, weekly/monthly report | `progress-report` | `reports/weekly/YYYY-Www.md`, `reports/monthly/YYYY-MM.md` |
| Curiosity signal, "new domain?", elective proposal | `interest-scout` | `learner/interests.md`, `domains/INDEX.md` proposed row |
| Pain, injury, overtraining, sleep debt, age violation, veto | `safety-guardian` | `STATE.md` Safety Flags, `## VETO` appends |
| Book/video/exercise recommendation, resource curation | `knowledge-librarian` | `library/`, `domains/<slug>/resources.md` |
| Meal plan, macros, hydration, food log | `nutritionist` | `pillars/nutrition/*` |
| Sleep schedule, wind-down, hygiene | `sleep-coach` | `pillars/sleep/routine.md`, `playbook.md`, `metrics.md` (merge) |
| Bedtime/wake watch, night quality | `sleep-guardian` | `pillars/sleep/night-log.md`, `metrics.md` |
| Weekly training structure, periodization, load balance | `training-coordinator` | `pillars/training/*`, `domains/mobility/` |
| Running session, technique, run plan | `run-coach` | `domains/running/`, `pillars/training/sessions.md` append |
| Swim session, water skills | `swim-coach` | `domains/swimming/`, `pillars/training/sessions.md` append |
| Strength/plyometric session | `strength-coach` | `domains/strength/`, `pillars/training/sessions.md` append |
| Martial arts session | `kungfu-coach` | `domains/kung-fu/`, `pillars/training/sessions.md` append |
| Driving theory; gated practical | `driving-mentor` | `domains/driving/` |
| Lesson in any non-physical domain (language, physics, law, …) | `domain-mentor` | `domains/<slug>/*` for its slug |
| "What should I prioritize", quarter/year strategy | `strategy-advisor` | `learner/goals.md` priority section, `INDEX.md` ordering |
| Friday retro, sprint close, next-week adjustments | `weekly-review` | `routine/weekly/YYYY-Www.md` |
| Approval needed, gate question, medical/legal/transaction | `human-consult` (halt) | question in scope file + `STATE.md` HUMAN queue |

Special targets: `human-consult` = halt for human answer; `done` = chain complete. Neither is a skill.

---

## 3. NEXT-Marker Protocol

### 3.1 Format

Every skill ends its work by appending, in the file that best represents the work's state:

```markdown
## NEXT: @<skill-name>          — or: human-consult | done
Trigger: <what happened / what the next skill should do>
Scope: <file or slug affected>
```

Rules:

- Exactly one NEXT marker per completed unit of work.
- The marker names the *next owner*, not instructions on how to do their job.
- Missing/malformed markers are treated as `## NEXT: @orchestrator` (§6).
- A human answers gates with `## HUMAN: <decision>` + timestamp in the same file, then orchestrator resumes routing.

### 3.2 Worked examples

**Example A — narrated run:**

`LOG.md`:
```markdown
## 2026-09-25T18:40+06 — ACTIVITY
Learner: "ran 3km, 21min, felt knee twinge at km2"
Classified: physical/running + pain-signal
## NEXT: @run-coach
Trigger: log 3km/21min session; knee twinge needs screening
Scope: domains/running/
```

`run-coach` appends `domains/running/sessions.md` + a merge append to `pillars/training/sessions.md`, then:
```markdown
## NEXT: @safety-guardian
Trigger: knee twinge reported mid-run — assess before next session
Scope: domains/running
```

`safety-guardian` writes `STATE.md` flag `warn` + screening note, then either `## NEXT: done` (cleared) or `## NEXT: human-consult` (gate).

**Example B — evening audit cascade:**

`checklist-review` audits `routine/daily/2026-09-25.md`: 8/10 items evidenced → ticks; 2 missed → carried over with reasons. Emits in the daily file:
```markdown
## NEXT: @achievement-engine
Trigger: 10-day plan-completion streak hit — issue streak badge
Scope: STATE.md streaks
```

`achievement-engine` writes `achievements/streak-10d.md`, bumps `STATE.md` counters (merge), emits `## NEXT: done`.

**Example C — gate flow (ward mode):**

`nutritionist` reads a new allergy claim in `LOG.md` → writes in `pillars/nutrition/meal-plan.md`:
```markdown
## NEXT: human-consult
Gate: dietary restriction
Question: add peanut allergy to learner/health.md and rebuild week menu?
Context: LOG.md 2026-09-25T18:41+06
```

Guardian appends `## HUMAN: approved — confirmed allergy 2026-09-26T08:10+06`. Orchestrator routes back to `nutritionist` to apply the change; the `learner/health.md` edit is applied per approval.

---

## 4. Conflict-Resolution Precedence

When two skills' outputs collide, the higher row wins. Loser work is not deleted — it is annotated and the winner emits the NEXT marker.

| # | Conflict | Winner | Mechanism |
|---|---|---|---|
| 1 | Training plan vs sleep/recovery | **`safety-guardian`** | `## VETO` append overrides `periodization.md`; `training-coordinator` must replan within limits (guardrail 3) |
| 2 | Any plan vs minimum sleep | **`sleep-coach`/`safety-guardian`** | sub-minimum sleep requires `human-consult`; plans auto-lose until approved |
| 3 | Two skills want the same time slot | **`schedule-manager`** | fixed commitments > pillar minimums > planned sessions > electives; emits resolved block to `daily-routine` |
| 4 | Claimed progress vs evidence | **`checklist-review`** | no evidence → `- [ ]`; inflated claims → Safety Flag (guardrail 6) |
| 5 | Level disagreements (coach vs test) | **`assessment-engine`** | assessed level in `progress.md` is authoritative; coach may request re-test via NEXT |
| 6 | Domain priority disputes | **`strategy-advisor`** decides, **`roadmap-planner`** executes | INDEX ordering + roadmaps updated after decision |
| 7 | Learner request vs active gate/flag | **gate/flag** | halted scope stays halted regardless of request |
| 8 | Anything unresolved above | **`orchestrator`** | arbitrates; second occurrence of same conflict → `human-consult` |

Precedence summary for plan-writing: `human-consult` > `safety-guardian` > `schedule-manager` (fixed commitments) > pillar owners (`sleep-coach`, `nutritionist`, `training-coordinator`) > `daily-routine` (assembles the day) > domain skills (fill session content).

---

## 5. Parallel Wave Rules

Per `docs/ARCHITECTURE.md` §5 — runtime summary:

1. `orchestrator`/`daily-routine` declares the wave in `STATE.md → Next Parallel Wave` with a file-claim per skill.
2. **Disjoint claims only** — same file in two claims → wave is invalid; orchestrator splits it.
3. Shared files take append-merge roles only: `LOG.md` free-append; `STATE.md` section-locked; `sessions.md`/`night-log.md` append-only.
4. **Merge step** after fan-out: orchestrator serializes queued appends, then resolves all NEXT markers as a fresh routing pass.
5. `safety-guardian` reads all wave output in audit role; its writes happen in merge.
6. Gate results (approvals, assessments that change level) are wave-terminal — downstream consumers run next wave against settled state.

Daily runtime waves: morning wave (`daily-routine` + pillar planners feeding it) → execution (learner + `activity-log` continuous) → evening wave (`checklist-review` → `achievement-engine` → `safety-guardian` audit) → Friday (`weekly-review` + `progress-report` + `roadmap-planner`).

---

## 6. Error Recovery

| Failure | Recovery |
|---|---|
| Missing expected file | create from `domains/TEMPLATE/` (domain files) or section skeleton per `AGENTS.md` §State Conventions; log the creation in `LOG.md` |
| Malformed/absent NEXT marker | treat as `## NEXT: @orchestrator`; orchestrator re-reads output and routes |
| NEXT loop (same marker pair twice, no state change) | orchestrator breaks loop → `human-consult` with deadlock summary |
| Concurrent-write violation | detected in merge step: first claim wins, second writer's change queued and applied by orchestrator as annotated append; repeat offender gets a `safety-guardian` `info` flag |
| Skill produces no expected writes | orchestrator verifies claims post-wave; empty run → retry once → `human-consult` |
| Corrupt entry (bad timestamp, non-chronological append) | do not edit history — append `## CORRECTION` entry referencing the bad line |
| Gate answered but owning skill gone/renamed | orchestrator applies the approved change directly with `## APPLIED` note |
| `STATE.md`/`LOG.md` structurally damaged | restore last committed version from git, replay appends from `LOG.md` tail (never fabricate — guardrail 6) |
| Unknown domain slug in request | `## NEXT: @interest-scout` to propose; human approves; `roadmap-planner` copies `domains/TEMPLATE/` |

Universal fallback: **when in doubt, `## NEXT: @orchestrator`** — and orchestrator, when in doubt, escalates to `## NEXT: human-consult`.
