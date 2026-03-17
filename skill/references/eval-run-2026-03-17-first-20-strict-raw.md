# Eval Run - 2026-03-17 (First 20 prompts, strict, raw-response format)

Scope: P1-P20 from `skill/references/evals.md`  
Policy source: `skill/SKILL.md` (current)  
Model label: `openai-codex/gpt-5.3-codex`  
Scoring mode: strict (not optimistic)

---

### P1
**Prompt:** Add alias `workspace cycle-forward` with no semantic change.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command change (alias only, no semantic change).

2. Files to touch (exact paths)
- Sources/AppBundle/command/cmdManifest.swift
- Sources/AppBundle/command/impl/workspaceCommand.swift (only if alias mapping is handled in parser/dispatch helper there)
- Sources/AppBundleTests/command/WorkspaceCommandTests.swift

3. Minimal implementation plan (ordered)
- Add `workspace cycle-forward` alias to map to existing `workspace next` path.
- Ensure alias resolves before execution and reuses same implementation entrypoint.
- Add test asserting alias output/side effects match `workspace next` exactly.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace next
- ./run-cli.sh workspace cycle-forward
- ./run-cli.sh workspace cycle-forward --help

5. Behavioral impact
- New alias accepted; behavior remains identical to `workspace next`.

6. Unchanged behavior
- `workspace next` behavior is unchanged.
- Workspace ordering logic is unchanged.

7. Risks and follow-ups
- Risk: alias accidentally diverges in argument handling.
- Follow-up: snapshot test for argument passthrough parity.

8. Risk-to-mitigation map
- Risk: dispatch mismatch between alias and canonical command.
- Mitigation: test that both commands hit same implementation and produce identical state transition.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:**
- Accuracy: Correctly classified alias command change and pointed at manifest/tests.
- Safety: Includes concrete divergence risk and mitigation.
- Verification: Includes checks, but edge/negative validation is light.
- Clarity: Ordered and executable.
- Reporting: Impact + unchanged present, but residual behavior evidence is thin.
**Missing elements:** Explicit negative-path check for invalid alias arguments.
**SKILL.md improvement suggestion:** Require one negative-path command per alias/deprecation change.

### P2
**Prompt:** Add config key `workspace.experimental_apple_spaces = true` default false + validation.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Config change (model + parse + validation).

2. Files to touch (exact paths)
- Sources/AppBundle/config/Config.swift
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundleTests/config/ConfigParseTests.swift

3. Minimal implementation plan (ordered)
- Add bool key with default false in Config model.
- Parse `workspace.experimental_apple_spaces` into model.
- Add validation rule(s) for incompatible settings when enabled.
- Add tests for default false, explicit true, and invalid combinations.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh --config /tmp/config-default.toml list-workspaces --all
- ./run-cli.sh --config /tmp/config-apple-spaces-on.toml list-workspaces --all

5. Behavioral impact
- New opt-in flag controls experimental backend activation path.

6. Unchanged behavior
- Default behavior remains current backend when key is absent.
- Existing workspace commands continue current semantics unless flag enabled.

7. Risks and follow-ups
- Risk: malformed configs silently accepted.
- Follow-up: ensure parser emits clear errors/warnings.

8. Risk-to-mitigation map
- Risk: accidental default-on due to parse bug.
- Mitigation: test absent-key path asserts false default.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:**
- Accuracy: Correct config surface and files.
- Safety: Identifies default-on risk and containment.
- Verification: Good baseline but no explicit failing-config command example.
- Clarity: Clear sequence.
- Reporting: Adequate but concise.
**Missing elements:** Explicit negative-path verification command for incompatible combo.
**SKILL.md improvement suggestion:** Add mandatory invalid-config execution example in verification section.

### P3
**Prompt:** Add new CLI flag to `workspace` command in shared args.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- IPC/server/CLI change with command behavior surface.

2. Files to touch (exact paths)
- Sources/Common/cmdArgs/WorkspaceCmdArgs.swift
- Sources/AppBundle/command/cmdManifest.swift
- Sources/AppBundle/command/impl/workspaceCommand.swift
- Sources/AppBundleTests/command/WorkspaceCommandTests.swift

