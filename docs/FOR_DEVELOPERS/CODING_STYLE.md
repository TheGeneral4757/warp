# Coding Style and Conventions

Style rules and patterns for Warp code.

---

## Format and Lint

All code must pass these checks before PR submission:

```bash
cargo fmt                                          # Format code
cargo clippy --workspace --all-targets --all-features --tests -- -D warnings
./script/run-clang-format.py -r --extensions 'c,h,cpp,m' ./crates/warpui/src/ ./app/src/
find . -name "*.wgsl" -exec wgslfmt --check {} +  # WGSL shaders
```

Run presubmit to check everything:

```bash
./script/presubmit
```

---

## Rust Style Rules

### Inline Format Arguments

Use inline format arguments (clippy rule `uninlined_format_args`):

✅ Good:
```rust
format!("{x}")
println!("{value}");
```

❌ Bad:
```rust
format!("{}", x)
println!("{}", value);
```

### No Unused Parameters

Delete unused parameters and update call sites:

✅ Good:
```rust
fn process_data(data: &str) -> String {
    // Uses `data`
    data.to_uppercase()
}
```

❌ Bad:
```rust
fn process_data(data: &str, _unused: i32) -> String {
    // Doesn't use `unused`
    data.to_uppercase()
}
```

### Function Parameter Order

Context parameter (`ctx`) goes **last**, except when there's a closure parameter (closure goes last, ctx second-to-last):

✅ Good:
```rust
fn render(x: i32, y: i32, ctx: &mut AppContext) { }

fn on_event(|event| { }, ctx: &mut AppContext) { }
```

❌ Bad:
```rust
fn render(ctx: &mut AppContext, x: i32, y: i32) { }
```

### Exhaustive Pattern Matching

Avoid wildcard `_ =>` patterns over enums you control:

✅ Good:
```rust
match state {
    State::Active => { }
    State::Inactive => { }
    State::Error => { }
}
// Compiler catches if you add a new variant
```

❌ Bad:
```rust
match state {
    State::Active => { }
    _ => { }  // Silently ignores new variants
}
```

### Type Annotations

Avoid redundant type annotations, especially in closures:

✅ Good:
```rust
let items: Vec<Item> = vec![];
let filtered = items.iter().filter(|item| item.active);
```

❌ Bad:
```rust
let items: Vec<Item> = Vec::<Item>::new();
let filtered = items.iter().filter(|item: &Item| item.active);
```

### Imports

Prefer specific imports over long path qualifiers, except in `cfg`-guarded scopes:

✅ Good:
```rust
use warpui::prelude::*;
use app::ai::models::Model;

let model = Model::new();
```

❌ Bad:
```rust
let model = app::ai::models::Model::new();
```

---

## Comments

### When to Add Comments

Only add comments when the **WHY** is non-obvious:

✅ Good:
```rust
// Terminal model uses a re-entrant lock; calling lock() if we already hold
// the lock causes a deadlock and UI freeze. Pass a reference down instead.
if let Ok(lock) = terminal.lock() { }
```

✅ Good:
```rust
// Workaround for https://github.com/issue/1234 — this can be removed
// when the upstream issue is fixed.
```

❌ Bad:
```rust
// Set x to 5
let x = 5;

// Check if x is greater than 0
if x > 0 { }
```

### Don't Reference Current Task/PR

Avoid comments tied to specific PRs or issues:

❌ Bad:
```rust
// Added for #1234 — can be removed when PR merges
// Used by the OnboardingFlow (GH-1066)
```

These belong in the PR description and rot as code evolves.

### Preserve Existing Comments

Don't strip comments in unrelated changes. Only update if the referenced logic changed:

✅ Good (preserving):
```rust
// Handler for keyboard input events
fn on_key_press(key: Key, ctx: &mut AppContext) { }
```

✅ Good (updating):
```rust
// Previously: "Handler for keyboard input events"
// Now: "Handler for keyboard and mouse input events"
fn on_input(event: InputEvent, ctx: &mut AppContext) { }
```

---

## Architecture Patterns

### Entity-Handle Pattern (UI)

Views never own each other directly — they hold handles and resolve via context:

✅ Good:
```rust
struct MyView {
    child_handle: ViewHandle<ChildView>,  // Handle, not direct ownership
}

fn render(&self, ctx: &ViewContext) {
    let child = ctx.resolve(self.child_handle);  // Resolve when needed
}
```

❌ Bad:
```rust
struct MyView {
    child: Box<ChildView>,  // Direct ownership
}
```

### MouseStateHandle (Critical)

Create **once during construction**, not per-frame:

