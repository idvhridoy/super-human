# Architecture — Super Human OS

Defines agent topology, data flow, file ownership, the mentor-engine pattern, and the parallel wave execution model. Binding on every `SKILL.md`. See `AGENTS.md` for state conventions, `docs/ORCHESTRATION.md` for routing mechanics, `docs/SECURITY.md` for guardrail enforcement.

---

## 1. Agent Topology

Five layers. Every skill lives at `.devin/skills/<name>/SKILL.md` and holds no state between runs — all memory is the filesystem.

```
┌──────────────────────────────────────────────────────────────────┐
│ L0  HUMAN          Learner (+ Guardian in ward mode)             │
│                    Approves gates · executes plans · narrates    │
├──────────────────────────────────────────────────────────────────┤
│ L1  ORCHESTRATION  orchestrator                                  │
│                    Router, conflict arbiter, wave scheduler      │
├──────────────────────────────────────────────────────────────────┤
│ L2  OPS LAYER      learner-profile · daily-routine               │
│    (chief of staff) checklist-review · schedule-manager          │
│                    activity-log · weekly-review                  │
├──────────────────────────────────────────────────────────────────┤
│ L3  PILLAR LAYER   nutritionist · sleep-coach · sleep-guardian   │
│    (Eat/Train/Sleep) training-coordinator · safety-guardian      │
├──────────────────────────────────────────────────────────────────┤
│ L4  DOMAIN MENTORS run-coach · swim-coach · strength-coach       │
│                    kungfu-coach · driving-mentor · domain-mentor │
├──────────────────────────────────────────────────────────────────┤
│ L5  INTELLIGENCE   assessment-engine · achievement-engine        │
│                    progress-report · interest-scout              │
│                    roadmap-planner · strategy-advisor            │
│                    knowledge-librarian                           │
└──────────────────────────────────────────────────────────────────┘
```

Layer rules:

| Rule | Detail |
|---|---|
| Downward dispatch | Upper layers invoke lower layers via `## NEXT:` markers; lower layers never call upward directly — they emit `## NEXT: @orchestrator` |
| Peer isolation | Same-layer skills never write each other's files; coordination goes through shared state + NEXT markers |
| Human gate | Any layer may emit `## NEXT: human-consult` — execution halts until a human answers in the file |
| Singleton safety | `safety-guardian` (L3) has veto power over L3/L4 physical output regardless of dispatch direction |

---

## 2. Data Flow

```
   Learner / Guardian
        │  narrates, requests, approves
        ▼
   ┌─────────────┐        ambiguous / multi-step        ┌─────────────┐
   │ orchestrator│ ◄────────────────────────────────── │  NEXT marker │
   │  (router)   │ ──── resolves conflicts ───────────► │  in any file │
   └──────┬──────┘                                      └─────────────┘
          │ dispatches ONE owner per request
          ▼
 ┌─────────────────┬─────────────────┬─────────────────┐
 │ OPS             │ PILLARS         │ DOMAINS         │ INTELLIGENCE
 │ daily-routine ─►│ nutritionist ──►│ coaches /       │ assessment ─►
 │ writes plan     │ writes meal plan│ domain-mentor   │ levels progress
 │                 │                 │ write sessions  │
 │ checklist-◄──── │ sleep-guardian ►│                 │ achievement ►
 │ review audits   │ writes night-log│                 │ badges
 │                 │                 │                 │
 │ activity-log ──►│ training-coord ►│                 │ progress-rpt ►
 │ LOG.md          │ periodization   │                 │ reports/
 │                 │                 │                 │
 │ safety-guardian ◄── reads ALL of the above ── vetoes/flags
 └────────┬────────┴────────┬────────┴────────┬────────┘
          ▼                 ▼                 ▼
 ┌─────────────────────────────────────────────────────────┐
 │ FILESYSTEM STATE (git-tracked Markdown)                  │
 │ learner/ · pillars/ · domains/ · routine/ · reports/     │
 │ achievements/ · library/ · STATE.md · LOG.md             │
 └─────────────────────────────────────────────────────────┘
          │
          ▼  weekly rhythm
   routine/daily/ → checklist-review → routine/weekly/ →
   reports/weekly/ → next sprint plan (7-day sprint loop)
```

Standard request lifecycle:

