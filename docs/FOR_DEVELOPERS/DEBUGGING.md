# Debugging

Techniques for debugging Warp during development.

---

## Setting Up Your Debug Environment

### Enable Debug Logging

```bash
export RUST_LOG=debug
cargo run
```

Or for specific modules:

```bash
export RUST_LOG=warp=debug,warpui=debug,warp_ai=debug
cargo run
```

### Verbose Output

```bash
export RUST_BACKTRACE=1
cargo run

# For full backtrace:
export RUST_BACKTRACE=full
cargo run
```

### Check Logs

Warp writes logs to:

- **macOS:** `~/Library/Application Support/Warp/logs/`
- **Linux:** `~/.local/share/warp/logs/`
- **Windows:** `%AppData%\Warp\logs\`

Tail logs in real-time:

```bash
# macOS
tail -f ~/Library/Application\ Support/Warp/logs/warp.log

# Linux
tail -f ~/.local/share/warp/logs/warp.log
```

---

## Common Debugging Scenarios

### Debugging a Feature

1. **Find the code**
   ```bash
   # Search for the feature name
   grep -r "my-feature" crates/ app/
   ```

2. **Enable logging**
   ```bash
   export RUST_LOG=warp=trace
   cargo run
   ```

3. **Use `println!` for quick debugging**
   ```rust
   fn my_feature(ctx: &mut AppContext) {
       println!("DEBUG: Entering my_feature");
       // your code
       println!("DEBUG: my_feature result: {:?}", result);
   }
   ```

4. **Run specific test**
   ```bash
   cargo nextest run -p my_crate -- my_test --nocapture
   ```

### Debugging AI Integration

1. **Check which model is active**
   ```bash
   export RUST_LOG=warp::ai=debug
   cargo run
   ```

2. **Verify API key is loaded**
   Check logs for API key validation messages.

3. **Test API call directly**
   ```bash
   # Anthropic
   curl https://api.anthropic.com/v1/messages \
     -H "x-api-key: $ANTHROPIC_API_KEY" \
     -H "content-type: application/json" \
     -d '{...}'
   ```

4. **Check profile settings**
   In-memory state: `AIExecutionProfilesModel::active_profiles_per_session`

### Debugging Terminal Emulation

1. **Check escape sequence handling**
   ```bash
   export RUST_LOG=warp::terminal=debug
   cargo run
   ```

2. **Verify output is correct**
   ```bash
   # Test that terminal renders properly
   cargo run --release  # Use release build for better performance
   ```

3. **Check for deadlocks**
   Search for `TerminalModel::lock()` calls — this is a common deadlock source.

### Debugging UI Issues

1. **Enable UI logging**
   ```bash
   export RUST_LOG=warpui=debug
   cargo run
   ```

2. **Check for mouse input issues**
   - Verify `MouseStateHandle` is created once in `new()`, not per-frame
   - Check if `ctx.resolve()` is finding the right view

3. **Verify view hierarchy**
   - Views should hold `ViewHandle<T>`, not direct ownership
   - Use `ctx.resolve(handle)` to get references

4. **Check rendering performance**
   - Add timing to `render()` method
   - Use release build for accurate measurements

---

## Using the Debugger

### LLDB (macOS)

```bash
# Build first
cargo build

# Start debugger
lldb target/debug/warp

# In debugger:
(lldb) breakpoint set --name my_function
(lldb) run
(lldb) continue
(lldb) p variable_name
```

### GDB (Linux)

```bash
# Build first
cargo build

# Start debugger
gdb target/debug/warp

# In debugger:
(gdb) break my_function
(gdb) run
(gdb) continue
(gdb) print variable_name
```

### VS Code Debugging (with CodeLLDB extension)

In `.vscode/launch.json`:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug Warp",
            "cargo": {
                "args": ["build", "--bin", "warp"],
                "filter": {
                    "name": "warp",
                    "kind": "bin"
                }
            },
            "args": [],
            "cwd": "${workspaceFolder}"
        }
    ]
}
```

---

## Debugging Common Problems

### "Tests hanging or freezing"

Usually means a UI test trying to render:

```bash
# Run library tests only (no UI)
cargo test --lib
```