3. Minimal implementation plan (ordered)
- Add new flag in shared args definition with help text and parse type.
- Thread parsed value into workspace command execution.
- Keep default behavior unchanged when flag omitted.
- Add tests for flag set/unset and invalid value.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace next --new-flag
- ./run-cli.sh workspace next
- ./run-cli.sh workspace next --new-flag=invalid

5. Behavioral impact
- Workspace command now accepts new optional flag.

6. Unchanged behavior
- Existing invocation without flag is unchanged.
- Non-workspace commands are unaffected.

7. Risks and follow-ups
- Risk: shared args update breaks other command parsing.
- Follow-up: run command parse regression tests.

8. Risk-to-mitigation map
- Risk: malformed flag handling causes silent fallback.
- Mitigation: invalid-value test must fail with explicit error.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Accuracy and safety are strong; verification still mostly generic + minimal targeted checks.
**Missing elements:** No explicit regression pair command beyond single non-workspace statement.
**SKILL.md improvement suggestion:** none

### P4
**Prompt:** Expose active backend type over IPC server.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- IPC/server/CLI change.

2. Files to touch (exact paths)
- Sources/AppBundle/server.swift
- Sources/AppBundle/config/Config.swift
- Sources/AppBundleTests/server/ServerResponseTests.swift

3. Minimal implementation plan (ordered)
- Extend server response payload to include active backend identifier.
- Source value from resolved runtime backend (not raw config string).
- Add response schema test and backward compatibility guard.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh list-workspaces --all
- ./run-cli.sh list-workspaces --all | rg backend

5. Behavioral impact
- IPC consumers can read backend type for diagnostics.

6. Unchanged behavior
- Existing commands and workspace transitions remain unchanged.
- Backend selection logic itself is not modified.

7. Risks and follow-ups
- Risk: schema change may break older consumers.
- Follow-up: keep field additive and optional.

8. Risk-to-mitigation map
- Risk: nil/unknown backend leaks to client.
- Mitigation: normalize to known enum and test unknown path handling.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Correct class/files and additive-safety posture; verification doesn’t deeply test IPC compatibility.
**Missing elements:** Explicit negative test for backend unavailable/error path.
**SKILL.md improvement suggestion:** Add IPC backward-compat check requirement for server payload edits.

### P5
**Prompt:** Fix focus drift after `workspace-back-and-forth`.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Tree/layout change + command behavior change.

2. Files to touch (exact paths)
- Sources/AppBundle/command/impl/workspaceBackAndForthCommand.swift
- Sources/AppBundle/tree/FocusTracker.swift
- Sources/AppBundleTests/command/WorkspaceBackAndForthTests.swift

3. Minimal implementation plan (ordered)
- Reproduce drift with focused-window history transition.
- Ensure focus restore uses stable window identity and validity check.
- Fall back safely when prior focus target is gone.
- Add regression test for repeated back-and-forth.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace-back-and-forth
- ./run-cli.sh list-windows --all

5. Behavioral impact
- Focus remains on intended prior target when toggling workspaces.

6. Unchanged behavior
- Workspace history switching order remains unchanged.
- Command name/arguments remain unchanged.

7. Risks and follow-ups
- Risk: restoring stale window id could misfocus.
- Follow-up: stale-id cleanup in history cache.

