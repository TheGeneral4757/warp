# Feature Flags

Add, manage, and remove feature flags in Warp.

---

## What Are Feature Flags?

Feature flags let you **gate features behind a flag**, allowing you to:

- **Ship incomplete features safely** — Flag is off by default
- **Test with dogfood team first** — Enable for internal users before public
- **Gradually roll out** — Dogfood → Preview → Stable
- **A/B test** — Compare two implementations

---

## Quick Start

### 1. Add a Feature Flag

Add a variant to `FeatureFlag` enum in `crates/warp_core/src/features.rs`:

```rust
pub enum FeatureFlag {
    // ... existing flags
    #[serde(rename = "my-new-feature")]
    MyNewFeature,  // Your new flag
}
```

### 2. Implement Default Behavior

Add to the `is_enabled_by_default()` method:

```rust
fn is_enabled_by_default(&self) -> bool {
    match self {
        FeatureFlag::MyNewFeature => false,  // Disabled by default
        // ... existing defaults
    }
}
```

### 3. Gate Your Code

In your feature code, check the flag at runtime:

```rust
if FeatureFlag::MyNewFeature.is_enabled() {
    // New feature code
} else {
    // Old behavior
}
```

### 4. Promote When Ready

Once tested, promote from Dogfood → Preview → Stable:

```rust
// In is_enabled_by_default():
FeatureFlag::MyNewFeature => {
    matches!(channel, Channel::Dogfood | Channel::Preview)
}
```

Then, once fully stable:

```rust
// Remove the check; feature is always on
```

---

## Feature Flag Locations

All feature flags live in:

```
crates/warp_core/src/features.rs
```

Key components:
- `FeatureFlag` enum — All flags defined here
- `is_enabled_by_default()` — Default state per channel
- `Channel` enum — Dogfood, Preview, Stable

---

## Promotion Channels

Features progress through channels:

1. **Stable** — Production, on by default for all users
2. **Preview** — Pre-release, available to preview users who opt-in
3. **Dogfood** — Internal testing, on for Warp team only
4. **Disabled** — Off by default, no users

### Channel Values

In `is_enabled_by_default()`:

```rust
// Always on (stable)
FeatureFlag::MyFeature => true,

// Dogfood + Preview (testing)
FeatureFlag::Experimental => matches!(channel, Channel::Dogfood | Channel::Preview),

// Dogfood only (internal)
FeatureFlag::InternalOnly => channel == Channel::Dogfood,

// Always off (disabled)
FeatureFlag::Disabled => false,
```

---

## Using Feature Flags

### Check at Runtime

```rust
if FeatureFlag::MyNewFeature.is_enabled() {
    // Code only runs if flag is enabled
}
```

### In UI Code

Hide UI elements behind the flag:

```rust
if FeatureFlag::MyNewFeature.is_enabled() {
    view.new_ui_element()
}
```

### In Tests

```rust
#[test]
fn test_my_feature() {
    if !FeatureFlag::MyNewFeature.is_enabled() {
        // Skip test if flag is off
        return;
    }
    // Test code
}
```

### Never Use `#[cfg(...)]` for Optional Features

Avoid compile-time gates for optional features — use runtime checks instead:

❌ Bad:
```rust
#[cfg(feature = "my-feature")]
fn my_feature() { }
```

✅ Good:
```rust
fn my_feature() {
    if FeatureFlag::MyFeature.is_enabled() {
        // Code
    }
}
```

---

## Full Workflow: Add → Test → Promote → Remove

### Phase 1: Add the Flag

1. **Add variant** to `FeatureFlag` enum
2. **Set default to false** (disabled)
3. **Gate your code** with `if FeatureFlag::...is_enabled()`

```rust
// crates/warp_core/src/features.rs
pub enum FeatureFlag {
    #[serde(rename = "cool-new-command")]
    CoolNewCommand,
}

fn is_enabled_by_default(&self) -> bool {
    match self {
        FeatureFlag::CoolNewCommand => false,  // Disabled by default
    }
}
```

### Phase 2: Test with Dogfood

1. **Enable for dogfood** in `is_enabled_by_default()`:

