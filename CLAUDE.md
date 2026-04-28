# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This is a fork of [warpdotdev/warp](https://github.com/warpdotdev/warp). `WARP.md` is the upstream engineering guide and is the source of truth for build/test/style details — this file summarizes the parts most relevant to Claude sessions and points to where to dig in.

## Repo Snapshot

- Rust workspace pinned to `1.92.0` via `rust-toolchain.toml` (rustfmt + clippy components, minimal profile).
- ~63 member crates under `crates/` plus the main binary in `app/`.
- Custom UI framework lives in `crates/warpui` and `crates/warpui_core` (MIT-licensed; rest of repo is AGPL-3.0).
- Specs for in-flight features under `specs/` (PRODUCT.md + TECH.md per feature; spec-driven workflow is real, not aspirational).
- Heavy use of skills under `.claude/skills/` and `.agents/skills/` (mirrored). When a task matches one of those skills (e.g. `add-feature-flag`, `add-telemetry`, `warp-integration-test`, `fix-errors`, `warp-ui-guidelines`, `create-pr`, `review-pr`, `resolve-merge-conflicts`, `promote-feature`, `remove-feature-flag`, `rust-unit-tests`, `diagnose-ci-failures`, `implement-specs`, `spec-driven-implementation`, `write-product-spec`, `write-tech-spec`), invoke the skill instead of improvising.

## Common Commands

Build / run:
- `./script/bootstrap` — platform-specific setup (calls into `script/macos`, `script/linux`, `script/windows`).
- `./script/run` or `cargo run` — build and run Warp.
- `cargo run --features with_local_server` — run against a local `warp-server` (override with `SERVER_ROOT_URL` and `WS_SERVER_URL` env vars; defaults are `http://localhost:8080` and `ws://localhost:8080/graphql/v2`).
- `cargo bundle --bin warp` — bundle the macOS app.