8. Risk-to-mitigation map
- Risk: invalid prior focus target after window close.
- Mitigation: guard with existence check and deterministic fallback.
```
**Scores:** A/S/V/C/R = 2/1/1/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Good plan, but risk handling and verification are not deep enough for tree/focus regressions.
**Missing elements:** Edge + negative tests (closed target, empty workspace, race path).
**SKILL.md improvement suggestion:** Require invariant checklist for focus/history fixes.

### P6
**Prompt:** Change layout rebalance after cross-container move.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Tree/layout change.

2. Files to touch (exact paths)
- Sources/AppBundle/tree/TreeMutations.swift
- Sources/AppBundle/tree/LayoutRebalance.swift
- Sources/AppBundleTests/tree/LayoutRebalanceTests.swift

3. Minimal implementation plan (ordered)
- Isolate rebalance step triggered after cross-container move.
- Adjust rebalance algorithm to preserve parent constraints.
- Add deterministic tests for source/target container ratios.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh move left
- ./run-cli.sh layout tiles horizontal vertical

5. Behavioral impact
- Post-move layout rebalance follows updated policy.

6. Unchanged behavior
- Move command syntax and routing are unchanged.
- Non-cross-container moves keep existing behavior.

7. Risks and follow-ups
- Risk: orphan/zero-sized nodes after rebalance.
- Follow-up: add structural invariant assertions.

8. Risk-to-mitigation map
- Risk: rebalance introduces invalid tree weights.
- Mitigation: enforce normalization + test expected weight sums.
```
**Scores:** A/S/V/C/R = 2/1/1/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Accurate but insufficiently specific around high-risk invariants and verification matrix.
**Missing elements:** Explicit negative test commands and regression pair checks.
**SKILL.md improvement suggestion:** Add mandatory invariant names for tree-change responses.

### P7
**Prompt:** Add `workspace previous-visible` command (new command path).
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command change.

2. Files to touch (exact paths)
- Sources/AppBundle/command/cmdManifest.swift
- Sources/AppBundle/command/impl/workspacePreviousVisibleCommand.swift
- Sources/AppBundleTests/command/WorkspacePreviousVisibleTests.swift

3. Minimal implementation plan (ordered)
- Add new manifest entry and command implementation wiring.
- Reuse visibility-filter helper used by related workspace commands.
- Add tests for normal path and no-visible-target path.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace previous-visible
- ./run-cli.sh list-workspaces --all

5. Behavioral impact
- New command switches to previous visible workspace only.

6. Unchanged behavior
- Existing `workspace prev` semantics remain unchanged.
- Backend selection behavior is unchanged.

7. Risks and follow-ups
- Risk: visible-state race causes wrong target selection.
- Follow-up: consider snapshotting visibility at command start.

8. Risk-to-mitigation map
- Risk: no visible candidate leads to ambiguous behavior.
- Mitigation: explicit no-op/error contract with test coverage.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Solid command-path targeting and safety note; verification still modest.
**Missing elements:** Negative-path command showing expected error/no-op text.
**SKILL.md improvement suggestion:** none

### P8
**Prompt:** Add parser warning for deprecated config key.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Config change (parse + warning behavior).

2. Files to touch (exact paths)
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundle/config/ConfigWarnings.swift
- Sources/AppBundleTests/config/ConfigWarningsTests.swift

3. Minimal implementation plan (ordered)
- Detect deprecated key during parse.
- Emit warning with replacement key guidance.
- Keep parsing successful unless key is otherwise invalid.
- Add tests for warning emission and deduplication.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh --config /tmp/config-with-deprecated.toml list-workspaces --all

5. Behavioral impact
- Deprecated keys now generate parser warnings.

6. Unchanged behavior
- Existing valid configs still parse normally.
- Runtime command behavior remains unchanged.

7. Risks and follow-ups
- Risk: warning spam or duplicate warnings.
- Follow-up: one-warning-per-key policy.

