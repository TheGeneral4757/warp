# Environment Variables

Configure Warp using environment variables.

---

## What Are Environment Variables?

Environment variables are settings you can set in your shell that Warp reads at startup. Instead of clicking Settings, you can configure Warp via the command line.

---

## AI Model Configuration

### Using a Custom API Key

Set environment variables to use your own API keys:

```bash
# Anthropic (Claude)
export ANTHROPIC_API_KEY=sk-ant-...

# OpenAI
export OPENAI_API_KEY=sk-...

# Google Gemini
export GOOGLE_API_KEY=...
```

Then restart Warp and the keys will be automatically loaded.

### Using AWS Bedrock

```bash
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
export AWS_REGION=us-east-1
```

### Using Open Router

```bash
export OPEN_ROUTER_API_KEY=...
```

---

## Server Configuration

### Custom Warp Server

If using a custom Warp Server deployment:

```bash
export SERVER_ROOT_URL=http://localhost:8080
export WS_SERVER_URL=ws://localhost:8080/graphql/v2
```

This is useful for:
- Local development
- Team/enterprise deployments
- Offline usage

---

## Development & Debugging

### Enable Debug Logging

```bash
export RUST_LOG=debug
```

Or for specific modules:

```bash
export RUST_LOG=warp=debug,warpui=debug
```

### Verbose Output

```bash
export RUST_BACKTRACE=1  # Better error messages
export RUST_BACKTRACE=full  # Complete backtrace
```

---

## Shell Integration

### Custom Shell Path

If Warp can't find your shell automatically:

```bash
export WARP_SHELL=/usr/local/bin/zsh
```

### Custom Shell Config

Override the config file Warp reads:

```bash
export WARP_SHELL_CONFIG=~/.config/zsh/.zshrc
```

---

## Workspace Configuration

### Default Workspace

```bash
export WARP_WORKSPACE_ID=workspace_123
```

### Default Workdir

```bash
export WARP_DEFAULT_WORKDIR=/path/to/my/projects
```

---

## UI and Display

### Disable GPU Acceleration (if having rendering issues)

```bash
export WARP_DISABLE_GPU=1
```

### Force Software Rendering

```bash
export WARP_RENDERER=software
```

### Custom Theme

```bash
export WARP_THEME=dark
```

---

## Performance Tuning

### Increase Cache Size

```bash
export WARP_CACHE_SIZE=1000  # MB
```

### Adjust Buffer Size

```bash
export WARP_BUFFER_SIZE=10000  # Lines
```

---

## Setting Environment Variables Permanently

### macOS / Linux (bash)

Add to `~/.bashrc`:

```bash
export ANTHROPIC_API_KEY=sk-ant-...
export WARP_WORKSPACE_ID=workspace_123
```

Then reload:

```bash
source ~/.bashrc
```

### macOS (zsh)

Add to `~/.zshrc`:

```bash
export ANTHROPIC_API_KEY=sk-ant-...
```

Then reload:

```bash
source ~/.zshrc
```

### Windows (PowerShell)

```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "sk-ant-...", "User")
```

Then restart PowerShell.

### Windows (cmd)

```cmd
setx ANTHROPIC_API_KEY sk-ant-...
```

Then restart cmd.

---

## Checking Current Environment Variables

### macOS / Linux

```bash
env | grep WARP
env | grep ANTHROPIC_API_KEY
```

### Windows (PowerShell)

```powershell
Get-ChildItem env:WARP*
Get-ChildItem env:ANTHROPIC_API_KEY
```

---

## Common Configurations

### Developer Setup

```bash
export RUST_LOG=debug
export WARP_RENDERER=software  # If GPU issues
export SERVER_ROOT_URL=http://localhost:8080
export WS_SERVER_URL=ws://localhost:8080/graphql/v2
```

### Enterprise Setup

```bash
export SERVER_ROOT_URL=https://warp.company.internal
export WS_SERVER_URL=wss://warp.company.internal/graphql/v2
export WARP_WORKSPACE_ID=company_workspace
```

### Custom Model Setup

```bash
export ANTHROPIC_API_KEY=sk-ant-...
export OPENAI_API_KEY=sk-...
export GOOGLE_API_KEY=...
```

---

## Troubleshooting

### "Environment variable not being read"

1. **Verify it's set:** `echo $YOUR_VARIABLE`
2. **Check you used export:** `export MY_VAR=value` (not just `MY_VAR=value`)
3. **Reload shell:** `source ~/.bashrc` (or equivalent)
4. **Restart Warp** — Environment variables are read at startup

### "API key still not working"

1. **Verify the key is correct:** Copy from your provider's console
2. **Check for typos or spaces**
3. **Try removing the env var and using Settings UI instead**
4. **Check logs:** Warp logs location:
   - Mac: `~/Library/Application Support/Warp/logs/`
   - Linux: `~/.local/share/warp/logs/`
   - Windows: `%AppData%\Warp\logs\`

---

## Next Steps

- **[Bring Your Own API Key](../AI_MODELS/BRING_YOUR_OWN_API_KEY.md)** — More API key details
- **[Installation](./INSTALLATION.md)** — Back to setup
- **[Back to Setup and Config](./README.md)**
