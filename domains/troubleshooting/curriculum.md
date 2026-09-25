# Troubleshooting Curriculum

> Maintained by `@domain-mentor`. The full L0→L10 path; each level lists skills, theory, and the assessment that unlocks the next level. Scope: the universal fault-finding method — reproduce the error, isolate variables, find root cause, fix-verify-document — applied across devices, code, systems, processes, and plans. `coding/` owns programming depth; this domain owns the METHOD that transfers everywhere. Physical device work is supervised and age-gated per `learner/profile.md`.

## L0 — Foundations: Symptom vs Cause
- Skills: write a problem definition — what's expected, what's observed, when it started, what changed.
- Theory: the symptom is what you see, the cause is what makes it; "it's broken" is not a problem statement; most failed fixes come from solving the wrong problem.
- Assessment: 5 broken-thing scenarios each restated as a proper problem definition (expected / observed / since / changed).

## L1 — Reproduce the Error
- Skills: turn a vague or intermittent fault into repeatable steps; find the minimal trigger; document steps another person could follow.
- Theory: a fault you can't reproduce is a fault you can't verify fixed; "it happens sometimes" means the trigger isn't found yet; reproduction is the first real diagnostic.
- Assessment: 3 faults reproduced with written steps — at least one previously "random" fault reliably triggered by found conditions.

## L2 — Isolating Variables
- Skills: change one thing at a time; design a works-vs-doesn't comparison; build a control.
- Theory: two simultaneous changes = zero information; every test must answer one question; a test that can't change your mind wasn't a test.
- Assessment: 5 isolation exercises — each names the variable, predicts the outcome BEFORE running, records the result.

## L3 — Bisection
- Skills: halve the search space — test the midpoint, not the edges; apply to sequences, timelines, settings chains.
- Theory: bisection is the fastest general-purpose isolation; "which half is the bug in?" is worth more than "what might the bug be?"; works on anything ordered.
- Assessment: 3 bisection runs (ordered-sequence fault, timeline fault — "worked last week", config/settings chain) — each solved in ≤ log2(n) + 2 tests, steps logged.

## L4 — Root-Cause Analysis (5-Whys)
- Skills: run 5-whys past the first plausible cause; build a simple cause map — contributing factors, not just the culprit.
- Theory: the first cause found is usually a trigger, not the root; "human error" is a lazy root cause — ask why the system allowed it; fixing the symptom means the fault returns in disguise.
- Assessment: 3 root-cause write-ups — each reaches a cause that, if fixed, prevents recurrence; mentor challenges "why is that the root?" and the learner defends or digs further.

## L5 — Intermediate milestone: Fix → Verify → Document
- Skills: run the full loop — hypothesize, fix, verify with the SAME reproduction steps, confirm no new faults, document for next time.
- Theory: a fix isn't done until the original reproduction steps pass clean; "seems fine now" is not verification; the write-up makes the fix transferable.
- Assessment: 3 complete fault reports — problem definition, reproduction steps, isolation log, root cause, fix, verification result, prevention note.

## L6 — Debugging Devices & Physical Things
- Skills: troubleshoot household devices, connections, mechanical basics — power first, connections second, parts third.
- Theory: check the cheap/likely things first (is it plugged in? is it switched on?); most physical faults are power, connection, or wear. Supervised only — unplug before opening, no mains-voltage work, tools age-gated.
- Assessment: 3 real or staged device faults fixed and documented under supervision — each with the order of checks justified.

## L7 — Debugging Code & Software
- Skills: read an error message top-to-bottom; reduce to a minimal reproducer; use logs, print/breakpoint bisection, rubber-duck the logic.
- Theory: the error message is a clue, not an insult — read the whole stack; minimal reproduction removes noise; syntax depth belongs to `coding/` — the method here.
- Assessment: 3 software bugs isolated to minimal reproducers — each with error read, bisection log, root cause stated.

## L8 — Debugging Systems, Processes & Plans
- Skills: apply the method to non-physical faults — a workflow that fails, a plan that keeps breaking, a process producing bad output.
- Theory: the method doesn't care about the medium — define, reproduce, isolate, root-cause, fix, verify; process faults hide in handoffs and assumptions.
- Assessment: 2 non-physical troubleshooting cases (failed process or broken plan) — full fault reports with the root at the process level, not "someone messed up."

## L9 — Complex & Intermittent Faults
- Skills: hunt faults that resist reproduction — timing-dependent, environment-dependent, multi-cause; build evidence logs over time; recognize when two faults masquerade as one.
- Theory: intermittent faults need observation windows and logging, not guesses; a fault cluster can have multiple roots — fix and re-verify separately; sometimes the correct output is "not yet reproducible — monitoring plan: X."
- Assessment: one long-running intermittent-fault case (real or simulated) managed across ≥1 week — logging protocol, hypothesis tree, outcome (fixed or bounded with a monitoring plan).

## L10 — Mastery: Cross-Domain Debugging Capstone
- Skills: deploy the full method across ≥3 domains in one portfolio — physical, software, process/plan.
- Theory: synthesis — the troubleshooter's creed: reproduce the error, isolate, find the root, verify the fix, write it down. The method IS the mastery.
- Assessment: capstone dossier — 3+ complete fault reports across ≥3 domains, each through the full loop; one fault chosen and seeded by the mentor; plus a written "method card" the learner could hand to a novice.