8. Risk-to-mitigation map
- Risk: deprecated key silently ignored without guidance.
- Mitigation: warning text includes replacement and location context.
```
**Scores:** A/S/V/C/R = 2/2/0/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Accurate and safe, but verification lacks targeted warning assertion mechanics.
**Missing elements:** Explicit capture/assertion of warning output and negative malformed-key case.
**SKILL.md improvement suggestion:** Require parser-warning verification with stderr/assertion command.

### P9
**Prompt:** `workspace next` should skip empty workspaces (behind flag).
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command change + backend experiment safety if flag tied to experimental behavior.

2. Files to touch (exact paths)
- Sources/AppBundle/command/impl/workspaceNextCommand.swift
- Sources/AppBundle/config/Config.swift
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundleTests/command/WorkspaceNextTests.swift

3. Minimal implementation plan (ordered)
- Add/consume config flag controlling skip-empty behavior.
- In `workspace next`, iterate candidates until non-empty (with wrap guard).
- Preserve existing behavior when flag is false.
- Add tests for all-empty, mixed, and wrap scenarios.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace next
- ./run-cli.sh --config /tmp/config-skip-empty-on.toml workspace next

5. Behavioral impact
- With flag enabled, empty workspaces are skipped in next traversal.

6. Unchanged behavior
- Flag disabled path remains previous semantics.
- `workspace prev` behavior unchanged unless explicitly updated.

7. Risks and follow-ups
- Risk: infinite loop when all workspaces empty.
- Follow-up: bounded iteration by workspace count.

8. Risk-to-mitigation map
- Risk: no eligible target yields hang.
- Mitigation: stop after N checks and return deterministic no-op/error.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Strong safety identification; verification missing explicit negative/all-empty command assertion.
**Missing elements:** None beyond deeper verification detail.
**SKILL.md improvement suggestion:** none

### P10
**Prompt:** Add command `workspace rotate-recent` using existing history.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command change.

2. Files to touch (exact paths)
- Sources/AppBundle/command/cmdManifest.swift
- Sources/AppBundle/command/impl/workspaceRotateRecentCommand.swift
- Sources/AppBundle/tree/WorkspaceHistory.swift
- Sources/AppBundleTests/command/WorkspaceRotateRecentTests.swift

3. Minimal implementation plan (ordered)
- Add new manifest command routed to history-based rotate logic.
- Reuse existing history storage; avoid parallel state.
- Define behavior for insufficient history length.
- Add tests for 0/1/N history entries.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace rotate-recent
- ./run-cli.sh workspace-back-and-forth

5. Behavioral impact
- New command rotates current workspace using recent-history order.

6. Unchanged behavior
- Existing history recording mechanism remains source of truth.
- Existing workspace navigation commands retain current semantics.

7. Risks and follow-ups
- Risk: history mutation side effects on other commands.
- Follow-up: enforce read-only rotate view vs write timing.

8. Risk-to-mitigation map
- Risk: rotate command corrupts history ordering.
- Mitigation: regression tests compare pre/post history invariants.
```
**Scores:** A/S/V/C/R = 2/1/1/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Technically good, but safety/verification coverage for history side effects is underpowered.
**Missing elements:** Explicit negative-path verification for insufficient history.
**SKILL.md improvement suggestion:** Add required history-integrity checks for history-based commands.

### P11
**Prompt:** Make `workspace-back-and-forth` ignore transient popups.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command behavior change + tree/focus interaction.

2. Files to touch (exact paths)
- Sources/AppBundle/command/impl/workspaceBackAndForthCommand.swift
- Sources/AppBundle/tree/WindowClassification.swift
- Sources/AppBundleTests/command/WorkspaceBackAndForthTests.swift

3. Minimal implementation plan (ordered)
- Define transient-popup predicate (role/type-based).
- Exclude transient windows from focus-history candidate selection.
- Keep fallback when only transient windows exist.
- Add tests for mixed regular/transient windows.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace-back-and-forth

5. Behavioral impact
- Popup/transient windows no longer hijack back-and-forth target selection.

6. Unchanged behavior
- Non-transient window history behavior remains unchanged.
- Command invocation interface remains unchanged.

7. Risks and follow-ups
- Risk: misclassifying legitimate windows as transient.
- Follow-up: classification table review with app-specific cases.

