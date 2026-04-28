# AI System Architecture

Deep dive into how Warp's AI model system works.

---

## Overview

Warp's AI system is modular and extensible. You can:

- Switch between different AI providers (Anthropic, OpenAI, Google, AWS Bedrock)
- Use different models for different features (Agent Mode, Code Editing, CLI)
- Bring your own API keys
- Add new providers and models

---

## Key Components

### 1. LLM Selection (`app/src/ai/llms.rs`)

**File:** `app/src/ai/llms.rs` (1084 lines)

**Responsibility:** Manage available LLMs and user preferences

**Key Struct:** `LLMPreferences`

```rust
pub struct LLMPreferences {
    // Cached models from server
    models_by_feature: Arc<DashMap<String, Vec<LLMInfo>>>,
    // User's selected model per feature
    selected_model: Arc<DashMap<String, LLMId>>,
}
```

**Key Methods:**

- `LLMPreferences::global()` — Get singleton instance
- `get_active_base_model()` — Get the active model (with fallback to default)
- `fetch_models_from_server()` — Get available models from Warp Server
- `set_model()` — User selects a different model

**Important:** LLMs are fetched from **Warp Server**, not hardcoded. The server response includes:
- Model display name
- Base model identifier
- Cost/speed/quality specs
- Feature support (reasoning, computer use, etc.)

### 2. Execution Profiles (`app/src/ai/execution_profiles/mod.rs`)

**File:** `app/src/ai/execution_profiles/mod.rs`

**Responsibility:** Store model + permission settings per use case

**Key Struct:** `AIExecutionProfile`

```rust
pub struct AIExecutionProfile {
    pub base_model: Option<LLMId>,           // Agent Mode model
    pub coding_model: Option<LLMId>,         // Code editing model
    pub cli_agent_model: Option<LLMId>,      // CLI commands model
    pub computer_use_model: Option<LLMId>,   // Computer automation model
    pub permissions: AIPermissions,          // What the AI can do
}
```

**Key Struct:** `AIPermissions`

```rust
pub struct AIPermissions {
    pub can_read_files: bool,        // Can read from disk
    pub can_view_output: bool,       // Can see command output
    pub can_execute_commands: bool,  // Can run commands
    pub can_apply_diffs: bool,       // Can modify code
    pub can_write_files: bool,       // Can create/modify files
    pub mcp_permissions: MCPPermissions,  // Tool permissions
}
```

### 3. Profiles Model (`app/src/ai/execution_profiles/profiles.rs`)

**File:** `app/src/ai/execution_profiles/profiles.rs` (56K lines)

**Responsibility:** Manage all execution profiles and apply per-session overrides

**Key Struct:** `AIExecutionProfilesModel`

```rust
pub struct AIExecutionProfilesModel {
    // All available profiles
    profiles: Vec<AIExecutionProfile>,
    // Per-session model override (temp, doesn't persist)
    active_profiles_per_session: HashMap<SessionId, AIExecutionProfile>,
    // Per-terminal LLM override
    base_llm_for_terminal_view: HashMap<ViewHandle, LLMId>,
    // Cloud sync integration
    cloud_sync_manager: Arc<CloudSyncManager>,
}
```

**Key Methods:**

- `get_profile_for_session()` — Get effective profile (with overrides)
- `set_session_override()` — Temporarily change profile for a session
- `sync_profiles_to_cloud()` — Save to Warp Drive
- `load_profiles_from_cloud()` — Load from Warp Drive

**Profiles Persist Via:**
- **Local:** Settings in `~/.config/warp/` (platform-specific)
- **Cloud:** Warp Drive (synced across devices)

### 4. API Key Management (`crates/ai/src/api_keys.rs`)

**File:** `crates/ai/src/api_keys.rs`

**Responsibility:** Manage user-provided API keys securely

**Key Struct:** `ApiKeyManager`

```rust
pub struct ApiKeyManager {
    // Providers: google, anthropic, openai, open_router, aws
    keys: HashMap<String, SecureString>,
}
```

**Key Methods:**

- `set_api_key()` — Store a new API key (goes to system keychain)
- `get_api_key()` — Retrieve API key (from secure storage)
- `delete_api_key()` — Remove API key

**Storage:**
- **macOS:** Keychain
- **Linux:** Secret Service / KWallet
- **Windows:** Credential Manager
- **Fallback:** Encrypted local storage

---

## Model Selection Flow

Here's how a user's request gets routed to the right model:

```
1. User presses Cmd+K (ask AI for help)
   ↓
2. App determines feature: "CLI_AGENT"
   ↓
3. Look up profile for current session
   ↓
4. Check if there's a per-terminal override for base_llm
   ↓
5. Get model from profile.cli_agent_model
   ↓
6. If model is None, use default from server response
   ↓
7. Resolve to actual LLMId (e.g., "claude-3-5-sonnet")
   ↓
8. Send request to API provider
   ↓
9. Receive response, display to user
```

**Key insight:** Selection happens at runtime, not compile-time. This allows:
- Server to add new models without app update
- Users to switch models without restarting
- Per-session overrides (Agent Mode only)

---

## Adding a New LLM Provider

### Step 1: Extend LLM ID Enum

