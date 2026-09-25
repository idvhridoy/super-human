# Security & Safety — Super Human OS

Privacy model, guardrail enforcement map, escalation paths, age-gating, prohibited actions, and incident logging. Binding on every `SKILL.md`. Source rules: `AGENTS.md` §Guardrails; enforcement mechanics: `docs/ARCHITECTURE.md`, `docs/ORCHESTRATION.md`.

---

## 1. Privacy Model

### 1.1 Data classification

| Class | Examples | Storage | Rule |
|---|---|---|---|
| Public-progress | levels, streaks, badges, session counts | committed `.md` | free to commit |
| Personal-context | age, goals, interests, schedule, general health notes | `learner/*.md` (committed) | commit OK; keep minimal — enough to coach, nothing more |
| Sensitive | full legal name, address, phone, IDs, medical detail, credentials, guardian contacts | `learner/private.md` only | **gitignored — never committed** |
| Secrets | API keys, tokens | `.env*` | **gitignored — never committed, never logged** |

`.gitignore` must contain (already enforced in repo): `learner/private.md`, `.env`, `.env.*`, `*.key`, `*.pem`.

### 1.2 Rules for agents

- Never copy content from `private.md`/`.env*` into any committed file, log, or output. Agents must function without reading `private.md`; if a task genuinely needs sensitive data, emit `## NEXT: human-consult`.
- `learner/health.md` contains coaching-relevant restrictions only (e.g., "knee strain — no plyometrics"), never diagnoses or clinical detail. Human-approved edits only (guardrail 1).
- No real names/locations of third parties (schoolmates, instructors) in committed files — use roles ("swim instructor").
- Timestamps use `+06` offset only; no GPS or device metadata anywhere.

### 1.3 Minor's data handling (ward mode)

When `learner/profile.md → Mode: ward`:

- Guardian approves every gate in §2; the learner's own "ok" is not approval.
- Committed files must be safe for a child record: no embarrassing detail, no behavioral speculation — factual entries only (guardrail 6 keeps logs factual; `checklist-review` writes evidence, not judgment).
- `interest-scout` proposals and all external-facing content (messages, posts, uploads) require guardian gate.
- Data minimization: `learner-profile` collects only fields the skills consume.

---

## 2. Guardrail Enforcement Matrix

The 7 hard rules from `AGENTS.md`, expanded with the enforcing skill and enforcement point.

| # | Rule | Primary enforcer | Supporting | Enforcement point |
|---|---|---|---|---|
| 1 | **Human approval gates** — dietary/allergy changes, injury-adjacent training, medical questions, sleep below minimum, driving practice, real-world transactions | `orchestrator` (halts dispatch) | every skill must detect gate conditions in its scope | Affected skill writes `## NEXT: human-consult` + entry in `STATE.md → [HUMAN] Queue`; no further writes to gated scope until human reply appended. `nutritionist` gates `health.md`-linked meal changes; coaches gate injury-adjacent plans; `driving-mentor` gates all practical work; `schedule-manager`/`sleep-coach` gate sub-minimum sleep |
| 2 | **Age-gating** — content & load filtered by `learner/profile.md` age | `safety-guardian` (audit) | `learner-profile` (sets age/mode); `domain-mentor` + coaches (apply gates); `knowledge-librarian` (age-rates resources) | Every curriculum/session/resource write must state the assumed age band (§4); `safety-guardian` wave-audit flags violations; `driving-mentor` hard-blocks practical content |
| 3 | **Recovery enforcement** — `safety-guardian` vetoes `training-coordinator`; mandatory rest; sleep debt blocks intensity | `safety-guardian` | `sleep-guardian` (supplies debt data), `training-coordinator` (must check flags before planning) | `training-coordinator` reads `STATE.md → Safety Flags` + `pillars/sleep/metrics.md` before writing `periodization.md`; veto = `## VETO` append block in `pillars/training/` + flag in `STATE.md`; veto cleared only by `safety-guardian` or human |
| 4 | **No medical claims** — never diagnose/prescribe/treat | all skills (self-check) | `safety-guardian` (audit) | Any health question → respond with general info only + `## NEXT: human-consult`. Banned phrasing checked at write time (§5); incidents logged to Safety Flags |
| 5 | **Privacy** — `private.md`/`.env*` gitignored; nothing sensitive committed | every skill | `orchestrator` (pre-commit scan when committing) | §1 classification; any write containing `private.md` content, keys, IDs → blocked + incident flag. `activity-log` strips personal identifiers before writing `LOG.md` |
| 6 | **Honesty** — log only what happened; evidence required | `checklist-review` | `activity-log` (source entries), `safety-guardian` (audit) | `- [x]` requires a linked entry in `LOG.md`/`sessions.md`/pillar log; unverifiable claims → `- [ ]` with `reason: no evidence`. Suspected inflation → Safety Flag |
| 7 | **Wellbeing over metrics** — burnout signals override streaks | `safety-guardian` | `weekly-review` (trend detection), `checklist-review` (miss patterns), `sleep-guardian` (fatigue data) | Burnout/overtraining pattern → flag + forced deload directive in `pillars/training/` + `## NEXT: @daily-routine` to lighten plan. Streaks may be intentionally broken; log reason, no penalty |

Enforcement principles:

