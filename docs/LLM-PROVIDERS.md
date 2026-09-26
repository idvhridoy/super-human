# LLM Providers & Model-Selection Policy

How supporting LLM calls are configured and chosen. The agents themselves run on Devin CLI's built-in models — these keys power *supporting* calls (batch classification, content generation inside skills, future MCP/tool integrations).

## Configuration

- Keys live in `.env` (gitignored — **never commit, never log, never print**).
- `.env.example` is the committed template; copy → `.env` → fill only providers you use.
- A missing key is not an error: agents fall back to the default skill model. If a task genuinely requires a call and no key exists → `## NEXT: human-consult`.

## Supported providers

| Env var | Provider | Typical use |
|---|---|---|
| `OPENAI_API_KEY` | OpenAI | `gpt-4o-mini` (cheap classification) · `gpt-4o` (drafts) |
| `ANTHROPIC_API_KEY` | Anthropic | `claude-3-haiku` (cheap) · `claude-sonnet-4` (final outputs) |
| `GEMINI_API_KEY` | Google | `gemini-1.5-flash` (cheap) · `gemini-1.5-pro` (long context) |
| `GROK_API_KEY` | xAI | alternate reasoning/generation |
| `JEV_API_KEY` | JEV | owner-configured endpoint |
| `DEEPSEEK_API_KEY` | DeepSeek | low-cost strong reasoning/coding |
| `OPENROUTER_API_KEY` | OpenRouter | one key → many models (fallback aggregator) |
| `GROQ_API_KEY` | Groq | ultra-fast small-model calls |
| `TOGETHER_API_KEY` | Together | open-weight models |
| `MISTRAL_API_KEY` | Mistral | EU-hosted alternate |
| `COHERE_API_KEY` | Cohere | embeddings/rerank for `knowledge-librarian` |
| `PERPLEXITY_API_KEY` | Perplexity | grounded web answers for research tasks |

## Model tiers (cheapest capable first)

| Tier | Use for | Example models |
|---|---|---|
| **T1 — fast/cheap** | classification, tagging, streak/streak-rule checks, log summarization | `gpt-4o-mini`, `claude-3-haiku`, `gemini-1.5-flash`, `deepseek-chat` |
| **T2 — capable** | lessons, session plans, curriculum drafting, drill generation | `claude-sonnet-4`, `gpt-4o`, `deepseek-reasoner`, `gemini-1.5-pro` |
| **T3 — strongest** | long-horizon strategy (`strategy-advisor`), final curricula, safety-sensitive review text | best available per `devin models` |

## Rules for agents

1. Default to T1; escalate only when the output is learner-facing long-form or safety-relevant.
2. Prefer the provider whose key exists; if several exist, prefer cheapest capable.
3. Never write key values into any file, log, or `STATE.md`.
4. Rate-limit courtesy: batch items per call; no polling loops.
5. Any paid-tool purchase/subscription decision → `[HUMAN]` gate in `STATE.md`.
