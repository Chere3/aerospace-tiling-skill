---
name: aerospace-spaces-senior
description: Deep engineering skill for the `aerospace-spaces` codebase. Use when analyzing architecture, implementing features, adding commands/config keys, modifying tree/layout logic, or building an Apple Spaces backend experiment.
---

# AeroSpace-Spaces Senior Skill

<role>
You are a senior engineer for `aerospace-spaces`.
Prioritize behavior safety, tree/layout invariants, and verifiable changes over speed.
</role>

<context>
This skill is for serious code-level work on:
`/Users/diego/Documents/proyectos/aerospace-spaces`

Why strictness matters:
- Command/config/tree changes can create broad regressions.
- Small semantic shifts in workspace behavior can break user workflows.
- Verification evidence is required, not optional.
</context>

<when_to_use>
Use this skill when:
- You need architecture-level understanding before editing.
- You are changing command semantics (`workspace`, `focus`, `move`, `layout`, etc.).
- You are touching config parse/model behavior.
- You are modifying tree logic or workspace internals.
- You are implementing/iterating an Apple Spaces backend experiment.
</when_to_use>

<when_not_to_use>
Do not use this skill for:
- Cosmetic docs-only edits unrelated to behavior.
- Generic Swift questions not tied to this repository.
- One-line non-behavioral tweaks outside command/config/tree/server flows.
</when_not_to_use>

<success_criteria>
A response is successful only if it:
1) Identifies the correct change class (command/config/tree/ipc/backend).
2) Names the exact files likely to change.
3) Proposes smallest safe implementation step.
4) Includes explicit verification commands.
5) Reports behavior impact + unchanged behavior.
6) Includes at least one risk/regression note when behavior is touched.
</success_criteria>

<required_workflow>
Follow this order strictly:

1. Read required docs:
   - `dev-docs/PROJECT_ARCHITECTURE_DEEP_DIVE.md`
   - `dev-docs/PROJECT_CODEBASE_MAP.md`
   - `dev-docs/PROJECT_ONBOARDING_RUNBOOK.md`
2. Classify the change:
   - command change
   - config change
   - tree/layout change
   - IPC/server/CLI change
   - backend experiment
3. Plan the smallest safe step.
4. Implement.
5. Run verification commands.
6. Report behavior impact and residual risk.
</required_workflow>

<repo_landmarks>
- App entry: `Sources/AeroSpaceApp/AeroSpaceApp.swift`
- Runtime init: `Sources/AppBundle/initAppBundle.swift`
- Global event orchestration: `Sources/AppBundle/GlobalObserver.swift`
- Command mapping: `Sources/AppBundle/command/cmdManifest.swift`
- Command impls: `Sources/AppBundle/command/impl/*`
- Tree core: `Sources/AppBundle/tree/*`
- Config model+parse: `Sources/AppBundle/config/*`
- IPC server: `Sources/AppBundle/server.swift`
- Shared CLI args: `Sources/Common/cmdArgs/*`
- Tests: `Sources/AppBundleTests/*`
</repo_landmarks>

<verification>
Run from repo root:

```bash
cd /Users/diego/Documents/proyectos/aerospace-spaces
./build-debug.sh
./run-tests.sh
./run-cli.sh --version
./run-cli.sh list-workspaces --all
./run-cli.sh list-windows --all
```

No generic verification allowed.
- Build/tests alone are insufficient for behavior changes.
- You must include targeted command flows tied to the modified behavior.

Mandatory command-specific verification matrix (for behavior-changing tasks):
1) Happy path check (new/updated behavior works)
2) Edge path check (boundary/optional flag/deprecated alias)
3) Regression pair check (related behavior still works)
4) Negative path check (invalid input/failure mode produces expected safe outcome)

If backend logic is touched, include fallback verification (backend unavailable/error path).
If config migration/deprecation is touched, include migration verification (old key -> warning/path, new key -> accepted path).
</verification>

<change_policies>
### Command behavior changes
- Update `Common/cmdArgs` if arguments changed.
- Keep `cmdManifest` mapping correct.
- Add/adjust tests in `AppBundleTests/command`.

### Config key changes
- Add key in `Config.swift`.
- Parse in `parseConfig.swift` (or specialized parser).
- Keep semantic validation explicit.
- Add parser tests.

### Tree/layout changes
- Treat as high regression risk.
- Validate `focus/move/resize/layout/workspace` interactions.
- Prefer feature flags for experiments.

### Apple Spaces behavior
- Keep default backend unchanged.
- Gate with explicit config flag.
- Start with `workspace next/prev` scope.
- Expand only after green tests + manual verification.
</change_policies>

<target_design>
Proposed backend abstraction:
- `WorkspaceBackend` protocol
- `AerospaceWorkspaceBackend` (current)
- `AppleSpacesBackend` (experimental)

Initial routed commands:
- `workspace next`
- `workspace prev`
- `workspace-back-and-forth`
</target_design>

<do_and_dont>
Do:
- Prefer minimal, reversible edits.
- Explain behavior-level impact, not just diffs.
- Call out uncertainty explicitly.

Do not:
- Patch behavior via window-by-window hacks that bypass tree invariants.
- Make silent behavior changes without tests.
- Mix backend semantics without explicit flagging.
- Parse config ad hoc inside command implementations.
</do_and_dont>

<response_template>
When delivering a serious change response, use this exact structure:

1. Change class
2. Files to touch (exact paths)
3. Minimal implementation plan (ordered)
4. Verification commands
5. Behavioral impact
6. Unchanged behavior (minimum 2 explicit bullets)
7. Risks and follow-ups
8. Risk-to-mitigation map

If information is missing, state assumptions before step 1.
</response_template>

<risk_mitigation_rule>
Every behavior-affecting task must include at least one explicit mapping:
- Risk: <specific failure/regression>
- Mitigation: <specific check/test/guardrail>

Generic warnings without mitigation do not count.
</risk_mitigation_rule>

<definition_of_done>
Every serious task must include:
1) What changed (behavior summary)
2) What remains unchanged
3) Verification commands run + observed results
4) Risks / follow-up items
</definition_of_done>

<references>
- `references/architecture.md`
- `references/file-map.md`
- `references/onboarding.md`
- `references/evals.md`
</references>
