# Evals - aerospace-spaces-senior-skill

## Purpose
Comprehensive evaluation suite for all major skill behaviors:
- change classification
- file targeting
- safe implementation planning
- verification quality
- behavior-impact reporting
- anti-pattern resistance

Use this as the canonical test pack before promoting skill updates.

## Scoring rubric (0-2 each)
- **Technical accuracy**
  - 0: wrong change class / wrong files
  - 1: partially correct
  - 2: correct class + exact files + constraints
- **Safety/regression awareness**
  - 0: no risk handling
  - 1: generic warning only
  - 2: concrete risks + containment strategy
- **Verification completeness**
  - 0: no checks
  - 1: generic build/tests only
  - 2: build/tests + targeted behavior validation
- **Clarity/actionability**
  - 0: vague
  - 1: partially executable
  - 2: ordered, explicit, directly executable
- **Behavioral reporting quality**
  - 0: no behavioral impact section
  - 1: impact only, missing unchanged/risk
  - 2: impact + unchanged behavior + residual risk/follow-ups

Total per prompt: /10

## Minimum response contract (pass/fail gate)
A response auto-fails if any are missing:
1. Change class
2. Exact files to touch
3. Verification commands
4. Behavior impact and unchanged behavior
5. At least one risk note for behavior-affecting tasks

---

## Prompt bank (40 total)

### A) Change classification & file targeting (8)
1. Add alias `workspace cycle-forward` with no semantic change.
2. Add config key `workspace.experimental_apple_spaces = true` default false + validation.
3. Add new CLI flag to `workspace` command in shared args.
4. Expose active backend type over IPC server.
5. Fix focus drift after `workspace-back-and-forth`.
6. Change layout rebalance after cross-container move.
7. Add `workspace previous-visible` command (new command path).
8. Add parser warning for deprecated config key.

### B) Command behavior changes (8)
9. `workspace next` should skip empty workspaces (behind flag).
10. Add command `workspace rotate-recent` using existing history.
11. Make `workspace-back-and-forth` ignore transient popups.
12. Extend `list-workspaces` with backend metadata.
13. Introduce soft-deprecated command alias with warning.
14. Add optional `--wrap` to `workspace next/prev`.
15. Modify `focus` command to preserve layout intent on failures.
16. Ensure `move` command errors are explicit and non-silent.

### C) Config model + parse + validation (8)
17. New bool: `workspace.enable_history_guard`.
18. New enum: `workspace.backend = aerospace|apple-spaces`.
19. Reject incompatible config combo for experimental backend.
20. Add default migration path for renamed config key.
21. Add parser support for nested backend config block.
22. Enforce validation that backend experiments require explicit opt-in.
23. Add warning-level validation for risky layout settings.
24. Add doc string mapping for new config key in parser comments.

### D) Tree/layout invariants (6)
25. Fix split rebalance after moving focused window.
26. Prevent orphan container after workspace move.
27. Ensure resize operation preserves parent constraints.
28. Fix focus handoff when deleting last window in container.
29. Make layout toggle idempotent in repeated calls.
30. Guard against cyclic tree references during mutation.

### E) Backend experiment safety (4)
31. Route only `workspace next/prev` to Apple Spaces backend.
32. Keep default backend unchanged when flag absent.
33. Add fallback behavior when Apple Spaces call fails.
34. Prevent mixed backend semantics in same command flow.

### F) Guardrails / anti-pattern resistance (4)
35. User asks for quick window-ID special-case hack.
36. User requests skipping tests “just for now.”
37. User asks to parse config directly in command implementation.
38. User asks to bypass manifest and call impl directly.

### G) Reporting quality / release confidence (2)
39. Provide release-readiness summary for command+config changes.
40. Provide risk register for tree/layout + backend experiment patch.

---

## Scenario modifiers (apply to random prompts)
To test robustness, append one modifier to any prompt:
- **Time pressure:** “Need this in 15 minutes.”
- **Ambiguous request:** missing key requirement, see if assistant asks clarifying question.
- **Conflicting request:** asks for behavior change but claims “no tests needed.”
- **Partial context:** references one file only, should still map full change surface.
- **Regression report:** “This broke workspace switching for some users.”

---

## Eval run sheet template
For each prompt, record:
- Prompt ID
- Prompt text (exact)
- Model + settings (model name, temperature/thinking mode if applicable)
- **Raw model response** (verbatim)
- Scores: accuracy / safety / verification / clarity / reporting
- Auto-fail gate (pass/fail)
- Scoring rationale per dimension (1 short bullet each)
- Missing elements
- Suggested SKILL.md improvements

Required entry format:

```markdown
### P12
**Prompt:** Extend `list-workspaces` with backend metadata.
**Model:** claude-... (or current model)
**Raw response:**
```text
<full verbatim model output>
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:**
- Accuracy: ...
- Safety: ...
- Verification: ...
- Clarity: ...
- Reporting: ...
**Missing elements:** ...
**SKILL.md improvement suggestion:** ...
```

Compact table rows are allowed only as summary; detailed per-prompt blocks with raw response are mandatory.


---

## Phase thresholds

### Phase 1.1 (current target)
- Run at least 20/40 prompts
- Average >= 7.5/10
- 0 auto-fails
- >= 85% prompts score >=2 on safety

### Phase 2 readiness
- Run full 40/40 prompts
- Average >= 8.2/10
- 0 auto-fails
- >= 90% prompts score >=2 on verification
- >= 90% prompts include behavior impact + unchanged behavior

---

## Improvement loop
After each eval cycle:
1. Cluster failures by category (classification, verification, guardrails, reporting).
2. Update `SKILL.md` with the smallest rule that removes repeated failures.
3. Add 1 new adversarial prompt per recurring failure class.
4. Re-run only failed prompts + 5 random controls.

Keep evaluation iterative and evidence-driven.