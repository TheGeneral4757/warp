# Build and Run

Set up your Warp development environment.

---

## Prerequisites

- **Rust 1.92.0** — Pinned in `rust-toolchain.toml` (auto-installed via rustup)
- **Git** — To clone the repository
- **System dependencies** — Platform-specific (see below)

---

## Clone the Repository

```bash
git clone https://github.com/warpdotdev/warp.git
cd warp
```

---

## Bootstrap Your Environment

Warp provides a bootstrap script for each platform:

```bash
./script/bootstrap
```

This installs:
- Rust dependencies
- System libraries
- Build tools
- Cargo plugins (nextest, etc.)

### macOS Bootstrap

```bash
./script/macos
```

Installs Xcode tools, Homebrew dependencies, Rust.

### Linux Bootstrap

```bash
./script/linux
```

Installs system libraries for your distribution (Ubuntu, Fedora, Arch, etc.).

### Windows Bootstrap

```bash
./script/windows
```

Sets up Windows build tools.

---

## Building Warp

### Standard Build

```bash
cargo build
```

Creates a debug binary in `target/debug/warp`.

### Optimized Release Build

```bash
cargo build --release
```

Creates an optimized binary in `target/release/warp`.

### Build Just the Binary

```bash
cargo build -p warp
```

Builds only the main `warp` binary (faster than full workspace).

---

## Running Warp

### From Source

```bash
cargo run
```

This builds and immediately runs Warp.

### From Compiled Binary

```bash
./target/debug/warp   # Debug build
./target/release/warp # Release build
```

### With Custom Server

To use a local Warp Server instead of cloud:

```bash
cargo run --features with_local_server
```

Or set environment variables:

```bash
export SERVER_ROOT_URL=http://localhost:8080
export WS_SERVER_URL=ws://localhost:8080/graphql/v2
cargo run
```

---

## Development Workflow

### 1. Make Code Changes

Edit files in `app/` or `crates/`.

### 2. Run Warp to Test

```bash
cargo run
```

### 3. Check Code Style

```bash
cargo fmt  # Format code
cargo clippy --workspace --all-targets --all-features --tests -- -D warnings
```

### 4. Run Tests

```bash
cargo nextest run --workspace
```

### 5. Run Full Presubmit

Before pushing a PR, run the full presubmit:

```bash
./script/presubmit
```

This runs:
- Format check
- Clippy linting
- All tests
- C/C++/Obj-C formatting (if applicable)
- PowerShell linting (if applicable)
- WGSL shader formatting

---

## Build Optimization

### Faster Incremental Builds

Use `mold` or `lld` linker (Linux):

```bash
export RUSTFLAGS="-C link-arg=-fuse-ld=mold"
cargo build
```

Or in `.cargo/config.local`:

```toml
[build]
rustflags = ["-C", "link-arg=-fuse-ld=mold"]
```

### Parallel Test Execution

Nextest runs tests in parallel by default:

```bash
cargo nextest run --workspace --no-fail-fast  # Run all tests, don't stop on first failure
```

---

## Platform-Specific Builds

### macOS Application Bundle

```bash
cargo bundle --bin warp
```

Creates `target/release/bundle/osx/Warp.app`.

### Linux AppImage

```bash
./script/wasm  # If WASM features needed
# Then use cargo build --release
```

### Windows NSIS Installer

Use the GitHub Actions workflow (see `.github/workflows/`).

---

## Features

### Optional Features

Some crates have optional features:

```bash
# Completer with v2 features
cargo run -p warp_completer --features v2

# Build with specific AI providers
cargo build --features "anthropic openai google"
```

---

## Dependency Management

### Update Dependencies

```bash
cargo update  # Update to latest compatible versions
```

### Check for Outdated

```bash
cargo outdated
```

### Check for Vulnerabilities

```bash
cargo audit
```

---

## Common Issues

### "Rust toolchain not installed"

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Switch to pinned version
rustup override set 1.92.0
```

### "Missing system dependencies" (Linux)

Run bootstrap:

```bash
./script/bootstrap
```

Or manually install (Ubuntu):

```bash
sudo apt-get install build-essential pkg-config libssl-dev
```

### "Build fails with linker errors"

Try:

```bash
cargo clean
cargo build
```

Or check if a build tool is missing (see bootstrap output).

### "Tests hang or freeze"

This usually means a UI test is trying to render. Run tests without UI:

```bash
cargo test --lib  # Library tests only (no UI)
```

---

## Performance Debugging

### See What's Being Compiled

```bash
cargo build -p warp --verbose
```

### Profile Build Time

```bash
cargo build -p warp --timings
```

Creates `target/cargo-timings.html` with detailed breakdown.

### Profile Runtime

```bash
cargo run --release -- --profile-name my_profile
```

(Check Warp's CLI for profiling options.)

---

## Environment Variables for Development

```bash
# Enable debug logging
export RUST_LOG=debug

# Use local server
export SERVER_ROOT_URL=http://localhost:8080
export WS_SERVER_URL=ws://localhost:8080/graphql/v2

# Custom shell
export WARP_SHELL=/bin/zsh

# Disable GPU (if rendering issues)
export WARP_DISABLE_GPU=1
```

---

## Next Steps

- **[Testing Guide](./TESTING_GUIDE.md)** — Run tests
- **[Coding Style](./CODING_STYLE.md)** — Learn the style rules
- **[Debugging](./DEBUGGING.md)** — Debug techniques
- **[WARP.md](../../WARP.md)** — Full engineering guide

---

[👈 Back to For Developers](./README.md)