- **Self-check first, audit second, human third.** Each skill enforces rules in its own writes; `safety-guardian` audits every wave; humans hold the gates.
- A skill that cannot enforce a rule (e.g., it lacks context) must route instead of guessing: `## NEXT: @safety-guardian` or `human-consult`.
- Guardrail text is duplicated inside each relevant `SKILL.md` — the docs are the contract, the skills are the mechanism.

---

## 3. Escalation Paths

### 3.1 `## NEXT: human-consult`

The universal halt marker. Not a skill — a stop state.

```
## NEXT: human-consult
Gate: dietary-restriction change
Question: learner reports peanut allergy — add to health.md + rebuild meal plan?
Options: approve / reject / modify
Context: pillars/nutrition/meal-plan.md 2026-09-25T14:30+06 entry
```

Rules:

- The emitting skill stops writing to the gated scope immediately.
- `orchestrator` mirrors the item into `STATE.md → [HUMAN] Queue` as `- [ ]`.
- Human answers by appending `## HUMAN: <decision>` + timestamp in the same file (ward mode: guardian; adult mode: learner).
- After an answer, `orchestrator` routes the decision to the owning skill to apply.
- A gate item older than 48h blocks dependent work; `orchestrator` re-surfaces it in `routine/daily/` plans.

### 3.2 Escalation ladder

| Trigger | Path |
|---|---|
| Ambiguous routing / missing file / protocol error | any skill → `@orchestrator` |
| Safety-relevant signal (pain, fatigue, sleep debt, age violation) | any skill → `@safety-guardian` → maybe `human-consult` |
| Medical/dietary/transactional/driving-practical question | skill → `human-consult` directly |
| Veto or unresolved flag blocking the plan | `safety-guardian` → `human-consult` |
| Same NEXT loop twice without state change | `orchestrator` → `human-consult` (deadlock break) |

---

## 4. Age-Gating Mechanics

Age and mode live in `learner/profile.md` (`Age:`, `Mode: ward|adult`). All gated skills read them before producing content.

| Band | Label | Effect |
|---|---|---|
| < 7 | early-child | play-based only; no structured load; sessions ≤ 15 min; guardian present for all physical activity |
| 7–12 | child | fundamental movement; bodyweight-only strength; no maximal efforts; academic content at grade level; ward mode expected |
| 13–15 | teen | graded training loads; technique before intensity; theory-open domains (incl. driving theory) |
| 16–17 | youth | near-adult loads with `safety-guardian` review; practical driving only if legal jurisdiction age met + licensed instructor |
| 18+ | adult | full catalog; adult mode may self-approve gates |

Domain-specific gates:

| Domain | Gate |
|---|---|
| `driving` | theory from any age; practical = legal age **and** licensed human instructor **and** `human-consult` approval per session — enforced in `driving-mentor` SKILL.md and re-audited by `safety-guardian` |
| `strength` | <13: bodyweight/plyo-play only; 13–15: light external load supervised; 16+: progressive loading |
| `kung-fu` | <13: no contact sparring; 13–15: light contact drills; 16+: contact with protective rules |
| `swimming` | any age but never solo — supervision line mandatory in every `swim-coach` session plan |
| content domains (`law`, `geopolitics`, `literature`) | `knowledge-librarian` + `domain-mentor` filter resources by age band; mature themes deferred |

If `Age:` is `TODO`, gated skills assume **youngest applicable band** and emit `## NEXT: @learner-profile` to fill it.

---

## 5. Prohibited Actions (all skills, always)

- Diagnose, prescribe, treat, or interpret medical symptoms beyond "see a professional" — no exceptions (guardrail 4).
- Execute real-world transactions: purchases, bookings, messages, posts, uploads, account creation — draft only, `human-consult` to send.
- Write to `learner/private.md`, `.env*`, or copy their contents anywhere.
- Edit `learner/health.md` without a `## HUMAN:` approval on record.
- Override or delete a `## VETO` block, Safety Flag, or another agent's entry; history files are append-only.
- Schedule sleep below the age-band minimum or training through an active flag.
- Fake, inflate, or backfill progress/checkmarks/metrics (guardrail 6).
- Contact, surveil, or collect data on third parties; no scraping of the learner's accounts/devices.
- Produce practical instructions for gated physical domains to under-age learners.
- Create additional accounts/profiles or misrepresent identity anywhere.

---

## 6. Incident Logging — `STATE.md → Safety Flags`

`safety-guardian` is the sole writer of the Safety Flags section. Any skill can request a flag via `## NEXT: @safety-guardian`.

Flag entry format:

```
### FLAG-YYYYMMDD-NN — <severity: info|warn|block> — <status: open|monitoring|resolved>
- Raised: 2026-09-25T14:30+06 by @<skill>
- Scope: pillars/training | domains/<slug> | learner | system
- Signal: <evidence — file + entry reference>
- Rule: <guardrail # violated / risk type>
- Action: <veto | deload | gate | note>
- Cleared: <timestamp + by whom> (only on resolution)
```

Rules:

- `block` flags freeze the affected scope; `warn` flags require `safety-guardian` review before the next wave; `info` is advisory.
- 0 unresolved `block` flags is a hard acceptance metric (`prd.md` §10); `weekly-review` must report flag counts.
- Incident patterns (repeat flags in one domain) escalate automatically to `human-consult` on the third occurrence.
