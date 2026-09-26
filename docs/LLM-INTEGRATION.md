# LLM Integration Roadmap — Multi-Modal, Multi-Thread, Parallel Assist Layer

## Strategy: Devin-First, API-Assist

```
Request → Devin CLI skill (primary engine, always first)
              │
              ├─ Succeeds → done
              │
              └─ Limit hit / difficulty / bulk batch work
                     │
                     ▼
              tools/llm router (exec) → provider chain:
                1. cheapest capable key present in .env
                   (deepseek / gemini-flash / gpt-4o-mini / groq)
                2. strong tier for learner-facing output
                   (sonnet-4 / gpt-4o / gemini-pro / grok)
                3. OPENROUTER_API_KEY as universal fallback
                     │
                     ▼
              Output written back to the same state file the
              skill owns — indistinguishable to downstream agents
```

**Rule #1:** Devin CLI always gets first attempt. External APIs are an assist layer — never a bypass. They fire when: (a) a Devin call hits a limit/fails, (b) the task is bulk mechanical work below a skill's judgment threshold, or (c) a capability Devin doesn't have is needed (vision, TTS, embeddings).

## Capability lanes (multi-modal)

| Lane | Provider fit | Who uses it | Purpose |
|---|---|---|---|
| **Text-fast** | deepseek-chat, gpt-4o-mini, haiku, gemini-flash, groq | all skills | classification, tagging, log summarization, tick-derivation |
| **Text-strong** | sonnet-4, gpt-4o, gemini-1.5-pro, grok, deepseek-reasoner | domain-mentor, coaches, strategy-advisor | curricula, lesson drafts, capstone designs |
| **Vision** | gpt-4o, gemini-1.5-pro | run/strength-coach, drawing mentor | form-check photos, technique video stills, artwork review |
| **Audio/TTS** | elevenlabs, openai-tts | communication-coach, language mentor | speech drills, pronunciation models, voice feedback |
| **Embeddings** | cohere, openai | knowledge-librarian | semantic resource search across library/ + 53 resources.md files |
| **Grounded web** | perplexity, tavily | interest-scout, knowledge-librarian | fresh resources, sky-event dates, domain news |

## Parallel & multi-thread model

Extends the existing wave model (`docs/ORCHESTRATION.md`):

- **Wave lane A (Devin):** skills run as parallel subagents — unchanged, disjoint file ownership.
- **Wave lane B (API workers):** inside a skill, bulk fan-out work goes to `tools/llm` in parallel subprocesses — e.g., `domain-mentor` regenerating 10 lesson variants, `knowledge-librarian` re-curating 53 resources.md files, `checklist-review` summarizing a week of logs.
- **Merge rule:** Lane B never writes state directly to final files — it produces drafts/results that the owning skill reviews and commits. Skill = judgment, API = muscle.
- **Concurrency caps:** max 5 parallel API calls per skill run; total daily budget cap recorded in STATE.md (cost guardrail).

## Implementation phases (todo)

### Phase 1 — Router foundation (blocking)
- [ ] `tools/llm.py` — single entrypoint: `--task-class <fast|strong|vision|audio|embed|web> --prompt @file --out <file>`; reads `.env`; provider chain with retries; token/cost log appended to `logs/llm-usage.md` (aggregate numbers only — never prompts containing private data)
- [ ] `tools/llm.sh` — thin curl fallback for envs without python
- [ ] Failure taxonomy: `devin_limit`, `api_error`, `no_key`, `budget_cap` → each has a defined next step
- [ ] Test: one real call per configured provider; results logged in `logs/llm-usage.md`

### Phase 2 — Wire the highest-value skills
- [ ] `domain-mentor` — lesson/drill generation via text-strong lane
- [ ] `knowledge-librarian` — embeddings lane for resource search; grounded-web for freshness
- [ ] `checklist-review` — week-log summarization via text-fast
- [ ] `progress-report` — metric aggregation assist via text-fast
- [ ] `assessment-engine` — quiz generation via text-strong

### Phase 3 — Multi-modal lanes
- [ ] `communication-coach` — TTS models + speech-feedback loop (audio lane)
- [ ] `run-coach`/`strength-coach` — form-check stills via vision lane (human uploads photo)
- [ ] `language` mentor — pronunciation audio via TTS
- [ ] `interest-scout` — grounded web lane for domain signals

### Phase 4 — Resilience hardening
- [ ] `orchestrator` health-check routine: probe provider availability before waves
- [ ] Provider failover tests (simulate missing keys)
- [ ] Budget report section in weekly-review (cost/domain)
- [ ] `docs/AUTOMATION.md` update — API-assisted vs Devin-only table

## Guardrails (unchanged, extended)

1. Keys never leave `.env`; logs record provider + token count only, never key or private prompts.
2. API calls containing `learner/private.md` data are **forbidden** — private.md never crosses the wire.
3. Paid-usage budget is a `[HUMAN]`-gated decision; STATE.md tracks a daily cap.
4. Age-appropriate content rules apply to API output identically — same safety-guardian review.
5. If all providers fail → degrade gracefully: skill continues Devin-only or queues the item.

## Success criteria

- [ ] Any skill run survives Devin rate-limits without manual intervention
- [ ] Bulk tasks (10+ item generation) complete via Lane B without blocking the skill
- [ ] `logs/llm-usage.md` shows provider, tier, tokens, cost estimate per call
- [ ] Zero key leakage detectable in committed files (`grep -r "sk-\|AIza" .` clean)

## Status

- Phase: **0 — strategy approved** (this document)
- Next: Phase 1, `tools/llm.py` — assign to a build session