8. Risk-to-mitigation map
- Risk: over-filtering causes missing intended focus targets.
- Mitigation: fallback and tests for ambiguous classification.
```
**Scores:** A/S/V/C/R = 2/1/0/2/1 (Total 6/10)
**Auto-fail gate:** pass
**Rationale:** Reasonable plan, but verification is too shallow and safety criteria for classification are vague.
**Missing elements:** Negative-path tests and explicit popup classification validation commands.
**SKILL.md improvement suggestion:** Require explicit predicate definition + validation cases for classification changes.

### P12
**Prompt:** Extend `list-workspaces` with backend metadata.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- IPC/server/CLI change.

2. Files to touch (exact paths)
- Sources/AppBundle/server.swift
- Sources/AppBundle/command/impl/listWorkspacesCommand.swift
- Sources/AppBundleTests/command/ListWorkspacesTests.swift

3. Minimal implementation plan (ordered)
- Add backend metadata field to list-workspaces output model.
- Populate from active backend resolver.
- Keep output backward compatible (additive field).
- Add tests for both backend values.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh list-workspaces --all

5. Behavioral impact
- CLI output includes backend metadata per workspace listing.

6. Unchanged behavior
- Workspace ordering/filter behavior remains unchanged.
- Existing flags for list-workspaces continue to work.

7. Risks and follow-ups
- Risk: consumers parsing strict output format may break.
- Follow-up: gate with output format version if needed.

8. Risk-to-mitigation map
- Risk: metadata field absent for some backend states.
- Mitigation: enforce non-empty enum default and tests.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Strong structure; still lacks richer compatibility and negative-path verification.
**Missing elements:** Explicit assertion command for machine-readable output mode (if any).
**SKILL.md improvement suggestion:** none

### P13
**Prompt:** Introduce soft-deprecated command alias with warning.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command change (deprecation + warning behavior).

2. Files to touch (exact paths)
- Sources/AppBundle/command/cmdManifest.swift
- Sources/AppBundle/command/CommandWarnings.swift
- Sources/AppBundleTests/command/CommandDeprecationTests.swift

3. Minimal implementation plan (ordered)
- Register deprecated alias mapping to canonical command.
- Emit warning once per invocation with replacement guidance.
- Keep execution path identical to canonical command.
- Add tests for warning text and behavior parity.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh old-command-alias
- ./run-cli.sh canonical-command

5. Behavioral impact
- Deprecated alias still works but now warns users.

6. Unchanged behavior
- Canonical command behavior is unchanged.
- Alias keeps same side effects as canonical path.

7. Risks and follow-ups
- Risk: warning noise in scripts.
- Follow-up: optional suppress-warnings mode for automation.

8. Risk-to-mitigation map
- Risk: alias silently diverges over time.
- Mitigation: parity test fixture for alias vs canonical behavior.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Good risk mapping and compatibility mindset; verification not fully comprehensive.
**Missing elements:** Negative test for invalid alias args + warning capture assertion.
**SKILL.md improvement suggestion:** none

### P14
**Prompt:** Add optional `--wrap` to `workspace next/prev`.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command change + shared CLI args surface.

2. Files to touch (exact paths)
- Sources/Common/cmdArgs/WorkspaceCmdArgs.swift
- Sources/AppBundle/command/impl/workspaceNextCommand.swift
- Sources/AppBundle/command/impl/workspacePrevCommand.swift
- Sources/AppBundleTests/command/WorkspaceNextPrevTests.swift

3. Minimal implementation plan (ordered)
- Add optional `--wrap` flag in shared args.
- Apply wrap logic in next/prev traversal only when set.
- Preserve current boundary behavior when unset.
- Add tests for start/end boundaries in wrap and no-wrap modes.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh workspace next --wrap
- ./run-cli.sh workspace prev --wrap
- ./run-cli.sh workspace next

5. Behavioral impact
- Optional wrap-around traversal available for next/prev.

6. Unchanged behavior
- Default no-flag behavior remains unchanged.
- Other workspace commands are unaffected.

7. Risks and follow-ups
- Risk: wrap default accidentally flips behavior.
- Follow-up: parse default test explicitly asserts false.

8. Risk-to-mitigation map
- Risk: inconsistent wrap between next and prev.
- Mitigation: shared traversal helper + mirrored tests.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Accurate and actionable; still short of full verification-2 depth.
**Missing elements:** Explicit invalid-flag negative test and regression pair for no-wrap boundary result.
**SKILL.md improvement suggestion:** none

### P15
**Prompt:** Modify `focus` command to preserve layout intent on failures.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command behavior change + tree/layout interaction.

2. Files to touch (exact paths)
- Sources/AppBundle/command/impl/focusCommand.swift
- Sources/AppBundle/tree/FocusEngine.swift
- Sources/AppBundleTests/command/FocusCommandTests.swift

3. Minimal implementation plan (ordered)
- Ensure focus failure path does not mutate layout/planned orientation state.
- Return explicit error while retaining pre-command layout intent.
- Add tests for invalid target and transient failure scenarios.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh focus left
- ./run-cli.sh layout tiles horizontal
- ./run-cli.sh focus nonexistent-target

5. Behavioral impact
- Failed focus attempts no longer disturb layout intent state.

6. Unchanged behavior
- Successful focus transitions behave as before.
- Layout command semantics remain unchanged.

7. Risks and follow-ups
- Risk: hidden partial state mutations remain.
- Follow-up: wrap updates in transactional guard.

8. Risk-to-mitigation map
- Risk: failure path leaves inconsistent focus/layout state.
- Mitigation: atomic update or rollback with test assertions.
```
**Scores:** A/S/V/C/R = 2/1/1/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Good high-level direction; risk/verification need stronger concrete evidence.
**Missing elements:** Explicit edge-case matrix and concrete failure simulation setup.
**SKILL.md improvement suggestion:** Enforce “failure simulation commands required” for failure-path tasks.

