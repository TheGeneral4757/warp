# First Time Setup

Welcome to Warp! Let's get you set up in a few minutes.

---

## Step 1: Choose Your Shell (1 min)

When you first launch Warp, it asks which shell you use.

**Supported shells:**
- **bash** — Traditional, widely available
- **zsh** — macOS default (since Catalina)
- **fish** — Friendly and interactive
- **powershell** — Windows default
- **elvish** — Modern, experimental
- **nu** — Modern, fast

If you're not sure, pick whatever shell you usually use. You can change it later in Settings.

**Tip:** On macOS with Apple Silicon, use zsh or fish for best performance.

---

## Step 2: Sign In to Warp Cloud (2 min, optional but recommended)

Warp Cloud lets you sync settings and commands across devices.

1. **Click "Sign In"** in the welcome screen
2. **Create account or sign in** with:
   - Email
   - GitHub
   - Google
3. **Authorize Warp** to access your account

Benefits of Cloud sync:
- ✅ Settings sync across devices
- ✅ Command history in the cloud
- ✅ Profile and theme sync
- ✅ Workspace collaboration (team features)

You can skip this and use Warp offline — just do this later in Settings if you change your mind.

---

## Step 3: Customize Keyboard Shortcuts (2 min, optional)

Warp uses keyboard shortcuts for everything. Common ones:

| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Ask AI for help | Cmd+K | Ctrl+K |
| Agent Mode | Cmd+Shift+K | Ctrl+Shift+K |
| Open Command Palette | Cmd+P | Ctrl+P |
| New Tab | Cmd+T | Ctrl+T |
| Fullscreen | Cmd+Ctrl+F | F11 |

To customize:

1. **Open Settings** (⚙️)
2. **Go to "Keyboard"**
3. **Click on a shortcut** to reassign it
4. **Save**

---

## Step 4: Choose Your AI Model (1 min)

Warp uses AI for command suggestions. Choose your preferred model:

1. **Open Settings** (⚙️)
2. **Go to "AI Models"**
3. **Select a model** (Warp's default is usually fine)
4. **Save**

Options:
- **Warp Recommended** — Balanced speed and quality
- **Claude 3.5 Sonnet** — Best reasoning
- **GPT-4o** — Great for code
- **Gemini 2.0 Flash** — Fastest
- **Custom** — Bring your own API key

👉 See [Selecting Models](../AI_MODELS/SELECTING_MODELS.md) for details.

---

## Step 5: Configure Agent Mode (Optional)

Agent Mode lets Warp autonomously complete complex tasks.

1. **Open Settings** (⚙️)
2. **Go to "Agent"** (or "AI" → "Agent Settings")
3. **Toggle permissions:**
   - Can read files?
   - Can execute commands?
   - Can write to disk?
4. **Save**

**Tip:** Start with read-only. Enable execution once you trust the model.

---

## Step 6: (Optional) Connect to Warp Server

If your team uses Warp Server for internal deployments:

1. **Open Settings** (⚙️)
2. **Go to "Server"** or **"Cloud"**
3. **Enter server URL** (ask your admin)
4. **Sign in**

Most users skip this — it's for team/enterprise setups.

---

## Customizing Your Experience

### Changing Your Shell (After Setup)

1. **Open Settings**
2. **Go to "Terminal"** → **"Shell"**
3. **Select a new shell**
4. **Save** and restart Warp

### Changing Your Theme

1. **Open Settings**
2. **Go to "Appearance"**
3. **Choose light or dark** theme
4. **Or pick a custom theme**

### Customizing Keyboard Shortcuts

1. **Open Settings**
2. **Go to "Keyboard"**
3. **Click a shortcut** to change it
4. **Save**

### Importing Your Shell Configuration

Warp reads your existing shell config (`.bashrc`, `.zshrc`, etc.):

- ✅ Your aliases work
- ✅ Your functions work
- ✅ Your environment variables load
- ✅ Your prompt is preserved (or you can customize in Warp)

No additional setup needed!

---

## Importing History

If you have command history from another terminal:

1. **Most shells share history automatically** — If you've used bash/zsh before, it's already there
2. **Warp reads your history file** — `.bash_history`, `.zsh_history`, etc.
3. **You can also import manually:**
   - Settings → History → Import
   - Select your history file

---

## Common First-Time Questions

**Q: Can I use Warp as my default terminal?**  
A: Yes! Set it as your default terminal in system preferences.

**Q: Do I need an account?**  
A: No, but Cloud sync requires one. You can use Warp offline.

**Q: Will Warp interfere with my shell config?**  
A: No. Warp reads your existing config but doesn't modify it. You can keep using your shell elsewhere.

**Q: How do I get help with a command?**  
A: Press Cmd+K (or Ctrl+K) and ask. Warp's AI will help.

**Q: Can I use custom AI models?**  
A: Yes! See [Bring Your Own API Key](../AI_MODELS/BRING_YOUR_OWN_API_KEY.md).

---

## Next Steps

1. **[QUICK_START](../QUICK_START.md)** — 5-minute intro to Warp
2. **[FAQ](../FAQ.md)** — Common questions
3. **[Selecting Models](../AI_MODELS/SELECTING_MODELS.md)** — Learn about AI models
4. **[Features](../FEATURES/)** — Explore Agent Mode, Cloud Sync, etc.

---

[👈 Back to Setup and Config](./README.md)
