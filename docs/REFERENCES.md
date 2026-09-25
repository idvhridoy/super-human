# REFERENCES — Methodology Canon

> Evidence-based notes every specialist cites when making prescriptions or
> curriculum decisions. Agents cite inline as `(per docs/REFERENCES.md §N)` in
> state files and handoffs. These are educational defaults — the learner's
> `learner/health.md`, `learner/profile.md` age, and human approval gates always
> override. Nothing here is medical advice; anything clinical routes to
> `## NEXT: human-consult`.

---

## 1. Physical Training — Progressive Overload & Periodization

Cited by: `@training-coordinator`, `@run-coach`, `@swim-coach`, `@strength-coach`, `@kungfu-coach`, `@safety-guardian` · State: `pillars/training/periodization.md`, `pillars/training/sessions.md`, `domains/<physical>/sessions.md`

- **Progressive overload:** increase exactly one training variable at a time — load, volume, density, or intensity — by roughly 5–10% per week. Larger jumps are flagged as injury risk.
- **Periodization model:** linear/block hybrid.
  - *Microcycle* = the 7-day sprint (`routine/weekly/`).
  - *Mesocycle* = 3–4 week block with one focus (base / build / peak), recorded in `pillars/training/periodization.md` "Current Block".
  - *Deload* = reduced volume (~40–50%) every 4th week or when fatigue markers rise.
- **Hard floors (already enforced in `periodization.md`):** ≥ 2 rest days/week; sleep debt > 2 h → swap intense session for mobility.
- **Intensity is measured in RPE 1–10** on every `sessions.md` entry. Session quality is judged on technique + RPE, never on exhaustion.
- **Technique before load:** form cues mastered at low intensity precede any load/speed progression, especially pre-puberty (§6).
- Canon: ACSM progression models; Bompa-style periodization; RPE-based autoregulation (Borg/Zourdos).

## 2. Sleep — Duration by Age

Cited by: `@sleep-coach`, `@sleep-guardian`, `@daily-routine`, `@safety-guardian` · State: `pillars/sleep/routine.md`, `night-log.md`, `metrics.md`

Target nightly duration (24-h total incl. naps where applicable), per AASM/NSF/CDC consensus ranges:

| Age | Recommended | Floor (never schedule below) |
|---|---|---|
| 4–12 mo | 12–16 h | 12 h |
| 1–2 y | 11–14 h | 11 h |
| 3–5 y | 10–13 h | 10 h |
| 6–12 y | 9–12 h | 9 h |
| 13–18 y | 8–10 h | 8 h |
| 18–60 y | 7–9 h | 7 h |
| 61–64 y | 7–9 h | 7 h |
| 65+ y | 7–8 h | 7 h |

- **Consistency beats duration-first optimization:** bedtime/wake within ±30 min of the `routine.md` window is the adherence metric (`docs/KPI.md` §1).
- **Sleep debt** = target − actual, summed nightly in `metrics.md` "Total sleep debt". Recovery priority: extend sleep before adding any load.
- **Wind-down:** 60 min pre-bed — no screens, dim light, no intense exercise or heavy food (mirrors `pillars/sleep/routine.md` checklist).
- **Environment:** dark, cool (~18–20 °C), quiet.
- Below-floor scheduling is a **human approval gate** (guardrail 1) — never an agent decision.
- Canon: AASM/SRS pediatric & adult consensus statements; NSF recommendations; Walker (popular synthesis).

## 3. Learning — Spaced Repetition & Retrieval Practice

Cited by: `@domain-mentor`, `@assessment-engine`, `@knowledge-librarian`, all knowledge-domain sessions · State: `domains/<slug>/curriculum.md`, `sessions.md`, `roadmap.md`

- **Retrieval practice > re-reading:** every session includes recall from memory (questions, blank-page recall, teach-back) before any re-exposure. Testing is learning, not just measurement.
- **Spaced repetition default intervals:** review at +1 d, +3 d, +7 d, +16 d, +35 d; `@schedule-manager` books these as micro-blocks. Intervals stretch on success, reset on failure (Leitner rule).
- **Interleaving:** mix related problem types within a session once L2+ is reached; block practice is for first exposure only.
- **Elaboration & dual coding:** explain-why questions and sketches/diagrams accompany verbal material.
- **Session sizing:** 25–50 min focused blocks; children start 15–25 min. Massed cramming is flagged in `metrics.md` trends, not celebrated.
- Canon: Dunlosky et al. (2013) effective-techniques review; Cepeda et al. spacing meta-analysis; Roediger & Karpicke retrieval-practice studies; Leitner/Anki-style scheduling.

## 4. Deliberate Practice

Cited by: every mentor and coach; `@assessment-engine` rubrics · State: all `domains/<slug>/sessions.md`

Four required properties — a session lacking any is logged but not counted toward level-promotion evidence:

1. **Well-defined sub-goal** for the session (recorded in the `Next:` line of the previous session).
2. **Full concentration** — single-tasking; distractions noted in session notes count against quality.
3. **Immediate, specific feedback** — coach/mentor notes or self-check vs rubric in every session entry.
4. **Edge of competence** — task difficulty just beyond current reliable ability (target ~70–85% success rate on drills).

- Mental representations compound: later levels reuse earlier representations; curriculum sequences are built accordingly in `curriculum.md`.
- Hours logged ≠ deliberate practice hours; `metrics.md` distinguishes session count from assessment-verified progress.
- Canon: Ericsson et al., *Peak*; adapted to non-elite daily practice doses.

## 5. Nutrition Fundamentals

Cited by: `@nutritionist`, `@daily-routine` · State: `pillars/nutrition/meal-plan.md`, `log.md`, `metrics.md`

