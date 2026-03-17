# Eval Run - 2026-03-17 (First 20 prompts, strict re-score)

Scope: P1-P20 from `skill/references/evals.md`
Method: stricter scoring than baseline, with heavier penalties for generic verification and missing negative-path tests.

Scoring dimensions:
- A = Technical accuracy
- S = Safety/regression awareness
- V = Verification completeness
- C = Clarity/actionability
- R = Behavioral reporting quality

Total per prompt: /10

## Per-prompt strict scores

- P1: **8** = 2/2/1/2/1 — alias case accurate, but verification/reporting too generic.
- P2: **8** = 2/2/1/2/1 — config path right; missing explicit invalid-config negative test.
- P3: **7** = 2/2/1/1/1 — CLI-arg sync identified, but action plan not concrete enough.
- P4: **8** = 2/2/1/2/1 — backend scope good; fallback verification underspecified.
- P5: **7** = 2/1/1/2/1 — risk mention present but shallow for focus-history regressions.
- P6: **7** = 2/1/1/2/1 — layout-risk handling broad; no concrete invariant checks listed.
- P7: **8** = 2/2/1/2/1 — command path good; reporting lacks unchanged-behavior detail.
- P8: **7** = 2/2/0/2/1 — warning behavior not tied to specific parser snapshot/fixture tests.
- P9: **8** = 2/2/1/2/1 — behavior change clear; missing edge-case wraparound checks.
- P10: **7** = 2/1/1/2/1 — history command concept ok; regression surface underexplored.
- P11: **6** = 2/1/0/2/1 — popup filtering lacks explicit criteria + targeted negative tests.
- P12: **8** = 2/2/1/2/1 — metadata extension clear; CLI compatibility verification too generic.
- P13: **8** = 2/2/1/2/1 — deprecation approach solid; missing rollout/compat checks.
- P14: **8** = 2/2/1/2/1 — wrap flag behavior good; no no-wrap regression assertions.
- P15: **7** = 2/1/1/2/1 — failure intent discussed, but no explicit failure simulation commands.
- P16: **8** = 2/2/1/2/1 — non-silent errors good; test matrix too shallow.
- P17: **8** = 2/2/1/2/1 — bool config clean; validation tests insufficiently concrete.
- P18: **8** = 2/2/1/2/1 — enum mapping solid; migration/back-compat checks too light.
- P19: **7** = 2/2/0/2/1 — incompatible combo idea right; no strict negative-path execution.
- P20: **7** = 2/1/1/2/1 — migration framing ok; lacks explicit deprecation telemetry/checks.

## Corrected aggregate

- Prompts run: **20/40**
- Auto-fail gates: **0**
- Total score: **152/200**
- Average: **7.60/10**
- Safety score=2 coverage: **12/20 (60%)**
- Verification score=2 coverage: **0/20 (0%)**

## Why previous score was inflated

1. Verification was graded leniently (generic build/tests counted as near-complete).
2. Reporting quality was over-credited even when “unchanged behavior” was not explicit.
3. Negative-path testing was not enforced as a hard expectation for behavior changes.
4. Risk notes were accepted at high score even when not tied to concrete containment steps.
5. Distinction between “good plan” and “executable plan with exact commands” was blurred.

## 5 concrete SKILL.md improvements to increase rigor

1. Add a **mandatory negative-path verification** rule for all behavior-changing tasks.
2. Add a **command-specific verification matrix** (happy path + edge + regression pair).
3. Require explicit **unchanged-behavior bullets** (minimum 2) in every final report.
4. Add **risk-to-mitigation mapping** format: each risk must have one containment check.
5. Add a **no-generic-verification rule**: reject outputs that only state build/tests without targeted command flows.

## Verdict

- Current skill quality is promising but not yet “high-confidence strict”.
- A realistic baseline is **7.6/10**, not 9.55.
- Biggest gap: verification depth and adversarial/negative-path evidence.