Tests:
- `cargo nextest run --no-fail-fast --workspace --exclude command-signatures-v2` — full workspace test run (use nextest, not `cargo test`, for parallelism).
- `cargo nextest run -p <crate> -- <test_name>` — single test or filter.
- `cargo nextest run -p warp_completer --features v2` — completer with v2 features.
- `cargo test --doc` — doc tests (nextest doesn't run these).
- Integration tests live in `crates/integration` and use a custom Builder/TestStep framework — use the `warp-integration-test` skill when adding/debugging them.

Lint / format (must all pass before any PR push):
- `./script/presubmit` — runs the full fmt + clippy + tests gauntlet. **Always run this (or at minimum `cargo fmt` + the clippy invocation below) before pushing a PR branch.**
- `cargo fmt`
- `cargo clippy --workspace --all-targets --all-features --tests -- -D warnings`
- `./script/run-clang-format.py -r --extensions 'c,h,cpp,m' ./crates/warpui/src/ ./app/src/` — C/C++/Obj-C.
- `find . -name "*.wgsl" -exec wgslfmt --check {} +` — WGSL shaders.

Other:
- `./script/install_cargo_build_deps`, `./script/install_cargo_test_deps` — install required Cargo plugins.
- `./script/wasm` — WASM build helper.
- `./script/lint_powershell` — PowerShell lint (custom rules in `PSScriptAnalyzerCustomRules.psm1`).

## Architecture (Big Picture)

**Layers, top → bottom:**

1. `app/` — the main `warp` binary. Owns terminal emulation (`app/src/terminal/`), AI / Agent Mode (`app/src/ai/`), Warp Drive cloud sync (`app/src/drive/`), auth (`app/src/auth/`), settings (`app/src/settings/`), workspace/session management (`app/src/workspace/`), and the SQLite persistence layer (`app/src/persistence/`, schema in `app/src/persistence/schema.rs`).
2. `crates/warpui` + `crates/warpui_core` — custom Flutter-inspired UI framework. Entity-Component-Handle pattern: a global `App` owns all views/models as entities; views hold `ViewHandle<T>` references; `AppContext`/`ViewContext`/`ModelContext` give scoped access during render and event handling. Elements describe layout; Actions handle events.
3. `crates/warp_core` — core utilities and platform abstractions (including `features.rs`, the source of `FeatureFlag`).
4. Domain crates — `editor`, `command`, `warp_terminal`, `warp_completer`, `lsp`, `languages`, `graphql`, `ipc`, `jsonrpc`, `persistence`, `ai`, `computer_use`, `voice_input`, etc. Treat these as the building blocks `app/` composes.
5. Cross-cutting: `warp_features` (flag plumbing), `warp_logging`, `simple_logger`, `warp_util`, `http_client`/`http_server`, `websocket`, `warp_server_client`, `warp_graphql_schema`.

**Key cross-file patterns to be aware of before editing:**

- **Entity-Handle**: views never own each other directly — they hold handles and resolve them via the current context. Don't store raw references across frames.
- **Mouse input**: `MouseStateHandle` must be created **once during construction** and cloned/shared. Inlining `MouseStateHandle::default()` in a render path silently breaks all mouse interactions on that element. (See `WARP.md`.)
- **Terminal model locking**: `TerminalModel::lock()` is a deadlock hazard. Never call it if anything up the call stack might already hold the lock; prefer threading an already-locked reference down. A re-entrant lock here = beach ball / UI freeze.
- **Cross-platform**: native code paths for macOS, Windows, Linux + a WASM target. Platform-specific code is `cfg`-gated; check `script/macos|linux|windows` and the `cfg` flags before assuming a code path runs everywhere.
- **GraphQL**: schema and client are codegen'd from `crates/graphql/api/schema.graphql` (also generates TS types).
- **Database**: Diesel + SQLite. Migrations under `migrations/` (Diesel config in `diesel.toml`). Schema is checked in.

## Feature Flags

Flags are compile-time-defined but checked at runtime. To add one:

1. Add a variant to `FeatureFlag` in `crates/warp_core/src/features.rs`.
2. Optionally add to `DOGFOOD_FLAGS`, `PREVIEW_FLAGS`, or `RELEASE_FLAGS` for default-on rollout in those channels.
3. Gate code with `FeatureFlag::YourFlag.is_enabled()` — **prefer this over `#[cfg(...)]`** unless the code literally won't compile without the cfg (platform-specific deps).
4. Hide UI surfaces behind the same flag.
5. Use the `add-feature-flag`, `promote-feature`, and `remove-feature-flag` skills for the full lifecycle.

## Style Rules That Bite

These all come from `WARP.md` and clippy:

- Inline format args: `format!("{x}")`, not `format!("{}", x)` (clippy `uninlined_format_args` is `-D warnings`).
- No `_unused` parameters — delete them and update call sites.
- Function order: `ctx: &mut AppContext` (or `ViewContext`/`ModelContext`) is named `ctx` and goes **last**, except when there's a closure parameter (closure goes last, ctx second-to-last).
- Avoid wildcard `_ =>` in `match` over enums you control — exhaustive matching catches new variants at compile time.
- Don't strip existing comments in unrelated changes; only update a comment if its referenced logic actually changed.
- Avoid redundant type annotations (especially in closures); prefer imports over long path qualifiers, except inside `cfg`-guarded scopes where local imports / absolute paths are fine.

## Tests

- Use `cargo nextest` for everything except doc tests.
- Unit tests go in a sibling file using the convention:
  ```rust
  // in foo.rs
  #[cfg(test)]
  #[path = "foo_tests.rs"]  // or "mod_test.rs"
  mod tests;
  ```
- Integration tests: custom framework in `crates/integration` — see the `warp-integration-test` skill.
- Always run presubmit before pushing PR updates.

## PR Workflow

- Use the template at `.github/pull_request_template.md`.
- `./script/presubmit` (or at minimum the matching `cargo fmt` + `cargo clippy` invocations) **must** be green before opening or updating a PR.
- Changelog lines (in the PR body, format defined at the bottom of the template) — use sparingly:
  - `CHANGELOG-NEW-FEATURE:` — sizable new feature
  - `CHANGELOG-IMPROVEMENT:` — improvements to existing features
  - `CHANGELOG-BUG-FIX:` — bug/regression fixes
  - `CHANGELOG-IMAGE:` — GCP-hosted image URLs
  - Leave blank/remove if no entry is needed.
- The `create-pr` skill handles the PR-creation flow for this repo. Review automation flows through Oz (see `CONTRIBUTING.md`).
- Feature work is gated on issue readiness labels (`ready-to-spec` → spec PR under `specs/` → `ready-to-implement` → code PR). Bug fixes are implicitly `ready-to-implement`. Don't open code PRs for unlabeled feature requests.

## Pointers

- `WARP.md` — full engineering guide (read this for anything not covered above).
- `CONTRIBUTING.md` — issue/PR workflow, Oz triage, spec PR structure.
- `specs/` — committed product + tech specs for in-flight features.
- `.claude/skills/` and `.agents/skills/` — task-specific playbooks. Prefer skills over freelancing for any task they describe.
- `script/presubmit`, `script/run`, `script/bootstrap` — the canonical entry points for build, run, and CI parity.
