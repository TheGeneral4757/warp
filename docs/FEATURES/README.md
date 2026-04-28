# Warp Features

Learn how to use Warp's main AI-powered features.

---

## Overview

Warp has several AI-powered features to help you work faster in the terminal.

---

## Main Features

### 1. [Agent Mode](./AGENT_MODE.md)
Let Warp autonomously complete complex tasks.

- **Shortcut:** Cmd+Shift+K (Mac) or Ctrl+Shift+K (Windows/Linux)
- **Use case:** "Sync my dotfiles", "Backup my project", "Find all Python files modified today"
- **Permissions:** Can read, execute, and write (configurable)
- **Learn more:** [Agent Mode Guide](./AGENT_MODE.md)

### 2. [Code Editing](./CODE_EDITING.md)
AI-assisted code editing in your editor.

- **Use case:** Code suggestions, refactoring, completing functions
- **Works with:** Most editors (VS Code, JetBrains, etc.) via Warp integration
- **Learn more:** [Code Editing Guide](./CODE_EDITING.md)

### 3. [Terminal AI Help](./TERMINAL_AI.md)
Ask for command help right in the terminal.

- **Shortcut:** Cmd+K (Mac) or Ctrl+K (Windows/Linux)
- **Use case:** "How do I find all .gitignore files?", "List files by size"
- **Permissions:** Read-only by default
- **Learn more:** [Terminal AI Guide](./TERMINAL_AI.md)

### 4. [Cloud Sync](./CLOUD_SYNC.md)
Sync your settings, history, and profiles across devices.

- **Account:** Requires Warp Cloud account
- **Use case:** Same settings on laptop, desktop, and servers
- **Learn more:** [Cloud Sync Guide](./CLOUD_SYNC.md)

---

## Quick Comparison

| Feature | Shortcut | What It Does | Best For |
|---------|----------|--------------|----------|
| **Agent Mode** | Cmd/Ctrl+Shift+K | Autonomously complete tasks | Complex multi-step tasks |
| **Terminal AI** | Cmd/Ctrl+K | Suggest commands | Quick command help |
| **Code Editing** | In editor | AI code improvements | Code suggestions |
| **Cloud Sync** | Settings | Sync across devices | Multi-device setup |

---

## Getting Started

### 1. Understand Agent Mode (2 min)
[Agent Mode Guide](./AGENT_MODE.md) — What it is, how to use it, how to control it.

### 2. Try Terminal AI (1 min)
Press **Cmd+K** and ask for help. E.g., "How do I list files by modification date?"

### 3. Set Up Cloud Sync (2 min)
[Cloud Sync Guide](./CLOUD_SYNC.md) — Sync settings across your devices.

### 4. Explore Code Editing (Optional)
[Code Editing Guide](./CODE_EDITING.md) — If you use Warp with your editor.

---

## Common Tasks

| I want to... | Use this feature |
|-------------|-----------------|
| Ask Warp to run a complex command | [Agent Mode](./AGENT_MODE.md) |
| Get help with a command | [Terminal AI](./TERMINAL_AI.md) |
| Suggest code improvements | [Code Editing](./CODE_EDITING.md) |
| Use same settings everywhere | [Cloud Sync](./CLOUD_SYNC.md) |

---

## AI Models and Features

Each feature can use different AI models. Choose which model to use:

1. **Open Settings** (⚙️)
2. **Go to "AI Models"**
3. **Select a model per feature**
4. **Save**

Learn more: [Selecting Models](../AI_MODELS/SELECTING_MODELS.md)

---

## Permissions and Safety

Each feature has configurable permissions:

- **Can read files?** — Access to your files for context
- **Can execute commands?** — Run commands (Agent Mode only)
- **Can write files?** — Create/modify files (Agent Mode only)

You control what each feature can do. Start with safe defaults (read-only), then enable as you trust the model.

Learn more: [Execution Profiles](../AI_MODELS/EXECUTION_PROFILES.md)

---

## Privacy

- **On-device:** Command history stays on your device (unless Cloud Sync enabled)
- **API requests:** Requests go to your configured AI provider
- **Cloud sync:** Synced data is encrypted in transit and at rest
- **API keys:** Stored securely in your system keychain

---

## Tips for Best Results

1. **Be specific** — "Show me Python files larger than 100KB" works better than "Show me files"
2. **Give context** — Include error messages, recent changes, etc.
3. **Break down complex tasks** — Easier for AI to handle smaller steps
4. **Review before executing** — Always check what Agent Mode will do before approving
5. **Use appropriate models** — Claude for reasoning, Gemini for speed

---

## Troubleshooting

**Feature not working?**
- Check [AI_MODELS Troubleshooting](../AI_MODELS/TROUBLESHOOTING_MODELS.md)
- Check [FAQ](../FAQ.md)

---

## Next Steps

- **[QUICK_START](../QUICK_START.md)** — 5-minute introduction
- **[Agent Mode](./AGENT_MODE.md)** — Deep dive into autonomous tasks
- **[Selecting Models](../AI_MODELS/SELECTING_MODELS.md)** — Choose your AI model
- **[FAQ](../FAQ.md)** — Common questions

---

[👈 Back to Docs](../README.md)
