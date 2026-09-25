# Troubleshooting Playbook

> Do/don't list maintained by `@domain-mentor` from session evidence.

## Do
- Write the problem definition first — expected vs observed vs since-when vs what-changed
- Reproduce the error before touching anything — if you can't trigger it, you can't verify the fix
- Change ONE variable at a time — and predict the outcome before you run the test
- Bisect when the search space is ordered — test the midpoint, not your favorite suspect
- Ask "why" past the first cause — fix the root or the fault returns wearing a new costume
- Check the boring things first — power, connections, settings, recent changes
- Read the whole error message — the answer is usually in the part you skipped
- Verify the fix with the SAME reproduction steps that triggered the fault
- Re-test what you touched plus what you might have broken nearby
- Document every fix — the write-up turns one fix into permanent capability

## Don't
- Don't shotgun-fix — random changes until it works teaches nothing and hides the cause
- Don't fix the symptom and call it done — recurring faults are unrooted faults
- Don't skip reproduction on "obvious" faults — obvious-and-wrong is the classic trap
- Don't change two things and claim you learned something — you learned which pair, not which cause
- Don't declare victory on "seems fine now" — verify or it isn't fixed
- Don't accept "human error" as a root cause — ask why the system allowed the error
- Don't keep poking an unreproduced fault — switch to logging and a monitoring plan instead
- Don't open mains-powered devices or attempt repairs beyond supervision level — `## NEXT: human-consult`

## Safety notes
- Physical device rules: unplug before opening; no mains-voltage work; never handle damaged batteries or frayed cables; tools and repairs supervised and age-gated per `learner/profile.md`
- Electrical, structural, gas, or vehicle faults → qualified human always; the domain builds method, not technician credentials
- `@safety-guardian` may append vetoes
