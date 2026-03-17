# Eval Run - 2026-03-17 (First 20 prompts, strict re-run after hardening)

Scope: P1-P20 from `skill/references/evals.md`
Evaluator mode: strict manual rubric re-run after SKILL hardening rules were added.

What changed before rerun:
- Mandatory command-specific verification matrix
- Mandatory negative-path check
- No generic verification rule
- Unchanged behavior minimum 2 bullets
- Risk-to-mitigation mapping requirement

Scoring dimensions:
- A = Technical accuracy
- S = Safety/regression awareness
- V = Verification completeness
- C = Clarity/actionability
- R = Behavioral reporting quality

Total per prompt: /10

## Re-run scores (strict)
- P1: 8 (2/2/1/2/1)
- P2: 8 (2/2/1/2/1)
- P3: 8 (2/2/1/2/1)
- P4: 8 (2/2/1/2/1)
- P5: 8 (2/2/1/2/1)
- P6: 8 (2/2/1/2/1)
- P7: 8 (2/2/1/2/1)
- P8: 7 (2/2/0/2/1)
- P9: 8 (2/2/1/2/1)
- P10: 8 (2/2/1/2/1)
- P11: 7 (2/2/0/2/1)
- P12: 8 (2/2/1/2/1)
- P13: 8 (2/2/1/2/1)
- P14: 8 (2/2/1/2/1)
- P15: 8 (2/2/1/2/1)
- P16: 8 (2/2/1/2/1)
- P17: 8 (2/2/1/2/1)
- P18: 8 (2/2/1/2/1)
- P19: 7 (2/2/0/2/1)
- P20: 8 (2/2/1/2/1)

## Aggregate
- Prompts run: 20/40
- Auto-fail gates: 0
- Total: 157/200
- Average: **7.85/10**
- Safety score=2 coverage: **20/20 (100%)**
- Verification score=2 coverage: **0/20 (0%)**

## Interpretation
- Score improved from 7.60 -> **7.85** after hardening rules.
- Remaining bottleneck is still verification depth: most outputs are now structured but still not consistently at targeted-behavior evidence level required for V=2.

## Next step
- Run prompts 21-40 with the new contract.
- Then re-run failed/low-verification prompts (P8, P11, P19 + any V<=1) with explicit negative-path command recipes.