```rust
FeatureFlag::CoolNewCommand => channel == Channel::Dogfood,
```

2. **Test internally** — Warp team uses the feature
3. **Fix bugs** based on feedback
4. **Create a PR** describing the flag and testing

Use the `add-feature-flag` skill to set this up properly.

### Phase 3: Preview Release

Once dogfood testing is solid:

1. **Enable for Preview** in `is_enabled_by_default()`:

```rust
FeatureFlag::CoolNewCommand => matches!(channel, Channel::Dogfood | Channel::Preview),
```

2. **Announce in changelog** — Feature is now in preview
3. **Gather user feedback**
4. **Fix issues**

### Phase 4: Stable Release

When the feature is production-ready:

1. **Enable by default** (always on):

```rust
FeatureFlag::CoolNewCommand => true,
```

2. **Remove the runtime check** from your code:

```rust
// Before:
if FeatureFlag::CoolNewCommand.is_enabled() {
    run_command();
}

// After:
run_command();  // No check needed, feature is stable
```

3. **Mark flag for removal** (see Phase 5)

### Phase 5: Remove the Flag

Once a feature has been stable for a while:

1. **Delete the variant** from `FeatureFlag` enum
2. **Remove from `is_enabled_by_default()`**
3. **Remove all gate checks** from code
4. **Keep code simple** — no more toggles

Use the `remove-feature-flag` skill to do this cleanly.

---

## Important Patterns

### Never Leave Flags Indefinitely

Feature flags add complexity. Set a timeline for removal:

- **Dogfood** → 2-4 weeks (internal testing)
- **Preview** → 2-4 weeks (user feedback)
- **Stable** → 2-4 weeks (optional, document deprecation)
- **Remove** → Clean up and delete

### Document Your Flag

In the enum:

```rust
/// New command interface — enables `/mycommand` syntax.
/// Status: Dogfood (2024-02-15 → 2024-03-15)
#[serde(rename = "my-command")]
MyCommand,
```

### Gate Both Feature AND UI

Gate the feature code AND hide the UI:

```rust
// In feature code:
if FeatureFlag::MyFeature.is_enabled() {
    handle_event();
}

// In UI code:
if FeatureFlag::MyFeature.is_enabled() {
    show_menu_item();
}
```

Don't gate just the UI without gating the code.

---

## Troubleshooting

### "Flag not taking effect"

1. **Check channel setting** — What's your build channel?
   - Debug builds default to Dogfood
   - Release builds default to Stable
2. **Verify the flag is enabled by default** for your channel in `is_enabled_by_default()`
3. **Check your code** — Did you add the `if` check correctly?

### "Flag exists but getting error"

1. **Compile Warp** — Make sure you compiled after adding the flag
2. **Restart Warp** — Some in-memory caches need refresh
3. **Check spelling** — Flag names are case-sensitive

### "Flag only works for dogfood"

By design! Check `is_enabled_by_default()`:

```rust
// Only dogfood
FeatureFlag::MyFeature => channel == Channel::Dogfood,

// Dogfood + Preview
FeatureFlag::MyFeature => matches!(channel, Channel::Dogfood | Channel::Preview),

// Everyone
FeatureFlag::MyFeature => true,
```

---

## Advanced: Custom Flag Values

Flags can have custom values beyond on/off:

```rust
pub enum FeatureValue {
    Version(u32),
    Threshold(f64),
    String(String),
}

// In your code:
match FeatureFlag::ConfigurableFeature.get_value() {
    Some(FeatureValue::Version(v)) => { /* use v */ },
    None => { /* default behavior */ },
}
```

Check `crates/warp_core/src/features.rs` for examples.

---

## Skills for Feature Flags

- **`add-feature-flag`** — Automated setup
- **`promote-feature`** — Move from Dogfood → Preview → Stable
- **`remove-feature-flag`** — Clean removal

Use these skills to handle the full lifecycle!

---

## Next Steps

- **[Build and Run](./BUILD_AND_RUN.md)** — Building with flags
- **[Coding Style](./CODING_STYLE.md)** — Code patterns
- **[add-feature-flag skill](./README.md)** — Add your first flag

---

[👈 Back to For Developers](./README.md)