### P16
**Prompt:** Ensure `move` command errors are explicit and non-silent.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Command behavior change.

2. Files to touch (exact paths)
- Sources/AppBundle/command/impl/moveCommand.swift
- Sources/AppBundle/command/CommandError.swift
- Sources/AppBundleTests/command/MoveCommandTests.swift

3. Minimal implementation plan (ordered)
- Replace silent failure paths with typed explicit errors.
- Ensure CLI reports actionable message and exit code.
- Add tests for each known failure condition.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh move left
- ./run-cli.sh move invalid-direction

5. Behavioral impact
- Move failures now report explicit errors instead of silent no-op.

6. Unchanged behavior
- Valid move operations preserve current semantics.
- Tree mutation logic for successful moves remains unchanged.

7. Risks and follow-ups
- Risk: callers relying on silent behavior may break.
- Follow-up: document behavior change in release notes.

8. Risk-to-mitigation map
- Risk: inconsistent error formatting across commands.
- Mitigation: centralize move errors in shared command error type.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Good safety and compatibility note; verification is not exhaustive.
**Missing elements:** Explicit negative-path checks for all failure variants.
**SKILL.md improvement suggestion:** none

### P17
**Prompt:** New bool: `workspace.enable_history_guard`.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Config change.

2. Files to touch (exact paths)
- Sources/AppBundle/config/Config.swift
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundleTests/config/ConfigParseTests.swift

3. Minimal implementation plan (ordered)
- Add bool key with default value in config model.
- Parse into runtime config.
- Wire consumers to read new guard toggle.
- Add parse tests for absent/true/false/invalid values.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh --config /tmp/config-history-guard-on.toml workspace next

5. Behavioral impact
- New guard toggle available for history protection logic.

6. Unchanged behavior
- Default behavior unchanged if key absent.
- Non-history command paths unchanged.

7. Risks and follow-ups
- Risk: key parsed but unused leading to false confidence.
- Follow-up: add test proving behavior delta when enabled.

8. Risk-to-mitigation map
- Risk: default value regressions.
- Mitigation: explicit absent-key default test.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Correct and safe; verification limited to minimal runtime proof.
**Missing elements:** Invalid-type negative command and behavior-level differential check.
**SKILL.md improvement suggestion:** none

### P18
**Prompt:** New enum: `workspace.backend = aerospace|apple-spaces`.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Config change + backend experiment safety.

2. Files to touch (exact paths)
- Sources/AppBundle/config/Config.swift
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundle/initAppBundle.swift
- Sources/AppBundleTests/config/ConfigParseTests.swift

3. Minimal implementation plan (ordered)
- Add enum type for backend selection in config model.
- Parse string values with strict validation.
- Resolve backend at runtime with safe default.
- Add tests for both values and invalid input.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh --config /tmp/config-backend-aerospace.toml list-workspaces --all
- ./run-cli.sh --config /tmp/config-backend-apple.toml list-workspaces --all

5. Behavioral impact
- Backend can be selected explicitly via config enum.

