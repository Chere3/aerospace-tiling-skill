---
name: aerospace-spaces-senior
description: Deep engineering skill for the `aerospace-spaces` codebase. Use when analyzing architecture, implementing features, adding commands/config keys, modifying tree/layout logic, or building an Apple Spaces backend experiment.
---

# AeroSpace-Spaces Senior Skill

This skill is for **serious code-level work** on:
`/Users/diego/Documents/proyectos/aerospace-spaces`

## Use this skill when

- You need a full architecture understanding of the repo
- You are changing command semantics (`workspace`, `focus`, `move`, `layout`, etc.)
- You are touching config parse/model behavior
- You are modifying tree logic or workspace internals
- You are implementing/iterating an Apple Spaces backend experiment

## Required workflow (strict)

1. Read:
   - `dev-docs/PROJECT_ARCHITECTURE_DEEP_DIVE.md`
   - `dev-docs/PROJECT_CODEBASE_MAP.md`
   - `dev-docs/PROJECT_ONBOARDING_RUNBOOK.md`
2. Identify change class:
   - command change
   - config change
   - tree/layout change
   - IPC/server/CLI change
3. Implement in smallest safe step
4. Run verification commands
5. Report behavior impact (not just file diff)

## Repo landmarks

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

## Verification commands

```bash
cd /Users/diego/Documents/proyectos/aerospace-spaces
./build-debug.sh
./run-tests.sh
./run-cli.sh --version
./run-cli.sh list-workspaces --all
./run-cli.sh list-windows --all
```

## Change policies

### If you add/modify command behavior
- Update `Common/cmdArgs` if arguments changed
- Keep `cmdManifest` mapping correct
- Add/adjust tests in `AppBundleTests/command`

### If you add config keys
- Add to `Config.swift`
- Parse in `parseConfig.swift` (or specialized parser)
- Ensure semantic validation path is explicit
- Add parser tests

### If you touch tree/layout
- Assume high regression risk
- Validate `focus/move/resize/layout/workspace` interactions
- Prefer feature flags for experiments

### If you implement Apple Spaces behavior
- Keep default backend unchanged
- Gate via config flag
- Start with `workspace next/prev` only
- Expand scope after green tests + manual runs

## Apple Spaces experiment target design

Proposed abstraction:
- `WorkspaceBackend` protocol
- `AerospaceWorkspaceBackend` (current)
- `AppleSpacesBackend` (experimental)

Initial command routing:
- `workspace next`
- `workspace prev`
- `workspace-back-and-forth`

## Anti-patterns to avoid

- Window-by-window hacks that bypass tree invariants
- Silent behavior changes without tests
- Mixing backend semantics without explicit flagging
- Parsing config in command implementations

## Deliverables for every serious task

- What changed (behavioral summary)
- What remains unchanged
- Verification commands and observed results
- Risk notes (especially for workspace/tree logic)

## References

See local references in:
- `references/architecture.md`
- `references/file-map.md`
- `references/onboarding.md`