**Fix:** UI tests need special setup. See `crates/integration/` for the proper pattern.

### "Deadlock or UI freeze"

Often caused by:

1. **Calling `TerminalModel::lock()` while already holding lock**
   - Search: `terminal.lock()`
   - Fix: Thread the locked reference down instead

2. **Creating `MouseStateHandle` per-frame**
   - Search: `MouseStateHandle::default()`
   - Fix: Create once in `new()`, reuse in `render()`

**Debug:**

```bash
# Check thread IDs in backtrace
rust-backtrace=full cargo run

# Look for mutex contention in logs
export RUST_LOG=warp=trace
```

### "Memory leak or high memory usage"

1. **Check for circular references** — Entity-Handle should prevent this
2. **Check for unbounded Vec growth** — Is a Vec growing without bounds?
3. **Use heaptrack (Linux) or Instruments (macOS)**
   ```bash
   heaptrack target/release/warp
   heaptrack_gui heaptrack.warp.*
   ```

### "Panic or unexpected crash"

```bash
export RUST_BACKTRACE=full
cargo run

# Look at the backtrace to find the panic location
```

Then add a test that reproduces the panic:

```rust
#[test]
#[should_panic]
fn test_crash_scenario() {
    // Code that panics
}
```

---

## Inspecting State

### Print Debug Information

```rust
// Standard Debug formatting
println!("{:?}", my_struct);

// Pretty Debug formatting
println!("{:#?}", my_struct);

// Using dbg! macro (goes to stderr)
dbg!(my_value);
```

### Conditional Logging

```rust
#[cfg(debug_assertions)]
println!("Debug only: {:?}", value);
```

### Feature Flag Debugging

```rust
if FeatureFlag::MyFeature.is_enabled() {
    println!("Feature is ENABLED");
} else {
    println!("Feature is DISABLED");
}
```

---

## Testing in Different Conditions

### Different Shell Types

```bash
# Test with bash
export WARP_SHELL=/bin/bash
cargo run

# Test with zsh
export WARP_SHELL=/bin/zsh
cargo run

# Test with fish
export WARP_SHELL=/usr/bin/fish
cargo run
```

### Different AI Models

```bash
# Test with custom model (need API key)
export ANTHROPIC_API_KEY=sk-ant-...
cargo run

# Then select model in Settings
```

### Offline Mode

```bash
# Simulate no network
export SERVER_ROOT_URL=http://localhost:9999  # Non-existent server
cargo run
```

---

## Performance Profiling

### Cargo Timings

See what takes time to compile:

```bash
cargo build -p warp --timings
# Opens target/cargo-timings.html
```

### Runtime Profiling (macOS)

```bash
# Run Warp
cargo run --release

# In another terminal, use Instruments app:
# Instruments > Sampling (to profile CPU)
# Instruments > System Trace (to see thread activity)
# Then attach to Warp process
```

### Flamegraph (Linux)

```bash
# Install flamegraph
cargo install flamegraph

# Run with profiling
cargo flamegraph --bin warp

# Opens flamegraph.svg
```

---

## Debugging Async Code

Warp uses async/tokio in many places:

```bash
# Enable tokio console (if compiled with tokio-console feature)
export TOKIO_CONSOLE=true
cargo run

# Then in another terminal:
tokio-console  # Connects to console
```

Or add logging to async functions:

```rust
async fn my_async_function() {
    println!("DEBUG: Starting async work");
    // work
    println!("DEBUG: Finished async work");
}
```

---

## Checking Code Correctness

### Clippy Warnings

```bash
cargo clippy --workspace --all-targets --all-features -- -D warnings
```

Clippy catches many bugs at compile time.

### Running Tests with Coverage

```bash
# Install tarpaulin (code coverage tool)
cargo install cargo-tarpaulin

# Run tests with coverage
cargo tarpaulin --out Html
```

---

## Next Steps

- **[Build and Run](./BUILD_AND_RUN.md)** — Building and running
- **[Testing Guide](./TESTING_GUIDE.md)** — Writing tests
- **[Coding Style](./CODING_STYLE.md)** — Code patterns
- **[WARP.md](../../WARP.md)** — Full engineering guide

---

[👈 Back to For Developers](./README.md)
