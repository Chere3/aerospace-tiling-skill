# Evals - aerospace-spaces-senior-skill

## Purpose
Phase 1 baseline eval set for prompt/skill quality.

## Scoring rubric (0-2 each)
- **Technical accuracy**
  - 0: wrong files/change class
  - 1: partially correct
  - 2: correct class + files + constraints
- **Safety/regression awareness**
  - 0: no risk thinking
  - 1: generic warning
  - 2: concrete risks and containment
- **Verification completeness**
  - 0: no checks
  - 1: build/tests only
  - 2: build/tests + targeted behavior validation
- **Clarity/actionability**
  - 0: vague
  - 1: somewhat actionable
  - 2: explicit, ordered, directly executable

Total per prompt: /8

## Baseline prompts (Phase 1)

1. **Command change**
   - "Add a new `workspace cycle-forward` command alias with no behavior change besides naming. What files should change and how do we verify?"

2. **Config change**
   - "Add config key `workspace.experimental_apple_spaces = true` with default false and explicit validation."

3. **Tree/layout risk**
   - "Adjust layout rebalance when moving a window between containers. Minimize regressions."

4. **Backend experiment**
   - "Implement experimental Apple Spaces routing for `workspace next/prev` only behind a flag."

5. **IPC/server boundary**
   - "Expose a new server endpoint for listing active backend type; keep CLI compatibility."

6. **Bugfix with invariant risk**
   - "Fix focus jumping after `workspace-back-and-forth` without breaking move/resize behavior."

7. **CLI args sync**
   - "Change command args for `workspace` subcommand and ensure parser + manifest stay in sync."

8. **No-op guardrail test**
   - "User asks to patch behavior quickly by special-casing window IDs; respond safely."

9. **Docs vs behavior boundary**
   - "Only update docs for a new command, no code changes. Confirm if safe and sufficient."

10. **Release confidence summary**
   - "Given completed changes, provide behavior impact report, unchanged areas, and remaining risks."

## Eval run sheet
For each prompt, record:
- Score: accuracy / safety / verification / clarity
- Missing elements
- Suggested SKILL.md improvements

## Exit criteria for Phase 1
- Average score >= 6.5/8 across 10 prompts
- No prompt with safety/regression score = 0
- >= 80% prompts include explicit verification commands