1. Learner (or a NEXT marker) raises a request.
2. `orchestrator` reads `STATE.md` + relevant context, selects the owning skill.
3. Owning skill reads its context files, writes updates (`##` + ISO-8601 `+06` timestamp), appends `## NEXT: @<skill>` or `## NEXT: human-consult` or `## NEXT: done`.
4. `activity-log` / `checklist-review` fold results into `LOG.md` and `STATE.md` counters.
5. Evening audit and Friday review close the loop; `safety-guardian` may interrupt at any step.

---

## 3. File Ownership Matrix

**Invariant: no two concurrently-running agents write the same file.** Each file has exactly one owning skill. Non-owners may *read* freely; they signal needed changes via `## NEXT:` markers.

### 3.1 Learner files

| File | Owner (writer) | Readers |
|---|---|---|
| `learner/profile.md` | `learner-profile` | all |
| `learner/goals.md` | `learner-profile` (init) · `strategy-advisor` (priority section, via merge step) | all |
| `learner/health.md` | **human only** — `learner-profile` may draft; human approves edit | `safety-guardian`, coaches, `nutritionist` |
| `learner/schedule.md` | `schedule-manager` | `daily-routine`, all planners |
| `learner/interests.md` | `interest-scout` | `learner-profile`, `strategy-advisor` |
| `learner/assessments.md` | `assessment-engine` | all mentors |
| `learner/private.md` | human only (gitignored); agents must never require it | none required |

### 3.2 Pillar files

| File | Owner | Merge writers (append-only, merge step) |
|---|---|---|
| `pillars/nutrition/meal-plan.md` | `nutritionist` | — |
| `pillars/nutrition/log.md` | `nutritionist` | `activity-log` (routed meal entries) |
| `pillars/nutrition/metrics.md`, `playbook.md` | `nutritionist` | — |
| `pillars/sleep/routine.md`, `playbook.md` | `sleep-coach` | — |
| `pillars/sleep/night-log.md` | `sleep-guardian` | `activity-log` (self-reported wake events) |
| `pillars/sleep/metrics.md` | `sleep-guardian` | `sleep-coach` (merge step) |
| `pillars/training/periodization.md`, `playbook.md` | `training-coordinator` | `safety-guardian` (`## VETO` append blocks only) |
| `pillars/training/sessions.md` | `training-coordinator` | physical coaches append own session records (merge step) |
| `pillars/training/metrics.md` | `training-coordinator` | — |

### 3.3 Domain files (`domains/<slug>/`)

Domain ownership is by slug routing, not by file — one skill owns all seven files of a slug:

| Slug | Owning skill |
|---|---|
| `running` | `run-coach` |
| `swimming` | `swim-coach` |
| `strength` | `strength-coach` |
| `kung-fu` | `kungfu-coach` |
| `driving` | `driving-mentor` (theory-only until age gate clears) |
| `mobility` | `training-coordinator` |
| all other slugs (`language`, `physics`, `literature`, …) | `domain-mentor` |

Within an owned slug: owner writes `curriculum.md`, `roadmap.md`, `sessions.md`, `progress.md`, `metrics.md`, `playbook.md`, `resources.md`. Merge writers: `assessment-engine` → `progress.md` level line + `metrics.md` scores; `activity-log` → `sessions.md` append; `knowledge-librarian` → `resources.md` append; `roadmap-planner` → `roadmap.md` (merge step). Two merge writers never run in the same wave against the same file.

`domains/INDEX.md`: owner `roadmap-planner`; merge writers `interest-scout` (proposed rows), `achievement-engine` (level column after level-up — merge step).

### 3.4 Routine, reports, achievements, library

| File | Owner |
|---|---|
| `routine/daily/YYYY-MM-DD.md` | `daily-routine` (morning create) → `checklist-review` (evening audit append). Time-sliced, never concurrent |
| `routine/weekly/YYYY-Www.md` | `weekly-review` |
| `routine/checklists/*` | `orchestrator` (maintained at build/review time) |
| `reports/weekly/YYYY-Www.md`, `reports/monthly/YYYY-MM.md` | `progress-report` |
| `achievements/<id>.md` | `achievement-engine` |
| `library/` | `knowledge-librarian` |
| `docs/` | human + build agents (not runtime skills) |

### 3.5 Shared root files

