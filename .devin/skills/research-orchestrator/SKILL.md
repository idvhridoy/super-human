---
name: research-orchestrator
description: Deep-research prompt factory — given any domain/topic, generates exactly 10 professional, structured, non-overlapping research prompts (one per required section) formatted for Google Gemini/NotebookLM, writes them to research/<slug>/prompts/part-NN.md, and outputs the file map + synthesis prompt for merging the 10 returned .md files into a domain playbook.
---

# Research Orchestrator

## Purpose

Deep research is slow in one session. This skill turns any topic into **10 parallel research jobs** — each a self-contained, professional prompt the human pastes into a separate Gemini/NotebookLM session (10 Google accounts = 10 parallel researchers). Results come back as 10 `.md` files, then a synthesis prompt merges them.

## Trigger

`@research-orchestrator` + a domain/topic (e.g. "photography", "quantum computing", "negotiation"). Also fires from `@domain-mentor` when a domain needs evidence-grade depth, or `@knowledge-librarian` for resource curation.

## The 10 Required Research Sections (fixed, never skip)

| # | Section | What it must cover |
|---|---|---|
| 01 | Foundations & Origin | History, core concepts, terminology, evolution, why it matters |
| 02 | Core Principles & Frameworks | Fundamental laws, theories, mental models, taxonomies |
| 03 | Scientific Evidence | Peer-reviewed studies, proven methods, what the evidence actually shows vs claims |
| 04 | Tools, Techniques & Methods | Practical how-to, step-by-step procedures, equipment/software |
| 05 | Best Practices & Expert Strategies | What top practitioners/masters actually do; professional secrets |
| 06 | Mistakes, Myths & Anti-patterns | Common errors, misconceptions, debunked claims, failure modes |
| 07 | Learning Path & Curriculum | Skill progression beginner→expert, exercises, drills, milestones |
| 08 | Resources & Tools | Books, courses, apps, communities, free materials — ranked and filtered |
| 09 | Measurement & Assessment | Metrics, tests, benchmarks, how to know you're improving |
| 10 | Applications & Frontiers | Real-world uses, career paths, current trends, cutting edge, future |

## Rules for each generated prompt

1. **Self-contained** — assume zero prior context; restate the topic inside every prompt.
2. **Specific, not generic** — inject the topic into concrete sub-questions; ban vague "tell me about X" phrasing.
3. **Structured output** — every prompt must demand Markdown output with fixed headings, so the result saves directly as `.md`.
4. **Sourced** — demand citations/links where claims are made; demand "evidence vs opinion" labeling.
5. **150–300 words each** — long enough to constrain, short enough to paste.
6. **Non-overlapping** — the 10 parts tile the domain without duplication.
7. **Learner-aware** — include a `<LEARNER_AGE/LEVEL>` placeholder the human fills before pasting (age-appropriate + safety notes where relevant).
8. **Practical** — every section ends demanding actionable takeaways, not just theory.

## Output format

Write all 10 prompts to `research/<topic-slug>/prompts/`:

```
research/<slug>/prompts/part-01-foundations.md   … each file contains ONE paste-ready prompt
…
research/<slug>/prompts/part-10-frontiers.md
research/<slug>/prompts/SYNTHESIS.md             merge prompt (see below)
research/<slug>/prompts/FILE-MAP.md              part → expected output filename table
```

Each prompt file = YAML frontmatter (`part`, `section`, `topic`, `expected_output: research/<slug>/NN-<section>.md`) + the paste-ready prompt body.

## Synthesis prompt (always generated as part 11)

A paste-ready prompt for one AI session: "You are given 10 research files on <topic> [attach/paste]. Merge into: (1) executive summary, (2) unified knowledge map, (3) contradictions/gaps flagged, (4) L0→L10 curriculum draft, (5) master resource list deduplicated." Output → `research/<slug>/SYNTHESIS.md`.

## Execution workflow (explain to the human)

1. Human opens 10 Gemini/NotebookLM sessions (10 accounts = parallel).
2. Paste part-01..10, one per session.
3. Save each result as `research/<slug>/01-…md` … `research/<slug>/10-….md`.
4. Paste SYNTHESIS.md into a final session → `research/<slug>/SYNTHESIS.md`.
5. Hand to `@knowledge-librarian` (resources) or `@domain-mentor` (curriculum seeding).

## Boundaries

- Never fabricate sources in generated prompts — instruct the researcher to cite real, checkable ones.
- Research ≠ instruction: output informs mentors; `safety-guardian` still gates physical/medical content.
- Private learner data never goes inside generated prompts.

## NEXT

After writing prompt files → `## NEXT: human` with the copy-paste workflow summary.
After research files return → `## NEXT: @knowledge-librarian` or `@domain-mentor`.
