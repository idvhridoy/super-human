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

## Framework research (2026-01 survey)

Full open-source agent-framework scan. Verdict: **don't rebuild the harness — plug in, don't reinvent.**

| Framework | Stars/scale | What it is | Fit for super-human |
|---|---|---|---|
| **LiteLLM** | dominant router | Unified OpenAI-format API to 100+ providers + Router (retries, fallbacks, cooldowns, load-balance) + proxy with virtual keys, budgets, cost tracking | ✅ **CHOSEN for Phase 1** — our entire provider matrix (OpenAI/Anthropic/Gemini/Grok/DeepSeek/Groq/Together/Mistral/Cohere/Perplexity) is native; fallback chains + cost tracking are built-in, not hand-rolled |
| **smolagents** (HuggingFace) | ~5k LOC | Minimal agent lib: CodeAgent/ToolCallingAgent, any LLM via LiteLLM, MCP tools, built-in web search, sandboxed exec | ✅ Optional Lane-B micro-agents (e.g. interest-scout recon) — 10-line agents without a framework tax |
| **OpenHands** | ~60k | The closest open-source Devin equivalent: autonomous CLI/GUI agent, MCP-first tool dispatch, plan-first loop, any LLM | 🔶 Full harness if a Devin-free runtime is ever needed; heavy (Docker). Not needed while Devin CLI exists |
| **Goose** | ~38k | Rust-native general-purpose local agent: CLI + desktop + API, MCP extensions, multi-model | 🔶 Lighter alternative to OpenHands for a self-hosted harness |
| **OpenAI Agents SDK** | large | Provider-agnostic multi-agent: handoffs, guardrails, MCP, voice pipelines — mirrors our skill/handoff design | 🔶 Reference architecture; overkill vs Markdown skills |
| **CrewAI** | ~22k | Role-based crews — philosophically nearest to our 33-skill roster | ❌ Redundant — our skill system already does role routing; Python-bound state |
| **LangChain/LangGraph** | ~85k | Maturest ecosystem: graphs, checkpoints, 500+ integrations | ❌ Heavy learning curve; power we don't need for a Markdown OS |
| **AutoGen** | ~35k | Microsoft multi-agent research framework | ❌ Merged into MS Agent Framework / maintenance mode — skip new builds |
| **aider / OpenCode / Pi** | 46–172k | Terminal coding agents; OpenCode = 75+ providers + MCP | ❌ Coding-focused; we're a life-orchestration OS, not a coding tool |

### Decision

- **`tools/llm.py` = thin wrapper over LiteLLM Router** — one `completion()` call shape for every provider, `.env` keys auto-detected, fallback chains + budgets + cost logs are framework features (not our code).
- **Devin stays the harness** — OpenHands/Goose are held in reserve; building a parallel harness duplicates what Devin CLI already provides.
- **smolagents reserved** for the rare case a Lane-B task needs a mini agent loop (MCP tools) rather than a single call.

## Implementation phases (todo)

### Phase 1 — Router foundation (blocking)
- [ ] `pip install litellm` + `requirements.txt` (pin a version ≥7 days old)
- [ ] `tools/llm.py` — entrypoint: `--task-class <fast|strong|vision|audio|embed|web> --prompt @file --out <file>`; wraps `litellm.Router` with model-groups per task-class from `.env`-detected keys; appends usage rows (provider, model, tokens, est. cost — never prompts containing private data) to `logs/llm-usage.md`
- [ ] `tools/llm-config.yaml` — task-class → ordered model list + fallback chains + per-class token caps
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
