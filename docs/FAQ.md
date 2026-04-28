# Frequently Asked Questions

Answers to common questions about Warp.

---

## Getting Started

### Q: Is Warp free?

**A:** Warp has a free tier with all core features. Premium features (like extended cloud sync, advanced AI models) may require a subscription. Check [warp.dev/pricing](https://warp.dev/pricing) for current plans.

### Q: What operating systems does Warp support?

**A:** 
- macOS (Intel and Apple Silicon)
- Linux (Ubuntu, Fedora, Arch, etc.)
- Windows 10/11
- WASM (experimental)

See [Installation](./SETUP_AND_CONFIG/INSTALLATION.md) for platform-specific details.

### Q: Can I use Warp alongside other terminals?

**A:** Yes! Warp doesn't interfere with other terminals. You can use Warp, iTerm2, Terminal, and others side-by-side.

### Q: Do I need a Warp Cloud account?

**A:** No. Warp works offline and locally. Cloud Sync is optional — it just lets you sync settings across devices.

---

## AI and Models

### Q: What AI models does Warp support?

**A:** Warp supports:
- **Anthropic** (Claude models)
- **OpenAI** (GPT models)
- **Google** (Gemini models)
- **AWS Bedrock** (multiple providers)
- **Open Router** (proxy service)

See [Supported Models](./AI_MODELS/SUPPORTED_MODELS.md) for a complete list.

### Q: Can I use my own API key?

**A:** Yes! See [Bring Your Own API Key](./AI_MODELS/BRING_YOUR_OWN_API_KEY.md) for setup instructions.

### Q: Is my data sent to Warp's servers?

**A:** No. Warp sends your prompts directly to the AI provider (Anthropic, OpenAI, etc.) with TLS encryption. Warp servers don't store your code or commands.

### Q: What model should I use?

**A:** 
- **For reasoning:** Claude 3.5 Sonnet
- **For speed:** Gemini 2.0 or Claude 3.5 Haiku
- **For coding:** Claude 3.5 Sonnet or GPT-4o
- **For budget:** Claude 3.5 Haiku or GPT-4o Mini

See [Selecting Models](./AI_MODELS/SELECTING_MODELS.md) for recommendations.

### Q: Can I use a local LLM?

**A:** Yes, via:
- **AWS Bedrock** — Self-hosted models
- **Open Router** — Proxy to local deployments
- **Custom endpoint** — If compatible with OpenAI API

---

## Features

### Q: What's the difference between Cmd+K and Cmd+Shift+K?

**A:** 
- **Cmd+K** — Terminal AI help (quick command suggestions)
- **Cmd+Shift+K** — Agent Mode (autonomous task execution)

See [Features Overview](./FEATURES/README.md).

### Q: Can I undo Agent Mode actions?

**A:** Yes:
1. Agent Mode always shows the plan first
2. You approve before any commands run
3. You can "Reject" the plan if it's wrong
4. If something did run, use Cmd+Z to undo in your shell

See [Agent Mode](./FEATURES/AGENT_MODE.md) for details.

### Q: How do I enable Cloud Sync?

**A:** 
1. Sign in to Warp Cloud (or create account)
2. Settings → Cloud → Toggle Cloud Sync
3. Your settings sync to other devices

See [Cloud Sync](./FEATURES/CLOUD_SYNC.md) for details.

---

## Configuration

### Q: How do I change my shell?

**A:** Settings → Terminal → Shell → Choose your shell.

Or set environment variable:
```bash
export WARP_SHELL=/bin/zsh
```

### Q: How do I customize keyboard shortcuts?

**A:** Settings → Keyboard → Click a shortcut to change it.

### Q: Where are my settings stored?

**A:**
- **Local:** `~/.config/warp/` (Linux) or `~/Library/Application Support/Warp/` (Mac)
- **Cloud:** Synced to Warp Cloud if enabled

### Q: How do I set environment variables for Warp?

**A:** See [Environment Variables](./SETUP_AND_CONFIG/ENVIRONMENT_VARIABLES.md).

---

## Permissions and Privacy

### Q: Can Agent Mode delete files?

**A:** Only if you enable the "Write files" permission AND approve the plan. You always review before anything happens.

### Q: Are my command history and files visible to Warp?

**A:** 
- **Local:** Stored on your machine only
- **Cloud Sync:** Only settings and workspace preferences (not history details)
- **API requests:** Only the specific text you submit to AI provider

### Q: How do I disable history tracking?

**A:** Settings → History → Choose what's tracked.

### Q: Are API keys stored securely?

**A:** Yes. API keys are stored in your system's secure keychain (not plaintext).

---

## Troubleshooting

### Q: Warp won't launch

**A:** 
1. Check system requirements — [Installation](./SETUP_AND_CONFIG/INSTALLATION.md)
2. Verify disk space (need 100MB+)
3. Check logs in `~/.local/share/warp/logs/`
4. Try reinstalling

### Q: AI model isn't responding

**A:**
1. Check internet connection
2. Verify API key is valid
3. Check provider status page
4. See [Troubleshooting Models](./AI_MODELS/TROUBLESHOOTING_MODELS.md)

### Q: Cloud Sync isn't working

**A:**
1. Check you're signed in
2. Check internet connection
3. Force sync: Settings → Cloud → Sync Now
4. Check logs for error messages

### Q: Command history is missing

**A:**
1. Warp reads your shell's history file (`.bash_history`, etc.)
2. Make sure you're using the same shell
3. Try: Settings → History → Import
4. Restart Warp

---

## Development

### Q: How do I build Warp from source?

**A:** See [Build and Run](./FOR_DEVELOPERS/BUILD_AND_RUN.md).

### Q: How do I run tests?

**A:** See [Testing Guide](./FOR_DEVELOPERS/TESTING_GUIDE.md).

### Q: What's the code style?

**A:** See [Coding Style](./FOR_DEVELOPERS/CODING_STYLE.md).

### Q: How do I contribute?

**A:** See [CONTRIBUTING.md](../CONTRIBUTING.md) in the repo.

---

## Performance

### Q: Why is Warp slow?

**A:**
1. **First launch:** Takes a moment to load
2. **Heavy terminal output:** Large outputs slow rendering
3. **Large history:** Many commands slow history search
4. **GPU issues:** Try disabling GPU:
   ```bash
   export WARP_DISABLE_GPU=1
   ```

See [Debugging](./FOR_DEVELOPERS/DEBUGGING.md) for profiling tips.

### Q: How much disk space does Warp use?

**A:** ~500MB installed + ~100MB for settings/history. Cloud Sync adds minimal overhead.

### Q: Can I limit command history size?

**A:** Yes. Settings → History → Configure retention.

---

## Integration

### Q: Does Warp work with my terminal multiplexer?

**A:** 
- **tmux** — Yes, but each tmux pane is a separate session
- **screen** — Similar to tmux
- **zellij** — Similar to tmux

Note: Terminal emulation features work per pane/window.

### Q: Can I use Warp as my default terminal?

**A:** Yes. macOS: System Preferences → Default Terminal. Windows: Windows Terminal → Settings.

### Q: Does Warp work with my shell config?

**A:** Yes. Warp reads your `.bashrc`, `.zshrc`, etc. and loads all aliases and functions.

---

## Billing and Licensing

### Q: How much does Warp cost?

**A:** Check [warp.dev/pricing](https://warp.dev/pricing) for current plans.

### Q: What's the license?

**A:** Warp uses AGPL-3.0 for the terminal and MIT for the UI framework. See [LICENSE](../LICENSE) in repo.

### Q: Can I use Warp commercially?

**A:** Yes, under the AGPL-3.0 license terms.

---

## Advanced Topics

### Q: Can I use Warp with SSH?

**A:** Partially. You can use Warp locally to connect via SSH, but Warp AI features work best locally (not over SSH).

### Q: Does Warp support custom completions?

**A:** Yes. Completions come from your shell config. Warp can enhance them with AI.

### Q: Can I use Warp in a Docker container?

**A:** Warp is a terminal app, not a command-line tool, so direct Docker use is limited. But you can use Warp to work with Docker.

### Q: How do I contribute to Warp development?

**A:** See [CONTRIBUTING.md](../CONTRIBUTING.md) and [Build and Run](./FOR_DEVELOPERS/BUILD_AND_RUN.md).

---

## Still Have Questions?

- **Search the docs** — Use your browser's find function
- **Check [Features](./FEATURES/)** — Overview of main capabilities
- **Check [AI Models](./AI_MODELS/)** — AI-specific questions
- **Check [WARP.md](../WARP.md)** — Full engineering guide
- **Open a GitHub issue** — Ask on [GitHub](https://github.com/warpdotdev/warp/issues)

---

[👈 Back to Docs](./README.md)