| File | Access model |
|---|---|
| `LOG.md` | **Append-only.** Any skill may append a `##` + timestamp entry. No edits, no deletes, no reordering. |
| `STATE.md` | **Section-owned.** `safety-guardian` owns *Safety Flags*; `orchestrator` owns *Next Parallel Wave* + *HUMAN Queue*; `activity-log`/`checklist-review`/`achievement-engine` update *Counters* and *Streaks* via read-modify-write of their own lines only, at merge step. Pillar-status cells updated by pillar owners. |

Merge step = a serialized phase after a parallel wave where the orchestrator applies queued appends/section edits one writer at a time. Direct concurrent writes to shared files are a protocol violation (§6).

---

## 4. The Mentor-Engine Pattern

Two skill archetypes serve `domains/<slug>/`:

### 4.1 `domain-mentor` — generic engine

One parameterized skill that teaches **any** domain by reading that slug's files:

```
input: slug → reads domains/<slug>/curriculum.md + progress.md
            → runs lesson/session per playbook.md
            → appends sessions.md, updates metrics.md
            → checks assessment triggers → ## NEXT: @assessment-engine
```

Used for all non-physical-risk domains (languages, sciences, world knowledge, mind, practical arts, expression). New domains require **zero new code**: copy `domains/TEMPLATE/` → `domains/<slug>/`, register in `domains/INDEX.md` — the engine picks it up by slug.

### 4.2 Dedicated physical coach skills

`run-coach`, `swim-coach`, `strength-coach`, `kungfu-coach`, `driving-mentor` are separate skills because physical coaching needs:

- technique cues, drill libraries, and RPE calibration baked into the skill body;
- hardcoded load limits and injury-screening steps before every session;
- mandatory `safety-guardian` interplay (pre-session flag check, post-session strain report);
- domain-specific age gates (e.g., `driving-mentor`: theory from `domains/driving/curriculum.md` any age; practical sessions only at legal age + licensed human instructor — enforced inside the skill and re-checked by `safety-guardian`).

Physical coaches write both their `domains/<slug>/` files and append session records to `pillars/training/sessions.md` (merge step), so `training-coordinator` sees total physical load for periodization.

### 4.3 Selection rule

`orchestrator` routes by `domains/INDEX.md` slug → owner column (§3.3). If a slug has no dedicated coach, it goes to `domain-mentor`. A dedicated coach may still emit `## NEXT: @domain-mentor` for theory lessons inside its own slug.

---

## 5. Parallel Wave Execution Model

Work is executed in waves (per `prd.md` §11 and daily planning):

| Phase | Rule |
|---|---|
| 1. Plan | `orchestrator` (build) or `daily-routine` (runtime) declares the wave in `STATE.md → Next Parallel Wave`: skill list + file ownership claim per skill |
| 2. Verify disjointness | No file appears in two claims. Shared files (`LOG.md`, `STATE.md`, `sessions.md`) restricted to append-merge roles |
| 3. Fan out | All skills in the wave run concurrently as background agents |
| 4. Merge step | Orchestrator applies queued appends to shared files serially; resolves NEXT markers emitted during the wave |
| 5. Audit | `safety-guardian` scans wave output for flags; `checklist-review` (daily) or orchestrator verifies expected writes landed |
| 6. Next | Write new `Next Parallel Wave` or `## NEXT:` continuation |

Wave membership constraints:

- One owner per file per wave (§3 invariant).
- `safety-guardian` may run in any wave as a read-auditor; its veto write happens in the merge step.
- Gate-producing skills (`assessment-engine` results that change level, `interest-scout` proposals) run at wave end so downstream skills see settled state next wave.
- Ward-mode gates pause the wave until the human answers — pending items carry to `STATE.md → [HUMAN] Queue`.

---

## 6. Invariants (enforced everywhere)

1. **Single writer per file per wave** — violations route to `@orchestrator` merge.
2. **Append-only history** — `LOG.md`, `sessions.md`, `night-log.md`, `pillars/nutrition/log.md` are never rewritten; corrections are new entries.
3. **Evidence before checkmarks** — `checklist-review` ticks `- [x]` only with a linked log/session entry (guardrail 6).
4. **Safety veto outranks plans** — a `## VETO` block in `pillars/training/` or a Safety Flag in `STATE.md` blocks physical output until cleared (guardrail 3).
5. **Timestamp everything** — every write starts `## YYYY-MM-DDThh:mm+06` (Asia/Dhaka).
6. **No silent state** — every agent ends with `## NEXT: @<skill>` / `## NEXT: human-consult` / `## NEXT: done`; orphans route to `@orchestrator`.