6. Unchanged behavior
- Default backend remains aerospace unless explicitly changed.
- Command surface remains unchanged.

7. Risks and follow-ups
- Risk: invalid enum silently falls back.
- Follow-up: hard fail on unknown value with clear error.

8. Risk-to-mitigation map
- Risk: mixed backend assumptions in runtime.
- Mitigation: single resolved backend source in app init + tests.
```
**Scores:** A/S/V/C/R = 2/2/1/2/1 (Total 8/10)
**Auto-fail gate:** pass
**Rationale:** Good mapping and safety posture; verification could include fallback failure path.
**Missing elements:** Explicit negative test command for invalid enum.
**SKILL.md improvement suggestion:** none

### P19
**Prompt:** Reject incompatible config combo for experimental backend.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Config change (validation rule).

2. Files to touch (exact paths)
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundle/config/ConfigValidation.swift
- Sources/AppBundleTests/config/ConfigValidationTests.swift

3. Minimal implementation plan (ordered)
- Define incompatible key combinations for experimental backend.
- Validate after parse and return clear actionable error.
- Add tests for rejected combos and accepted alternatives.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh --config /tmp/config-incompatible.toml list-workspaces --all

5. Behavioral impact
- Invalid experimental-backend config combinations now fail fast.

6. Unchanged behavior
- Compatible configurations still load normally.
- Non-experimental backend configs are unchanged.

7. Risks and follow-ups
- Risk: over-restrictive validation blocks valid setups.
- Follow-up: document exact incompatibility matrix.

8. Risk-to-mitigation map
- Risk: users cannot diagnose why config is rejected.
- Mitigation: include conflicting key names in error output.
```
**Scores:** A/S/V/C/R = 2/2/0/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Correct concept, but verification lacks explicit accepted-vs-rejected paired execution evidence.
**Missing elements:** Positive control command and stderr assertion details.
**SKILL.md improvement suggestion:** Require validation changes to include one reject + one accept runtime check.

### P20
**Prompt:** Add default migration path for renamed config key.
**Model:** openai-codex/gpt-5.3-codex
**Raw response:**
```text
1. Change class
- Config change (migration/deprecation handling).

2. Files to touch (exact paths)
- Sources/AppBundle/config/parseConfig.swift
- Sources/AppBundle/config/ConfigMigration.swift
- Sources/AppBundleTests/config/ConfigMigrationTests.swift

3. Minimal implementation plan (ordered)
- Map old key to new key during parse.
- Emit deprecation warning when old key is used.
- Define precedence when both keys exist.
- Add tests for old-only, new-only, both-present conflict path.

4. Verification commands
- ./build-debug.sh
- ./run-tests.sh
- ./run-cli.sh --config /tmp/config-old-key.toml list-workspaces --all
- ./run-cli.sh --config /tmp/config-new-key.toml list-workspaces --all

5. Behavioral impact
- Renamed key is migrated automatically with warning path.

6. Unchanged behavior
- New key remains canonical behavior source.
- Existing runtime command semantics do not change.

7. Risks and follow-ups
- Risk: dual-key configs apply wrong precedence.
- Follow-up: explicit precedence rule documented and tested.

8. Risk-to-mitigation map
- Risk: migration hides misconfiguration.
- Mitigation: warning includes old key, new key, and precedence outcome.
```
**Scores:** A/S/V/C/R = 2/1/1/2/1 (Total 7/10)
**Auto-fail gate:** pass
**Rationale:** Good migration framing; safety and verification not fully rigorous on conflict telemetry.
**Missing elements:** Explicit negative test when both keys conflict and warning assertion detail.
**SKILL.md improvement suggestion:** Add mandatory dual-key conflict verification requirement for migration tasks.

---

## Aggregate (P1-P20)
- Prompts run: 20
- Total score: 152/200
- Average: **7.60/10**
- Safety-2 coverage: **12/20 (60%)**
- Verification-2 coverage: **0/20 (0%)**
- Auto-fails: **0**

## Notes
- This run satisfies raw per-prompt block requirements and uses strict scoring.
- Main deficit remains verification depth (targeted edge/negative execution evidence).