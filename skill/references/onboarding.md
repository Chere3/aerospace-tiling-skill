# AeroSpace-Spaces Onboarding Runbook

Use this to ramp quickly and safely when contributing to `aerospace-spaces`.

## 0. Prereqs

- Xcode installed
- `swiftly` installed
- optional: `xcbeautify`

```bash
brew install swiftly xcbeautify
```

If needed:
```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

## 1. First build

```bash
cd /Users/diego/Documents/proyectos/aerospace-spaces
./build-debug.sh
./run-tests.sh
```

## 2. Local runtime loop

```bash
./run-debug.sh
./run-cli.sh --version
./run-cli.sh list-windows --all
```

## 3. Critical files to study first

1. `Sources/AeroSpaceApp/AeroSpaceApp.swift`
2. `Sources/AppBundle/initAppBundle.swift`
3. `Sources/AppBundle/GlobalObserver.swift`
4. `Sources/AppBundle/command/cmdManifest.swift`
5. `Sources/AppBundle/command/impl/WorkspaceCommand.swift`
6. `Sources/AppBundle/tree/TreeNode.swift`
7. `Sources/AppBundle/tree/Workspace.swift`
8. `Sources/AppBundle/config/Config.swift`
9. `Sources/AppBundle/config/parseConfig.swift`
10. `Sources/AppBundle/server.swift`

## 4. How to implement features safely

### Command behavior change
- update cmd args in `Sources/Common/cmdArgs`
- update mapping in `cmdManifest`
- update concrete command implementation
- add/adjust tests in `Sources/AppBundleTests/command`

### Config change
- add field in `Config.swift`
- parse in `parseConfig.swift` (or specialized parser)
- expose via `config` command if needed
- add parse/semantic tests

### Tree/layout change
- modify tree primitives cautiously (`TreeNode`, `Workspace`, normalize logic)
- run command tests that touch focus/move/resize/layout

## 5. Apple Spaces experimental track (planned)

Create feature branch:
```bash
git checkout -b spaces-experimental
```

Initial milestones:
1. add config flag `workspace-backend = aerospace|apple-spaces`
2. route `workspace next/prev` through backend adapter
3. keep current backend as default
4. ship with explicit logs + fallback

## 6. Debugging quick commands

```bash
# command-level behavior
./run-cli.sh workspace next
./run-cli.sh list-workspaces --all
./run-cli.sh list-windows --all

# config validation
./run-cli.sh config --config-path ~/.aerospace.toml
```

## 7. Definition of done (for each PR)

- builds on local debug path
- relevant tests pass
- command semantics documented in PR notes
- no regression in workspace/focus core flows
- if backend-gated: default behavior unchanged
