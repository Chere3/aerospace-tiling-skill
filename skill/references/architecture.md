# AeroSpace-Spaces Architecture Deep Dive

This document explains the project as an engineering system (not just file listing).

## 1) Runtime model

AeroSpace has two primary runtime surfaces:

1. **App process** (`AeroSpace.app`) for window management + menu bar UI
2. **CLI process** (`aerospace`) for command execution against the running app via Unix socket

Core startup entrypoint:
- `Sources/AeroSpaceApp/AeroSpaceApp.swift`
- calls `initAppBundle()` from `Sources/AppBundle/initAppBundle.swift`

Startup sequence in `initAppBundle()` (simplified):
1. parse server args
2. load/validate config (fallback to default config)
3. check Accessibility permissions
4. start Unix socket server
5. initialize global observers (`GlobalObserver.initObserver()`)
6. initialize/focus workspace world
7. run startup refresh sessions + startup commands

---

## 2) Module boundaries

### `Sources/AppBundle` (main engine)
Major responsibilities:
- world/tree model (`tree/`)
- command runtime + command implementations (`command/`)
- config schema+parsing (`config/`)
- monitor/mouse/focus/session lifecycle
- menu bar + message UI
- Unix socket server

### `Sources/Common`
Shared infrastructure for app+cli:
- command argument kinds/parsers (`cmdArgs/`)
- DTO-ish shared models
- utility helpers used on both sides

### `Sources/Cli`
Command-line frontend:
- parses CLI args
- connects to running app over Unix socket
- prints errors/results

### `Sources/PrivateApi`
Thin bridge exposing private API (`_AXUIElementGetWindow`) used by the engine.

### `Sources/AppBundleTests`
Unit/integration-ish tests for command behavior, parser behavior, and tree logic.

---

## 3) Command pipeline

Flow from user command to behavior:
1. raw command line parsed into `CmdArgs` (Common)
2. `cmdManifest` maps `CmdArgs` kind -> concrete `Command`
3. command executes via `run(_ env, _ io)`
4. command mutates tree/workspace/window state (or queries it)
5. result serialized to stdout/json as needed

Core mapping file:
- `Sources/AppBundle/command/cmdManifest.swift`

Notable command families:
- navigation/focus: `Focus*`, `Workspace*`, `Move*`
- layout mutation: `Layout`, `Resize`, `Split`, `JoinWith`, `FlattenWorkspaceTree`, `BalanceSizes`
- system integration: `ExecAndForget`, `MoveMouse`, `MacosNative*`
- introspection: `ListWindows`, `ListWorkspaces`, `ListMonitors`, `Config`

---

## 4) Tree paradigm (i3-like)

The window manager computes layout from a **tree**, not from a single active window.

Core tree files:
- `tree/TreeNode.swift`
- `tree/TilingContainer.swift`
- `tree/Workspace.swift`
- `tree/normalizeContainers.swift`

Implication for behavior:
- resizing/layout decisions are global to container hierarchy
- one focused window does not define the whole workspace state
- flatten/balance commands are structural cleanup ops over the tree

---

## 5) Refresh/session orchestration

Global system events trigger refresh sessions:
- app activate/hide/unhide/terminate
- active Space changes
- mouse-up global monitor path

Primary observer:
- `Sources/AppBundle/GlobalObserver.swift`

The observer schedules refresh sessions rather than doing expensive state work inline; this keeps event processing responsive and centralizes reconciliation logic.

---

## 6) Config system

Core parse pipeline:
- `config/parseConfig.swift`
- plus specialized parsers:
  - `parseGaps.swift`
  - `parseOnWindowDetected.swift`
  - `parseWorkspaceToMonitorAssignment.swift`
  - `parseKeyMapping.swift`

Design notes:
- typed parse with semantic validation
- backward compatibility/deprecation messages handled at parser layer
- config watcher supports runtime reload behavior

---

## 7) IPC and server model

`AppBundle/server.swift` contains Unix domain socket handling for CLI<->app communication.

Behavior:
- if app server unavailable, CLI reports connection error
- command execution happens server-side in app process
- env/stdin values are passed in request payloads

This separation enables one authoritative WM state in app process.

---

## 8) Build + dev model

Primary docs:
- `dev-docs/development.md`

Typical paths:
- debug build scripts via SwiftPM wrappers
- release build uses Xcode flow + code signing cert
- generated artifacts/scripts present for shell completion/docs/xcode project regeneration

---

## 9) Where to modify for Apple Spaces backend experiment

If implementing a "spaces backend" variant, high-leverage change points:
1. **workspace commands** (`WorkspaceCommand`, `WorkspaceBackAndForthCommand`, `MoveWorkspaceToMonitorCommand` etc.)
2. **config switch/feature flag** in parse/config model (`Config.swift` + parser)
3. **observer policy** where active-space changes are interpreted
4. **query/list commands** semantics (`ListWorkspacesCommand`) for backend-specific truth

Recommended first spike:
- add backend flag in config, default current behavior
- route only `workspace next/prev` through experimental adapter
- keep the rest untouched to limit blast radius

---

## 10) Senior maintainer checklist for this repo

- Never change command semantics without updating both:
  - parser/args manifests
  - tests in `AppBundleTests/command`
- Treat tree structure as source of truth; avoid window-by-window hacks in core paths
- Keep IPC contract stable (`Cli` depends on it)
- Add feature flags for behavior experiments (Spaces backend)
- Validate behavior under multi-monitor + app activation + mouse drag edge cases
