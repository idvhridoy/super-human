# Handoff Contract — Agent-to-Agent `## NEXT:` Protocol

How agents pass work to each other. One job per skill; when a specialist finishes its job, it hands the baton to exactly one next agent through an explicit marker in a state file. `@orchestrator` is the default when the next owner is unclear.

---

## 1. Marker Syntax

```markdown
## NEXT: @<skill-name>
- Trigger: <what just happened / why this agent is next>
- Context: <paths the next agent must read>
- Expiry: <ISO-8601 timestamp — after this the marker is stale>
```

Rules:
- Marker text is exactly `## NEXT: @<skill-name>` on its own heading line — no extra words in the heading.
- Trigger/Context/Expiry bullets are required. Context must name real file paths.
- Only **one** NEXT marker is authoritative per tracker file — the newest one at the bottom of the file.
- Special targets: `@orchestrator` (default router), `human` or `human-consult` (escalation, not a skill — produces a `[HUMAN]` item in `STATE.md` instead of a handoff).

## 2. Where the Marker Lives

The marker goes in the **tracker file for the work just done** — never in logs, reports, or READMEs.

| Work type | Tracker file that holds `## NEXT:` |
|---|---|
| Domain session / assessment / level change | `domains/<slug>/progress.md` |
| Pillar work (meal plan, training plan, sleep check) | `pillars/nutrition/meal-plan.md` or `log.md` · `pillars/training/sessions.md` or `periodization.md` · `pillars/sleep/night-log.md` or `routine.md` |
| Learner-level routing, flags, achievements, cross-domain | `STATE.md` |
| Safety veto / health flag | `STATE.md` `## Safety Flags` + `## NEXT:` |

Append-only files (`LOG.md`, `sessions.md`, `night-log.md`) may carry a marker on their newest entry; the effective marker is the latest timestamped one in the tracker file.

## 3. Handoff Sequence (per AGENTS.md)

1. Agent reads its context files (`## Before acting` in its `SKILL.md`).
2. Does its work; appends a `## <title>` section with ISO-8601 timestamp (+06) to its own files.
3. Writes `## NEXT: @<skill>` + bullets in the tracker file.
4. Stops. The next agent (or the human invoking `/<skill>`) picks it up.

## 4. Worked End-to-End Example

**Learner reports:** *"I ran 5km this morning, felt easy."*

**Step 1 — `@activity-log`.** Appends to `LOG.md`, increments `STATE.md` counters (training sessions, learning minutes). In `STATE.md`:

```markdown
## 2026-10-03T09:20+06 — Activity logged: run
- Evidence: learner narration, 5km, easy effort
## NEXT: @run-coach
- Trigger: unlogged running session needs a session entry + RPE record
- Context: LOG.md §2026-10-03T09:20; pillars/training/sessions.md
- Expiry: 2026-10-04T09:20+06
```

**Step 2 — `@run-coach`.** Appends session to `pillars/training/sessions.md` and `domains/running/sessions.md` (5km, easy → RPE 4, notes on form/distance). Updates `domains/running/metrics.md` hours. In `domains/running/progress.md`:

```markdown
## 2026-10-03T09:35+06 — Session #4 recorded
- 5km easy, RPE 4. Weekly volume now at L2 assessment threshold.
## NEXT: @assessment-engine
- Trigger: threshold met per domains/running/curriculum.md — schedule L1→L2 check
- Context: domains/running/sessions.md, domains/running/curriculum.md §L2
- Expiry: 2026-10-05T09:35+06
```

**Step 3 — `@assessment-engine`.** Runs/schedules the check, records result in `learner/assessments.md`, bumps `domains/running/progress.md` level to `L2` with evidence link. In `domains/running/progress.md`:

```markdown
## 2026-10-03T18:10+06 — Level set: L1 → L2
- Evidence: learner/assessments.md §run L2 check, sessions.md #4
## NEXT: @achievement-engine
- Trigger: level-up → badge + counter update
- Context: domains/running/progress.md, achievements/
- Expiry: 2026-10-04T18:10+06
```

**Step 4 — `@achievement-engine`.** Writes `achievements/run-l2.md`, increments `STATE.md` counters (Level-ups, Achievements), updates `domains/INDEX.md` row. In `STATE.md`:

```markdown
## 2026-10-03T18:25+06 — Achievement: Running L2
## NEXT: @checklist-review
- Trigger: fold into tonight's daily audit
- Context: achievements/run-l2.md
- Expiry: 2026-10-03T23:59+06
```

Result: one narration → four clean handoffs → permanent record at every level.

## 5. Error Cases

| Case | Detection | Resolution |
|---|---|---|
| **Missing marker** | Agent finishes work but no `## NEXT:` in tracker | Treat as end-of-chain; `@orchestrator` routes on next invocation. Log the gap in `LOG.md` — do not invent a handoff. |
| **Conflicting markers** | Two agents write different NEXT markers to the same tracker | The **latest timestamp** wins; the older is void. If timestamps tie or both still look live, `STATE.md` gets `## NEXT: @orchestrator` and the conflict is noted in `LOG.md`. |
| **Stale marker** | Marker `Expiry` timestamp has passed and nothing ran | Marker is void. `@orchestrator` (or `checklist-review`) re-queues the intent as a `- [ ]` carry-over item or closes it with a reason. Never execute an expired handoff silently. |
| **Dead target** | `## NEXT: @<skill>` names a skill with no `SKILL.md` | Route to `@orchestrator`; record the broken link in `LOG.md`. |
| **Health flag in chain** | Any step surfaces injury/medical signal | Chain aborts → `## NEXT: human-consult` + `[HUMAN]` item in `STATE.md`. No further agent acts on that branch until cleared (see `docs/AUTOMATION.md`). |
| **Orphaned state** | `progress.md`/`sessions.md` updated but counters in `STATE.md` disagree | `@checklist-review` reconciles nightly from evidence files — domain and pillar files are the source of truth; counters are rebuilt, not trusted. |

## 6. Loop Prevention

- A skill may not hand off to itself (`@x` → `@x` is void; becomes a carry-over).
- Max **5 consecutive automated handoffs** without a human/`/orchestrator` touch; beyond that the chain parks on `## NEXT: @orchestrator` to prevent ping-pong between specialists.