✅ Good:
```rust
struct MyView {
    mouse_state: MouseStateHandle,  // Created once
}

impl MyView {
    fn new() -> Self {
        Self {
            mouse_state: MouseStateHandle::default(),  // Create once
        }
    }
    
    fn render(&self, ctx: &ViewContext) {
        // Reuse the same handle
    }
}
```

❌ Bad:
```rust
fn render(&self, ctx: &ViewContext) {
    let mouse = MouseStateHandle::default();  // BREAKS mouse input!
}
```

### Terminal Model Locking

Never call `TerminalModel::lock()` if anything up the call stack might hold the lock:

✅ Good:
```rust
fn handler(terminal: &TerminalModel, ctx: &mut AppContext) {
    let lock = terminal.lock().unwrap();
    // Use lock
}

fn caller(ctx: &mut AppContext) {
    handler(terminal, ctx);  // Lock is acquired and released properly
}
```

❌ Bad:
```rust
fn outer(terminal: &TerminalModel, ctx: &mut AppContext) {
    let _lock = terminal.lock();
    inner(terminal, ctx);  // Tries to acquire lock again → DEADLOCK
}

fn inner(terminal: &TerminalModel, ctx: &mut AppContext) {
    terminal.lock();  // Deadlock!
}
```

---

## Refactoring Guidelines

### Don't Over-Abstract

Three similar lines of code is better than a premature abstraction. Only refactor when there's clear duplication:

✅ Good (3 similar lines):
```rust
let x = input.parse::<i32>()?;
let y = input.parse::<i32>()?;
let z = input.parse::<i32>()?;
```

→ Extract only if you find yourself writing this 5+ times.

### Don't Add Error Handling for Impossible Cases

Trust internal code and framework guarantees:

✅ Good:
```rust
// Compiler ensures ModelId is valid
let model = models[id.index()];  // Direct indexing, no bounds check
```

❌ Bad:
```rust
// Unnecessary error handling for guaranteed-valid index
let model = models.get(id.index()).ok_or(Error::InvalidIndex)?;
```

### Only Validate at System Boundaries

Validate **user input** and **external APIs**, not internal data:

✅ Good:
```rust
// Validate user input
pub fn set_model(name: &str) -> Result<Model> {
    Model::parse(name)?  // Validates
}

// Trust internal code
fn process_model(model: &Model) {
    // Don't re-validate; Model is already valid
}
```

---

## Naming Conventions

### Function Parameters

Context parameter is always named `ctx`:

✅ Good:
```rust
fn render(name: &str, ctx: &mut AppContext) { }
fn handle_event(event: InputEvent, ctx: &mut ViewContext) { }
```

### Variable Naming

Use clear, descriptive names. Abbreviations only for common patterns:

✅ Good:
```rust
let active_models = models.filter(|m| m.is_active());
let ctx = app.context();  // `ctx` is standard for context
```

❌ Bad:
```rust
let am = models.filter(|m| m.is_active());  // Unclear abbreviation
let c = app.context();  // Non-standard
```

---

## Feature Flags

Use feature flags for compile-time gates, especially for platform-specific code:

```rust
#[cfg(target_os = "macos")]
fn platform_specific_fn() { }

// Prefer runtime feature flags for optional features
if FeatureFlag::MyFeature.is_enabled() {
    // Feature code
}
```

See [Feature Flags](./FEATURE_FLAGS.md) for detailed guidance.

---

## Clippy Warnings

All clippy warnings are treated as errors (`-D warnings`). Common issues:

- `uninlined_format_args` → Use `format!("{x}")` not `format!("{}", x)`
- `unused_variables` → Delete unused variables or prefix with `_`
- `missing_docs` → Add doc comments to public items
- `todo!()` → Replace with implementation or proper error

---

## Cross-Platform Considerations

Code can run on macOS, Linux, Windows, and WASM. Be careful with:

- **Platform-specific imports** — Use `cfg` attributes
- **File paths** — Use `Path` from `std::path`, not string concatenation
- **Environment variables** — Different behavior across platforms
- **Terminal escapes** — Not supported in all contexts

Example:

```rust
#[cfg(target_os = "macos")]
mod macos_specific {
    // macOS-only code
}

#[cfg(not(target_os = "windows"))]
fn unix_only() { }
```

---

## Further Reading

- **[WARP.md](../../WARP.md)** — Full engineering guide (source of truth)
- **[Rust Book](https://doc.rust-lang.org/book/)** — Rust fundamentals
- **[Clippy Docs](https://doc.rust-lang.org/clippy/)** — Lint rules

---

## Next Steps

- **[Build and Run](./BUILD_AND_RUN.md)** — Building Warp
- **[Testing Guide](./TESTING_GUIDE.md)** — Writing tests
- **[Debugging](./DEBUGGING.md)** — Debugging techniques

---

[👈 Back to For Developers](./README.md)
