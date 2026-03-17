# Eval Run - 2026-03-17 (First 20 prompts)

Scope: prompts P1-P20 from `skill/references/evals.md`
Evaluator mode: manual rubric scoring against current `skill/SKILL.md`

Scoring dimensions:
- A = Technical accuracy
- S = Safety/regression awareness
- V = Verification completeness
- C = Clarity/actionability
- R = Behavioral reporting quality

Total per prompt: /10

## Results

- P1: 2/2/2/2/2 = **10** (pass)
- P2: 2/2/2/2/2 = **10** (pass)
- P3: 2/2/1/2/2 = **9** (pass) — verification could include explicit CLI arg roundtrip check
- P4: 2/2/2/2/2 = **10** (pass)
- P5: 2/2/1/2/2 = **9** (pass) — missing targeted regression test for focus history
- P6: 2/2/1/2/2 = **9** (pass) — layout behavior checks need more concrete manual flow
- P7: 2/2/2/2/2 = **10** (pass)
- P8: 2/2/1/2/2 = **9** (pass) — parser warning snapshot test not always specified
- P9: 2/2/2/2/2 = **10** (pass)
- P10: 2/2/1/2/2 = **9** (pass) — history edge-case verification missing
- P11: 2/2/1/2/2 = **9** (pass) — popup classification criteria needs explicit test cases
- P12: 2/2/2/2/2 = **10** (pass)
- P13: 2/2/2/2/2 = **10** (pass)
- P14: 2/2/2/2/2 = **10** (pass)
- P15: 2/2/1/2/2 = **9** (pass) — failure-mode simulation not always included
- P16: 2/2/2/2/2 = **10** (pass)
- P17: 2/2/2/2/2 = **10** (pass)
- P18: 2/2/2/2/2 = **10** (pass)
- P19: 2/2/2/2/2 = **10** (pass)
- P20: 2/2/1/2/2 = **9** (pass) — migration verification can be more explicit

## Aggregate

- Prompts run: **20/40**
- Auto-fail gates: **0**
- Total score: **191/200**
- Average: **9.55/10**
- Safety score=2 coverage: **20/20 (100%)**
- Verification score=2 coverage: **13/20 (65%)**

## Phase 1.1 threshold check

- Run >= 20 prompts: ✅
- Average >= 7.5: ✅
- 0 auto-fails: ✅
- >=85% score=2 on safety: ✅

Status: **Phase 1.1 passes**, with verification depth as the clear improvement area.

## Improvement actions before prompts 21-40

1. Add command-specific verification checklist section to SKILL:
   - command semantics checks
   - parser/args consistency checks
   - backend fallback checks
2. Require one “negative path” verification for behavior-changing prompts.
3. Add explicit migration validation checklist for renamed/deprecated config keys.
