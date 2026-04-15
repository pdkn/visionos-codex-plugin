# Build visionOS Apps

`build-visionos-apps` is a plugin for building, debugging, refactoring, and
shipping visionOS 26 apps for Apple Vision Pro. It supports both **Codex** and
**Claude Code** runtimes under a shared repository.

## Repository Structure

```
plugins/build-visionos-apps/
├── shared/         ← Platform skills used by both flavours (14 skills)
├── codex/          ← Codex-specific orchestration (3 skills, commands, scripts)
└── claude/         ← Claude Code workflows (5 skills, agents, commands, hooks)
```

Platform knowledge (RealityKit, ARKit, spatial SwiftUI, SharePlay, USD, Shader
Graph, signing, immersive media, coding standards, packaging, SwiftPM, test
triage, WidgetKit) lives in `shared/` and is referenced by both flavours. Each
flavour adds its own orchestration skills, commands, and runtime-specific
configuration.

## What The Plugin Does

- Discovers local Xcode workspaces, projects, schemes, Swift packages, and
  Apple Vision Pro simulator targets
- Builds, runs, debugs, and captures logs for visionOS apps with
  `XcodeBuildMCP`
- Helps choose the right surface model: window, volume, immersive space, or a
  mixed flow between them
- Guides scene ownership, app structure, and spatial SwiftUI architecture
- Implements and troubleshoots RealityKit, ARKit, SharePlay, WidgetKit,
  immersive media, Shader Graph, and USD workflows
- Triages tests, signing failures, entitlement issues, privacy-key gaps, and
  launch blockers
- Supports packaging, TestFlight, and App Store submission workflows

## Claude Code Installation

For local development and testing:

```bash
claude --plugin-dir ./plugins/build-visionos-apps/claude
```

Use `/reload-plugins` inside a session to pick up changes without restarting.

The plugin registers XcodeBuildMCP as an MCP server automatically via
`.mcp.json`.

### Claude Code Features

**Engineering Workflow Skills** (new in the Claude Code flavour):

| Skill | Purpose |
|-------|---------|
| `spec-driven-spatial` | Write a feature spec before writing code, gated on scene model decision |
| `incremental-build` | Thin vertical slices - one RealityKit component/system at a time |
| `debugging-triage` | Five-step triage: reproduce, classify, isolate, fix, test |
| `adr-spatial` | Architecture decision records for spatial design choices |
| `git-workflow` | Atomic commits, dedicated .xcodeproj and .entitlements commits |

**Agent Personas:**

| Agent | When to Use |
|-------|-------------|
| `spatial-architect` | New feature specs, architecture reviews, scene model decisions |
| `realitykit-debugger` | Build succeeds but runtime behaviour is wrong |
| `xcode-build-agent` | Build failures, signing issues, distribution tasks |

**Slash Commands:**

| Command | Purpose |
|---------|---------|
| `/build-and-run-visionos-app` | Build and launch on Apple Vision Pro simulator |
| `/fix-visionos-capability-error` | Diagnose and fix capability/signing errors |
| `/test-visionos-app` | Run tests with failure classification |
| `/spec` | Start a feature specification |
| `/plan` | Break a spec into ordered, verifiable tasks |
| `/review` | Multi-axis code review (correctness, spatial, Swift, security, performance) |
| `/ship` | Pre-launch checklist for TestFlight and App Store |

## Codex Installation

The original Codex plugin is now located at `plugins/build-visionos-apps/codex/`.

### Option 1: Download The Packaged ZIP

Download `build-visionos-apps.zip` from the release assets and unzip into your
Codex plugins directory:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/plugins"
unzip build-visionos-apps.zip -d "${CODEX_HOME:-$HOME/.codex}/plugins"
```

### Option 2: Clone The Repo And Use The Installer Script

```bash
git clone https://github.com/studiomeije/visionos-codex-plugin.git
cd visionos-codex-plugin
./scripts/install-plugin.sh
```

The script copies the plugin from `plugins/build-visionos-apps/codex/` into your
Codex plugins directory. Target a custom location with:

```bash
./scripts/install-plugin.sh --home ~/.codex
./scripts/install-plugin.sh --plugins-dir /path/to/codex/plugins
```

### Codex Usage

- Ask directly for the outcome you want and let Codex choose the bundled skills
- Type `@` to invoke `Build visionOS Apps` or one of its skills explicitly
- Use the command layer: `/build-and-run-visionos-app`,
  `/fix-visionos-capability-error`, or `/test-visionos-app`

## Shared Platform Skills (`shared/skills/`)

These 14 skills live in `shared/` and are referenced by both flavours:

- **spatial-architecture** - scene model, surface selection, state ownership
- **realitykit** - entities, components, systems, render loop
- **arkit** - sessions, providers, anchors, tracked world
- **shareplay** - group activities, shared immersive presence
- **shader-graph** - materials authoring and debugging
- **usd** - asset editing, validation, runtime loading
- **signing-entitlements** - signing, entitlements, privacy keys
- **immersive-media** - spatial video, immersive playback
- **swiftui-spatial** - spatial views, scene types, visionOS modifiers
- **coding-standards** - Swift 6 concurrency, actor isolation, @Observable
- **packaging-distribution** - archive, TestFlight, App Store submission
- **swiftpm-visionos** - Swift Package Manager, Reality Composer Pro
- **test-triage** - XCTest and Swift Testing failure classification
- **widgetkit** - visionOS WidgetKit spatial UI, mounting, animations

## Optional External Tools

- `AXe` for post-launch simulator automation, screenshots, and accessibility
  inspection
- `asc` for App Store Connect automation (TestFlight, metadata, submission)

These tools are not bundled with the plugin.

## Maintenance

The repo-to-repo sync workflow with `visionOSAgents` is documented in
`docs/sync-agents.skills.md`.

---

## Credits

This plugin and its shared skill work were inspired by the work of:

- [Ivan Campos](https://github.com/ivancampos)
- [Paul Hudson](https://github.com/twostraws)
- [Pedro Pinera Buendia](https://github.com/pepicrft)
- [Thomas Ricouard](https://github.com/Dimillian/)
- [Sharno](https://github.com/sharno)

The Claude Code engineering workflow skills are adapted from
[agent-skills](https://github.com/addyosmani/agent-skills) by
[Addy Osmani](https://github.com/addyosmani).
