# Testing Guide

How to write and run tests in Warp.

---

## Test Types

Warp uses three types of tests:

1. **Unit Tests** — Test individual functions/modules
2. **Integration Tests** — Test features end-to-end
3. **Doc Tests** — Test code examples in documentation

---

## Running Tests

### All Tests

```bash
cargo nextest run --workspace
```

Always use **nextest**, not `cargo test`, for parallelism and speed.

### Single Crate

```bash
cargo nextest run -p my_crate
```

### Specific Test

```bash
cargo nextest run -p my_crate -- my_test_name
```

### Tests with Filter

```bash
cargo nextest run --workspace -- "ai::" # Run tests matching "ai::"
```

### No Fail Fast (Run All Tests)

```bash
cargo nextest run --workspace --no-fail-fast
```

Doesn't stop on first failure — useful for seeing all failures at once.

### Doc Tests

```bash
cargo test --doc
```

Nextest doesn't run doc tests, so use `cargo test --doc`.

### Specific Feature

```bash
cargo nextest run -p warp_completer --features v2
```

---

## Writing Unit Tests

Unit tests go in a sibling file using Rust's module convention:

### Example: `foo.rs`

```rust
// File: crates/my_crate/src/foo.rs

pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
#[path = "foo_tests.rs"]
mod tests;
```

### Example: `foo_tests.rs`

```rust
// File: crates/my_crate/src/foo_tests.rs

use crate::foo::add;

#[test]
fn test_add() {
    assert_eq!(add(2, 2), 4);
}

#[test]
fn test_add_negative() {
    assert_eq!(add(-1, 1), 0);
}
```

---

## Integration Tests

Integration tests live in `crates/integration/` using a custom framework.

### Creating an Integration Test

```rust
// File: crates/integration/tests/my_feature.rs

use warp_integration_test::prelude::*;

#[warp_test]
async fn test_my_feature() {
    let builder = Builder::new();
    builder
        .step("open_warp")
        .step("type_command", "ls")
        .step("press_enter")
        .expect("output_contains", "Cargo.toml")
        .run()
        .await;
}
```

Use the `warp-integration-test` skill for help writing integration tests.

---

## Test Organization

### Unit Test Convention

```
crates/my_crate/src/
├── lib.rs
├── foo.rs          # Implementation
├── foo_tests.rs    # Tests for foo.rs
├── bar.rs
└── bar_tests.rs
```

The `#[path = "foo_tests.rs"]` and `#[cfg(test)]` attributes tell Rust to compile tests as a separate module.

### Integration Test Convention

```
crates/integration/tests/
├── agent_mode.rs       # Tests for Agent Mode
├── command_help.rs     # Tests for command help
├── cloud_sync.rs       # Tests for cloud sync
└── ...
```

---

## Test Best Practices

### Test Names Should Be Descriptive

✅ Good:
```rust
#[test]
fn test_parse_valid_json() { }

#[test]
fn test_parse_invalid_json_returns_error() { }
```

❌ Bad:
```rust
#[test]
fn test1() { }

#[test]
fn test_it() { }
```

### Test One Thing Per Test

```rust
// ✅ Good - test one behavior
#[test]
fn test_model_selection_persists() {
    assert_eq!(model.save().is_ok(), true);
}

// ❌ Bad - tests multiple things
#[test]
fn test_model_lifecycle() {
    assert_eq!(model.create().is_ok(), true);
    assert_eq!(model.save().is_ok(), true);
    assert_eq!(model.delete().is_ok(), true);
}
```

### Use Assertions Correctly

```rust
// ✅ Use assert! for booleans
assert!(result.is_ok());

// ✅ Use assert_eq! for comparisons
assert_eq!(result, expected);

// ✅ Use assert_ne! for inequality
assert_ne!(result, 0);

// ✅ Use expect messages
assert_eq!(result, expected, "Result should be {}", expected);
```

### Test Error Cases

```rust
#[test]
fn test_invalid_model_name_fails() {
    let result = Model::new("invalid_model_!!!!");
    assert!(result.is_err());
}
```

---

## Running Presubmit Before PR

Before pushing a PR, always run:

```bash
./script/presubmit
```

This runs:
- Format check (`cargo fmt --check`)
- Clippy linting
- All tests
- C/C++ formatting (if applicable)

If presubmit fails, your PR will fail in CI.

---

## Common Test Issues

### "Test hangs or times out"

Usually a UI rendering test trying to display something. Check if you can run tests without the UI:

```bash
cargo test --lib  # Library tests only
```

### "Test passes locally but fails in CI"

Often due to:
- **Timing issues** — Tests may run faster locally
- **Platform differences** — macOS vs Linux vs Windows
- **Resource availability** — CI may have less memory

Add explicit waits or platform-specific skips if needed.

### "Tests are too slow"

Use `--no-fail-fast` strategically:

```bash
cargo nextest run -p my_crate --no-fail-fast
```

Or run only the tests you're working on:

```bash
cargo nextest run -p my_crate -- test_my_feature
```

---

## Debugging Tests

### Print Debug Output

```rust
#[test]
fn test_my_function() {
    let result = my_function();
    println!("Result: {:?}", result);  // Shows in output with -v flag
    assert_eq!(result, expected);
}
```

Run with verbose output:

```bash
cargo nextest run -p my_crate -- my_test --nocapture
```

### Use dbg! Macro

```rust
#[test]
fn test_my_function() {
    let x = 5;
    dbg!(x);  // Prints to stderr
    let result = my_function(x);
    assert_eq!(result, expected);
}
```

### Step Through with Debugger

```bash
rust-gdb --args cargo test test_my_function
```

Or use LLDB on macOS:

```bash
lldb -- cargo test test_my_function
```

---

## CI/CD Testing

When you push a PR, GitHub Actions runs:

```bash
cargo nextest run --no-fail-fast --workspace --exclude command-signatures-v2
```

The `--exclude command-signatures-v2` flag excludes that crate (known to be slow in CI).

If CI fails:
1. Check the error message
2. Run locally: `cargo nextest run --workspace --no-fail-fast`
3. Fix the issue
4. Commit and push again

Use the `diagnose-ci-failures` skill if you need help understanding CI failures.

---

## Next Steps

- **[Build and Run](./BUILD_AND_RUN.md)** — Building Warp
- **[Coding Style](./CODING_STYLE.md)** — Style rules
- **[Debugging](./DEBUGGING.md)** — Debugging techniques
- **[warp-integration-test skill](./README.md)** — Writing integration tests

---

[👈 Back to For Developers](./README.md)
