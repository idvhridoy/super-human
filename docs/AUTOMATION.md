# Automation Contract — Autonomous Work vs. `[HUMAN]` Gates

What agents do without asking vs. what is parked behind human approval. Implementing the hard guardrails of `AGENTS.md` §Guardrails and prd.md §9.

---

## 1. Fully Autonomous (no approval needed)

Agents may do all of the following on their own, then hand off per `docs/HANDOFF.md`:

| Area | Examples |
|---|---|
| **Logging & counters** | `LOG.md` narration entries, `STATE.md` counters/streaks, `pillars/*/log.md` + `night-log.md`, `sessions.md` records |
| **Planning** | `routine/daily/YYYY-MM-DD.md` time-blocked plans, `pillars/training/periodization.md` cycles, `domains/<slug>/roadmap.md` drafts, weekly sprint goals |
| **Curricula & lessons** | `curriculum.md` content, `domain-mentor` lessons, drills, quizzes, `library/` curation |
| **Audits & reports** | `@checklist-review` audits, `reports/weekly|monthly/`, `routine/weekly/` reviews, KPI tables |
| **Assessments & achievements** | Level tests (`learner/assessments.md`), `L0`–`L10` updates in `progress.md`, badges in `achievements/` |
| **Scheduling (within rules)** | Session placement, rest blocks, conflict resolution inside `learner/schedule.md` constraints |
| **Internal routing** | `## NEXT:` handoffs, `@orchestrator` dispatch, `interest-scout` domain *proposals* (queued, not active) |

Boundary: autonomous planning stops where the human's body, health data, money, or calendar beyond declared constraints begins.

## 2. `[HUMAN]` Approval Gate (hard block — never auto-execute)

| Gated action | Typical requester | Human decides |
|---|---|---|
| Edit `learner/health.md` (injuries, allergies, restrictions) | `@safety-guardian`, `@nutritionist` | Add/change/remove health facts |
| Change dietary restrictions / allergen rules | `@nutritionist` | New meal-plan constraints |
| Injury-adjacent or post-flag training | `@run-coach`…, `@training-coordinator` | Resume/modify after a flag |
| Reduce sleep below profile minimum | `@schedule-manager`, `@daily-routine` | Any schedule cutting into sleep floor |
| Driving practice (always; theory is autonomous) | `@domain-mentor` for `driving` | Any practical/real-world step — legal age + licensed instructor required |
| Real-world transactions: buy, book, message, subscribe | any agent | All out-of-system actions |
| Activate or drop a domain in `domains/INDEX.md` | `@interest-scout` proposes | `queued` → `active`, or removal |
| Medical questions of any kind | any agent | Always escalates; agents never diagnose |

Mode check first: `learner/profile.md` `mode:` — `ward` → guardian approves; `adult` → learner approves. Either way the gate is a human, never an agent.

## 3. Gate Format — the `[HUMAN]` Queue

Pending approvals live in `STATE.md` under `## [HUMAN] Queue`. Format:

```markdown
## [HUMAN] Queue
- [ ] [HUMAN] <action> — proposed by @<skill> <ISO-8601>
      Why: <one-line reason>
      Context: <file paths the human should read>
      On approval: <exact next step / NEXT marker to write>
      On rejection: <fallback>
```

Example:

```markdown
- [ ] [HUMAN] Resume running after knee flag — proposed by @safety-guardian 2026-10-03T10:15+06
      Why: 5 rest days elapsed; flag severity downgraded to watch
      Context: STATE.md §Safety Flags, pillars/training/sessions.md, learner/health.md
      On approval: write `## NEXT: @run-coach` in domains/running/progress.md, cap RPE 5
      On rejection: extend mobility-only block 7 days, re-evaluate
```

Rules:
- An agent that raises a gate **stops that branch of work** — no partial execution.
- Approval is recorded by editing `- [ ]` → `- [x]` with an approver note + timestamp (`- [x] approved by <human> <ISO-8601>`); agents must not tick their own gates.
- Rejections are logged with reason in `LOG.md`; the fallback executes automatically.

## 4. Rate & Effort Limits

| Limit | Enforced by | Mechanism |
|---|---|---|
| Training: ≥1 full rest day per 7 | `@safety-guardian` veto over `@training-coordinator` | `pillars/training/periodization.md` must contain rest blocks; guardian scans nightly |
| Sleep floor: no plan may schedule below `learner/profile.md` minimum | `@schedule-manager` + `[HUMAN]` gate for exceptions | day plan blocked; gate queued instead |
| Sleep debt: 2+ flagged nights → intense training auto-blocked | `@safety-guardian` | flag in `STATE.md §Safety Flags`; only mobility/recovery sessions allowed |
| Session load: physical RPE caps per level in each coach's playbook | coaches + guardian audit | `sessions.md` RPE field audited vs `playbook.md` caps |
| New domains: max 1 activation per sprint | `@roadmap-planner` + gate | prevents dilution; `queued` backlog in `domains/INDEX.md` |
| Handoff depth: max 5 chained `## NEXT:` without human/`/orchestrator` touch | `@orchestrator` | chain parks on `## NEXT: @orchestrator` (see `docs/HANDOFF.md` §6) |
| Reporting: max 3 recommendations per `reports/` issue | `@progress-report` | keeps reviews actionable |
| Zero-days: every missed day needs a logged reason | `@checklist-review` | honesty guardrail — no silent streak breaks |

## 5. Audit Trail Expectations

Every automated action must be reconstructable from files alone:

- **Timestamped entries**: all writes are `##` sections with ISO-8601 (+06) stamps; append-only files (`LOG.md`, `sessions.md`, `night-log.md`) are never rewritten — corrections are new entries.
- **Evidence links**: level-ups, badges, and checklist ticks cite the file + section that proves them (`achievements/<id>.md`, `learner/assessments.md`).
- **Handoff trail**: `## NEXT:` markers form the chain-of-custody between agents; `LOG.md` records merges and conflicts.
- **Flags**: `STATE.md §Safety Flags` is the live register; every flag carries owner, timestamp, and resolution state. 0 unresolved flags is the success target (prd.md §10).
- **Nothing hidden**: no covert tracking, no hidden files — everything an agent records is visible to learner/guardian (prd.md NG3). `learner/private.md` stays gitignored and is never referenced in committed files.
