# AeroSpace-Spaces Codebase Map

Generated map for fast onboarding and architectural orientation.

## Sources/AeroSpaceApp
- Total files: **1**
- Code files (.swift/.m/.h): **1**
- First entries: AeroSpaceApp.swift

## Sources/AppBundle
- Total files: **122**
- Code files (.swift/.m/.h): **122**
- First entries: GlobalObserver.swift, command, config, focus.swift, focusCache.swift, getNativeFocusedWindow.swift, initAppBundle.swift, layout, model, mouse, normalizeLayoutReason.swift, runLoop.swift, server.swift, shell, subscriptions.swift, tree, ui, util, windowLevelCache.swift

## Sources/Common
- Total files: **82**
- Code files (.swift/.m/.h): **82**
- First entries: appMetadata.swift, cmdArgs, cmdHelpGenerated.swift, gitHashGenerated.swift, model, util, versionGenerated.swift

## Sources/Cli
- Total files: **3**
- Code files (.swift/.m/.h): **3**
- First entries: _main.swift, cliUtil.swift, subcommandDescriptionsGenerated.swift

## Sources/PrivateApi
- Total files: **3**
- Code files (.swift/.m/.h): **2**
- First entries: include

## Sources/AppBundleTests
- Total files: **33**
- Code files (.swift/.m/.h): **33**
- First entries: AxUiElementWindowTypeTest.swift, assert.swift, command, config, model, shell, testExtensions.swift, testUtil.swift, tree

## dev-docs
- Total files: **2**
- Code files (.swift/.m/.h): **0**
- First entries: architecture.md, development.md

## docs
- Total files: **67**
- Code files (.swift/.m/.h): **0**
- First entries: aerospace-balance-sizes.adoc, aerospace-close-all-windows-but-current.adoc, aerospace-close.adoc, aerospace-config.adoc, aerospace-debug-windows.adoc, aerospace-enable.adoc, aerospace-exec-and-forget.adoc, aerospace-flatten-workspace-tree.adoc, aerospace-focus-back-and-forth.adoc, aerospace-focus-monitor.adoc, aerospace-focus.adoc, aerospace-fullscreen.adoc, aerospace-join-with.adoc, aerospace-layout.adoc, aerospace-list-apps.adoc, aerospace-list-exec-env-vars.adoc, aerospace-list-modes.adoc, aerospace-list-monitors.adoc, aerospace-list-windows.adoc, aerospace-list-workspaces.adoc

## script
- Total files: **9**
- Code files (.swift/.m/.h): **0**
- First entries: build-brew-cask.sh, check-uncommitted-files.sh, clean-project.sh, clean-xcode.sh, generate-cmd-help.sh, install-dep.sh, publish-release.sh, reset-accessibility-permission-for-debug.sh, setup.sh

## grammar
- Total files: **3**
- Code files (.swift/.m/.h): **0**
- First entries: ShellLexer.g4, ShellParser.g4, commands-bnf-grammar.txt

## Command Implementations (`Sources/AppBundle/command/impl`)
- Total commands: **37**
- BalanceSizes
- Close
- CloseAllWindowsButCurrent
- Config
- DebugWindows
- Enable
- ExecAndForget
- FlattenWorkspaceTree
- Focus
- FocusBackAndForth
- FocusMonitor
- Fullscreen
- JoinWith
- Layout
- ListApps
- ListExecEnvVars
- ListModes
- ListMonitors
- ListWindows
- ListWorkspaces
- MacosNativeFullscreen
- MacosNativeMinimize
- Mode
- Move
- MoveMouse
- MoveNodeToMonitor
- MoveNodeToWorkspace
- MoveWorkspaceToMonitor
- ReloadConfig
- Resize
- Split
- SummonWorkspace
- Swap
- TriggerBinding
- Volume
- Workspace
- WorkspaceBackAndForth