> Verify-with-professional disclaimer: values below are generic population defaults.
> Allergies, medical conditions, dietary restrictions, and any therapeutic diet
> are **human-gated** (`learner/health.md`, guardrail 1) and must be confirmed
> with a registered dietitian/physician. `@nutritionist` plans inside approved
> constraints only.

- **Energy balance:** match intake to activity load; training days may add ~10–20% carbs around sessions. Growth phases (children/teens) never run deficits without medical direction.
- **Protein (RDA-based defaults):**

| Group | g/kg/day |
|---|---|
| 1–3 y | ~1.05 |
| 4–13 y | ~0.95 |
| 14–18 y | ~0.85 |
| Adult sedentary | ≥ 0.8 |
| Adult training | 1.2–2.0 |

- **Fat:** ~20–35% of energy; children under 3 higher — prioritize unsaturated sources.
- **Carbohydrate:** remainder of energy (~45–65%); scale to training volume.
- **Hydration:** baseline ~30–35 mL/kg/day for adults; more with heat/sessions. Log "hydration target met" in `nutrition/log.md`; urine-color check as practical field test.
- **Micronutrients:** coverage via dietary variety (colors across the week); never supplement claims without `human-consult`.
- **Meal timing:** protein across 3–5 eating occasions; pre-session light carbs, post-session protein + carbs within ~2 h.
- Canon: WHO/FAO nutrient requirements; ISSN protein & exercise position stand; national RDA tables. Adjustments for Bangladeshi staple patterns (rice, lentils, fish) are set in `meal-plan.md`.

## 6. Age-Appropriateness by Domain Category

Cited by: `@orchestrator`, `@domain-mentor`, `@safety-guardian`, `@interest-scout` · Age source: `learner/profile.md`

| Category (`domains/INDEX.md`) | Scaling rule | Hard gates |
|---|---|---|
| physical | technique-first; volume/intensity scaled to maturation; play-based under ~12 y | no maximal/external-load lifting before post-puberty without human gate + clearance; sleep debt blocks intensity (§7) |
| physical (age-gated) — `driving` | theory/simulation allowed any age | practical driving only at legal age for the learner's jurisdiction AND with a licensed human instructor — double gate, non-negotiable |
| expression (language, literature, drawing, behavior) | depth scaffolds to reading level | content filtered for age |
| sciences | concrete→abstract progression; hands-on demos first under ~12 y | lab-adjacent activities require adult supervision note in session plan |
| world (geography, geopolitics, law) | factual → systems → contested-topics maturity ladder | contested/violent topics framed age-appropriately |
| mind (brain-training, strategy) | games/puzzles at any age; decision frameworks teen+ | none beyond content filter |
| practical (agriculture, engineering) | tool use under direct adult supervision | powered tools/sharp tools: adult-supervised, human-gated |
| new domains (`interest-scout`) | category assigned at proposal; defaults to content-filtered | reviewer assigns any extra gates before activation |

Rule: when age is unknown, default to the conservative (younger) tier until `learner/profile.md` is filled.

## 7. Safety-Guardian Veto Criteria

`@safety-guardian` may veto any `@training-coordinator`, domain-coach, or `@daily-routine` plan. A veto writes a flag to `STATE.md` Safety Flags + `LOG.md`, then routes `## NEXT: @training-coordinator` (plan adjust) or `## NEXT: human-consult` (health).

### Hard vetoes (automatic, no discretion)

| Trigger | Threshold | Required action |
|---|---|---|
| Acute sleep debt | last night < floor (§2) or debt > 2 h | intense session → mobility/recovery only |
| Cumulative sleep debt | > 5 h over trailing 3 days | block all intense training until repaid |
| Rest-day deficit | < 2 rest days in trailing 7 | insert mandatory rest day |
| Consecutive intensity | > 3 consecutive high-RPE (≥7) days | next day capped at RPE ≤ 4 |
| Pain stop-signals | sharp pain; pain ≥ 4/10 during session; pain that alters movement; pain persisting > 24–48 h | stop session; flag; human-consult if persists |
| Acute red flags | chest pain, dizziness/fainting, breathlessness out of proportion, suspected fracture/sprain | immediate stop; `## NEXT: human-consult` |
| Illness | any below-the-neck symptom (fever, chest cough, vomiting, body aches) | full rest; above-neck mild symptoms → optional light mobility only |
| Volume spike | week-over-week load jump > 10% without justification | reject plan, require revision |
| Age gate breach | any plan violating §6 | reject plan; flag for human review |
| Health-flag breach | plan conflicts with `learner/health.md` | reject; human approval required to proceed |

### Soft flags (warning, plan proceeds only if revised downward)

- Rising avg session RPE with flat/falling performance over ≥ 5 sessions → prescribe deload.
- Missed sleep-window rate < 90% for two consecutive weeks → escalate to `@sleep-coach` + report.
- Monotonic decline in a domain's session quality notes → suggest elective break (`interest-scout`).

### Wellbeing override

Burnout signals (logged fatigue, dread notes, falling quality across multiple domains) authorize `@safety-guardian` to declare **protected rest**: all streaks preserved (`docs/KPI.md` §3), all plans reduced to recovery floor. Streak protection here is deliberate — guardrail 7 ranks wellbeing over metrics.

## 8. Citation Convention

- When a prescription relies on this document, append `(per docs/REFERENCES.md §N)` in the state file entry.
- If evidence or the learner's `health.md` conflicts with a default here, the agent logs the conflict in `LOG.md` and defers to the human — defaults never silently win.
- This file stores method, not curriculum: level-by-level content lives in each `domains/<slug>/curriculum.md`.
