# Execution Profiles

Understand how Warp uses different models for different tasks, and how to customize permissions.

---

## What Are Execution Profiles?

An **execution profile** is a set of AI settings that applies to a specific context. It defines:

- Which AI model to use
- What permissions the AI has (read files, run commands, etc.)
- Whether the model can make autonomous decisions (Agent Mode)

---

## Default Profile Types

Warp provides built-in profiles optimized for different use cases:

### 1. **Default (Cloud)**
- Used for most interactions
- Model selected by Warp or you
- Permissions: Read files, read output, cannot write or execute
- Autonomy: Limited (asks before major actions)

### 2. **Agent Profile**
- Used for **Agent Mode** (Cmd+Shift+K)
- Full reasoning and planning
- Permissions: Can read, execute, and write (configurable)
- Autonomy: High (can make autonomous decisions)

### 3. **Code Editing**
- Used for code suggestions and edits
- Focused on code quality
- Permissions: Can suggest code, apply diffs
- Autonomy: Low (always asks before applying changes)

### 4. **CLI Agent**
- Used for command help (Cmd+K)
- Quick command suggestions
- Permissions: Read-only by default
- Autonomy: Low (shows suggestions, doesn't execute)

### 5. **Computer Use** (when available)
- Used for automation
- Can take control of your screen/keyboard
- Permissions: Full control (configurable)
- Autonomy: High

---

## Customizing Profiles

### Change the Model for a Profile

1. **Open Settings** → **AI Models**
2. **Select a feature** (Agent Mode, Code Editing, etc.)
3. **Choose a different model** from the dropdown
4. **Click Save**

### Change Permissions for a Profile

1. **Open Settings** → **AI Models** (or **Agent Settings** for Agent Mode)
2. **Look for permission toggles:**
   - ✅ Can read files
   - ✅ Can view command output
   - ✅ Can execute commands
   - ✅ Can apply code diffs
   - ✅ Can write to disk
3. **Toggle permissions** as needed
4. **Click Save**

---

## Permission Types Explained

### Read Files
If enabled, the AI can read files from your system to provide context.

**Safe?** Yes — read-only, no data modified.

### View Command Output
If enabled, the AI can see the output of commands you run.

**Safe?** Yes — helps the AI understand what happened.

### Execute Commands
If enabled, the AI can run commands automatically (Agent Mode only).

**Safe?** ⚠️ Use with caution! Only enable if you trust the model and task is well-defined.

### Apply Code Diffs
If enabled, the AI can automatically apply code changes.

**Safe?** ⚠️ Enable for trusted models. You can review before committing.

### Write to Disk
If enabled, the AI can create and modify files.

**Safe?** ⚠️ Review changes before committing. Not recommended for untrusted models.

---

## Per-Session Overrides

You can override the profile for a specific session:

1. **In Agent Mode** (Cmd+Shift+K) — Click "⚙️ Settings" to adjust permissions for this session only
2. **Changes don't persist** — They only apply to this session
3. **Reset** by closing and reopening Agent Mode

---

## Advanced: Workspace Profiles

If you're in a workspace with multiple team members:

- **Admins** can enforce profiles for everyone
- **Admins** can restrict certain models or features
- **Your overrides** still work at the session level

Check with your workspace admin if you can't change profile settings.

---

## Common Profile Configurations

### For Casual Users
- **Agent Mode:** Use Warp's recommended model
- **Permissions:** All disabled (Agent asks before doing anything)
- **Result:** Safe, requires approval for each action

### For Power Users
- **Agent Mode:** Claude 3.5 Sonnet
- **Permissions:** Read files ✅, execute ✅, write ✅
- **Result:** Fast and autonomous, requires trust in the model

### For Code-Heavy Work
- **Code Editing:** Claude 3.5 Sonnet or GPT-4o
- **Permissions:** Can read files ✅, apply diffs ✅
- **Result:** High-quality code suggestions

### For Security-Conscious
- **Agent Mode:** Disable execute, disable write
- **Permissions:** Read-only
- **Result:** AI can suggest, but you approve everything

---

## Testing Permissions

Before enabling full permissions:

1. **Start with read-only** — Test that the model understands your task
2. **Enable execute** — Test that it runs correct commands
3. **Enable write** — Test that it creates files correctly
4. **Full autonomy** — Only once you trust the results

---

## Troubleshooting Profiles

**"I can't change the permissions"**

→ Your workspace admin may have locked these settings. Ask your admin.

**"The model isn't using my permissions"**

→ The model may not support that permission. Try a different model or check [Supported Models](./SUPPORTED_MODELS.md).

**"The AI did something unexpected"**

→ Check which profile was active. You may have been in Agent Mode instead of regular mode.

---

## For Developers

See [AI System Architecture](../FOR_DEVELOPERS/AI_SYSTEM_ARCHITECTURE.md) for technical details about how profiles work internally.

Source code: `app/src/ai/execution_profiles/` directory.

---

## Next Steps

- **[Selecting Models](./SELECTING_MODELS.md)** — How to switch models
- **[Troubleshooting Models](./TROUBLESHOOTING_MODELS.md)** — Getting help
- **[Back to AI Models](./README.md)**