In `crates/ai/src/llm_id.rs`:

```rust
pub enum LLMId {
    AnthropicClaude3_5Sonnet,
    OpenAiGpt4o,
    GoogleGemini2_0,
    MyNewProvider,  // Add here
}
```

### Step 2: Add to Server Response Handling

In `app/src/ai/llms.rs`:

```rust
// When server sends model metadata:
match provider {
    "my-provider" => {
        // Parse and cache the model
    }
}
```

### Step 3: Add API Integration

In `crates/ai/src/` (or new crate):

Create request/response types compatible with the provider's API.

### Step 4: Update Settings UI

In `app/src/settings/` — add UI for configuring the new provider.

### Step 5: Update Docs

Add to [Supported Models](../AI_MODELS/SUPPORTED_MODELS.md).

---

## Cloud Sync Integration

Profiles sync to Warp Drive via `CloudSyncManager`:

```
Local Profile Change
    ↓
AIExecutionProfilesModel::set_profile()
    ↓
cloud_sync_manager.sync()
    ↓
Upload to Warp Drive
    ↓
Other devices receive update
    ↓
Local profiles updated
```

Syncing happens:
- On profile change
- On app startup (check for updates)
- Periodically (polling interval)

---

## Requesting Custom Models

If a model doesn't exist in Warp:

1. **Bring Your Own API Key** — Use your own key ([guide](../AI_MODELS/BRING_YOUR_OWN_API_KEY.md))
2. **Use AWS Bedrock** — Route through Bedrock
3. **Use Open Router** — Proxy for multiple providers

---

## Key Data Structures

### LLMId

```rust
pub struct LLMId {
    pub provider: String,      // "anthropic", "openai", etc.
    pub model_name: String,    // "claude-3-5-sonnet", "gpt-4o", etc.
}
```

### LLMInfo

```rust
pub struct LLMInfo {
    pub id: LLMId,
    pub display_name: String,  // "Claude 3.5 Sonnet"
    pub specs: LLMSpec,
}

pub struct LLMSpec {
    pub cost: Cost,            // Input/output token cost
    pub speed: u32,            // 1-100 (lower = faster)
    pub quality: u32,          // 1-100 (higher = better)
    pub reasoning: bool,       // Supports extended thinking
    pub computer_use: bool,    // Can control computer
}
```

---

## File Structure

```
app/src/ai/
├── llms.rs                    # LLM selection & preferences
├── cloud_agent_settings.rs    # Cloud agent settings
├── execution_profiles/
│   ├── mod.rs                # Profile struct
│   ├── profiles.rs           # Profile management (56K lines!)
│   └── permissions.rs        # Permission handling
└── ...

crates/ai/src/
├── lib.rs
├── llm_id.rs                 # LLMId type
├── api_keys.rs               # API key management
├── providers/
│   ├── anthropic.rs
│   ├── openai.rs
│   ├── google.rs
│   └── bedrock.rs
└── ...
```

---

## Testing AI Integration

### Unit Tests

```rust
#[test]
fn test_model_selection() {
    let prefs = LLMPreferences::global();
    let model = prefs.get_active_base_model();
    assert!(model.is_some());
}
```

### Integration Tests

Test full flow: Settings UI → Model change → API call → Response

See `crates/integration/tests/` for examples.

---

## Performance Considerations

### Model Caching

Models are cached in memory after first fetch. If server adds new models:
- Warp detects change (version check)
- Fetches updated list
- Updates in-memory cache
- No restart required

### API Key Caching

API keys are cached in memory after first use. Changes take effect:
- On Settings save
- On Warp restart
- After manual refresh (if implemented)

---

## Security Notes

- **API keys stored securely** — System keychain, not plaintext
- **No API key in logs** — Keys are redacted
- **HTTPS only** — All API calls use TLS
- **Per-session isolation** — Session overrides don't affect other sessions

---

## Debugging

### Enable AI Debug Logging

```bash
export RUST_LOG=warp::ai=debug
cargo run
```

### Check Active Model

In UI settings or via debugging:
- Active model for each feature
- Current provider
- API key status

### Inspect Cached Models

```rust
let prefs = LLMPreferences::global();
let models = prefs.get_all_models();
for model in models {
    println!("{:?}", model);
}
```

---

## Future Extensions

Potential improvements:

- **Model chaining** — Use different models for different stages of a task
- **Streaming responses** — Partial responses as they arrive
- **Model evaluation** — Compare model quality metrics
- **Custom fine-tuned models** — User-specific trained models
- **Open-source models** — Local LLM integration

---

## Related Files

See also:

- **User docs:** [AI Models](../AI_MODELS/README.md)
- **API Key setup:** [Bring Your Own API Key](../AI_MODELS/BRING_YOUR_OWN_API_KEY.md)
- **Profiles:** [Execution Profiles](../AI_MODELS/EXECUTION_PROFILES.md)
- **WARP.md:** Full engineering guide

---

## Next Steps

- **[Build and Run](./BUILD_AND_RUN.md)** — Set up development environment
- **[Debugging](./DEBUGGING.md)** — Debug AI features
- **[add-telemetry skill](./README.md)** — Track AI usage

---

[👈 Back to For Developers](./README.md)
