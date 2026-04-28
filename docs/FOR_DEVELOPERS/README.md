# For Developers

Guide to contributing to Warp and understanding its architecture.

---

## Getting Started

1. **[Build and Run](./BUILD_AND_RUN.md)** — Set up your development environment
2. **[Testing Guide](./TESTING_GUIDE.md)** — Running tests and presubmit
3. **[Coding Style](./CODING_STYLE.md)** — Style rules and conventions

---

## Learning Warp

- **[AI System Architecture](./AI_SYSTEM_ARCHITECTURE.md)** — How AI models are integrated
- **[Debugging](./DEBUGGING.md)** — Debugging techniques and troubleshooting
- **[Feature Flags](./FEATURE_FLAGS.md)** — Adding and managing feature gates

---

## Important Resources

- **[WARP.md](../../WARP.md)** — Complete engineering guide (upstream source of truth)
- **[CLAUDE.md](../../CLAUDE.md)** — Claude Code integration guide
- **[CONTRIBUTING.md](../../CONTRIBUTING.md)** — PR workflow and issue process
- **[Specs](../../specs/)** — Product and technical specifications for features

---

## Quick Links

| I want to... | Read this |
|-------------|-----------|
| Set up development environment | [Build and Run](./BUILD_AND_RUN.md) |
| Run tests | [Testing Guide](./TESTING_GUIDE.md) |
| Understand code style | [Coding Style](./CODING_STYLE.md) |
| Add a feature flag | [Feature Flags](./FEATURE_FLAGS.md) |
| Understand AI model system | [AI System Architecture](./AI_SYSTEM_ARCHITECTURE.md) |
| Debug Warp | [Debugging](./DEBUGGING.md) |
| View engineering guide | [WARP.md](../../WARP.md) |
| Make a PR | [CONTRIBUTING.md](../../CONTRIBUTING.md) |

---

## Architecture Overview

**Warp is a Rust-based terminal with AI capabilities.**

Key layers (top to bottom):

1. **UI Layer** — Custom Flutter-inspired framework (`crates/warpui`)
2. **App Layer** — Main app, terminal emulation, AI features (`app/`)
3. **Domain Crates** — Core functionality (editor, command, AI, etc.)
4. **Utilities** — Cross-cutting concerns (logging, features, HTTP, etc.)

See [WARP.md - Architecture](../../WARP.md) for full details.

---

## Common Tasks

### Adding a New Feature

1. **Spec first** — Write PRODUCT.md and TECH.md in `specs/`
2. **Implement** — Create PR with code changes
3. **Test** — Unit tests, integration tests, manual testing
4. **Feature flag** — Gate with feature flag if experimental
5. **Promote** — Roll out to Dogfood → Preview → Stable

Use the `implement-specs` skill for this workflow.

### Fixing a Bug

1. **Identify** — Reproduce and understand the issue
2. **Fix** — Write code that resolves it
3. **Test** — Add test case to prevent regression
4. **Run presubmit** — `./script/presubmit`
5. **Create PR** — Follow [CONTRIBUTING.md](../../CONTRIBUTING.md)

Use the `fix-errors` skill if you have build/test failures.

### Adding Telemetry

1. **Define event** — What do you want to track?
2. **Add code** — Instrument the feature
3. **Submit** — PR with telemetry event

Use the `add-telemetry` skill for this.

---

## Technology Stack

- **Language:** Rust 1.92.0 (pinned in `rust-toolchain.toml`)
- **UI Framework:** Custom Warp UI (`crates/warpui`)
- **Terminal Emulation:** Custom implementation (`app/src/terminal/`)
- **Database:** SQLite + Diesel ORM
- **Testing:** Cargo test, Nextest, custom integration framework
- **Features:** Feature flags, specs-driven development

---

## Workspace Structure

```
warp/
├── app/                      # Main binary
├── crates/                   # 63+ member crates
│   ├── warpui/              # Custom UI framework
│   ├── warp_core/           # Core utilities
│   ├── ai/                  # AI integration
│   ├── editor/              # Editor functionality
│   ├── command/             # Command handling
│   └── ... (many more)
├── specs/                    # Feature specifications
├── migrations/               # Database migrations
├── scripts/                  # Build scripts
└── docs/                     # Documentation (you are here)
```

---

## Code Organization

### If you're modifying...

- **Terminal behavior** → `app/src/terminal/`
- **AI features** → `app/src/ai/` or `crates/ai/`
- **UI components** → `crates/warpui/src/` or `app/src/ui/`
- **Settings/config** → `app/src/settings/`
- **Database/persistence** → `app/src/persistence/`

---

## Development Workflow

```bash
# 1. Clone and bootstrap
git clone https://github.com/yourusername/warp.git
cd warp
./script/bootstrap

# 2. Build and run
cargo run  # or ./script/run

# 3. Run tests
cargo nextest run --workspace

# 4. Check code style
./script/presubmit

# 5. Create a branch and make changes
git checkout -b feature/my-feature

# 6. Test your changes
cargo nextest run -p my_crate

# 7. Push and create PR
git push origin feature/my-feature
# Then open PR on GitHub
```

---

## Key Concepts

- **Entity-Handle Pattern** — Views hold handles, resolve via context
- **Mouse Input** — `MouseStateHandle` created once, not per-frame
- **Terminal Locking** — `TerminalModel::lock()` is a deadlock hazard
- **Feature Flags** — Compile-time defined, checked at runtime
- **Cross-platform** — macOS, Linux, Windows, WASM targets

See [WARP.md - Key Patterns](../../WARP.md) for details.

---

## Getting Help

- **Building issues?** → Use the `fix-errors` skill
- **Test failures?** → Use the `diagnose-ci-failures` skill  
- **UI questions?** → See `warp-ui-guidelines` skill
- **Integration tests?** → See `warp-integration-test` skill
- **General questions?** → Check [WARP.md](../../WARP.md) or ask in discussions

---

[👈 Back to Docs](../README.md)
