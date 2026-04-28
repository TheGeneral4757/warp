# Agent Mode

Let Warp autonomously complete complex tasks for you.

---

## What Is Agent Mode?

Agent Mode is Warp's **autonomous task completion** feature. Instead of running one command, you describe a goal and Warp:

1. **Understands your goal** — Parses your request
2. **Plans steps** — Figures out what needs to happen
3. **Executes** — Runs commands, reads output, adjusts
4. **Completes** — Achieves your goal or explains what went wrong

---

## How to Use Agent Mode

### Launch Agent Mode

**Mac:** Press **Cmd+Shift+K**  
**Windows/Linux:** Press **Ctrl+Shift+K**

### Ask for Something

Type your goal:

```
Sync my dotfiles to ~/.config/
```

```
Find all Python files modified in the last week and count them
```

```
Check if my projects have passing tests
```

### Review and Approve

Warp shows you what it will do:

1. **Steps it will take** — Read the plan
2. **Commands to run** — Review before approving
3. **Approveor Edit** — Let it continue or modify the plan

### Wait for Results

Warp executes the plan and shows you results.

---

## What Agent Mode Is Good For

✅ **Multi-step tasks:**
- "Back up my projects to AWS S3"
- "Set up a Python virtual environment with dependencies"
- "Find all config files and show diffs"

✅ **Complex analysis:**
- "Find all services consuming >50% CPU"
- "Check which repos have uncommitted changes"
- "List all files modified since yesterday"

✅ **Automation:**
- "Deploy my app to production"
- "Renew SSL certificates"
- "Archive old log files"

---

## What Agent Mode Is NOT Good For

❌ **Destructive operations:** Be careful with delete, rm, etc.
❌ **Production changes:** Review thoroughly before approving
❌ **Sensitive data:** Don't ask Agent Mode to handle passwords/secrets
❌ **One-line commands:** Use regular Cmd+K for quick commands

---

## Configuring Agent Mode

### Enable/Disable Permissions

Agent Mode can do different things based on permissions:

1. **Open Settings** (⚙️)
2. **Go to "Agent Mode"** or **"AI" → "Agent Settings"**
3. **Toggle permissions:**
   - ✅ Can read files
   - ✅ Can execute commands
   - ✅ Can write to disk
   - ✅ Can apply code changes
4. **Save**

### Changing the Model

1. **Open Settings** (⚙️)
2. **Go to "AI Models"**
3. **Select a model for "Agent Mode"**
4. **Save**

**Recommended models:**
- **Claude 3.5 Sonnet** — Best at planning complex tasks
- **GPT-4o** — Excellent reasoning and code understanding
- **Gemini 2.0** — Fast and capable

---

## Safety Features

### Review Before Executing

Agent Mode always shows you the plan before executing:

- Reads your request
- Creates a step-by-step plan
- Shows commands it will run
- Waits for your approval

**You approve or reject before anything runs.**

### Permission Controls

Granular permissions prevent accidental damage:

- **Execute Commands** — Can it run commands at all?
- **Read Files** — Can it access your files?
- **Write Files** — Can it create/modify files?
- **Apply Code** — Can it suggest code changes?

Start with **limited permissions** (read-only). Enable as you trust the model.

### Session Overrides

Want different permissions for this task only?

1. **In Agent Mode window** — Click "⚙️ Settings"
2. **Adjust permissions** — Just for this session
3. **They reset** — Don't persist after you close Agent Mode

---

## Tips for Best Results

### 1. Be Specific

❌ Bad:
```
Sort my files
```

✅ Good:
```
Sort my projects by size and show the top 5 largest directories
```

### 2. Give Context

```
I have Node.js projects in ~/projects/. 
Install dependencies for all projects and run their test suites.
Show me which ones fail.
```

### 3. Break Complex Tasks

Instead of:
```
Deploy my app, run migrations, and update configs
```

Try:
```
Deploy my app (describe your deploy process)
```

Then after reviewing, ask:
```
Now run the database migrations
```

### 4. Review Carefully

Always read the plan before approving:

- Do you understand each step?
- Are the commands what you expected?
- Could anything go wrong?

If unsure, reject and rephrase your request.

### 5. Use Appropriate Models

- **Claude 3.5 Sonnet** — Reasoning-heavy tasks (planning, complex workflows)
- **GPT-4o** — Code-heavy tasks (refactoring, testing)
- **Gemini 2.0** — Quick, simple tasks (list files, basic operations)

---

## Common Patterns

### List and Count Files

```
Find all .py files in my project and count them by directory
```

Agent Mode will:
1. Find Python files
2. Group by directory
3. Count and display summary

### Backup/Sync

```
Sync my ~/projects directory to ~/backups/, excluding .git and node_modules
```

### Search and Replace

```
Find all TODO comments in my project and create a summary
```

Or:
```
Replace all instances of "oldname" with "newname" in .ts files
```

### System Administration

```
Check disk usage and show directories larger than 1GB
```

### Development Tasks

```
Run tests on all Python projects and show summary of failures
```

---

## Common Issues

### "Agent Mode gets stuck"

1. **Stop it** — Press Ctrl+C
2. **Try simpler task** — Break it into smaller pieces
3. **Check the logs** — What was the last thing it tried?

### "Agent Mode makes wrong decisions"

1. **Give more context** — Agent Mode works better with info
2. **Be more specific** — Instead of "sort files", say "sort by size"
3. **Use a better model** — Claude 3.5 Sonnet handles ambiguity better

### "Agent Mode wants to delete something wrong"

**Good!** Review and reject:

1. **Read the plan carefully**
2. **If it's wrong, click "Reject"**
3. **Rephrase** and try again

### "Permissions errors"

1. **Check your permissions** — Agent Mode may need to read/write files
2. **Enable required permissions** in Settings
3. **Try again**

---

## Advanced: Custom Profiles

For power users, you can create multiple Agent Mode profiles with different permission levels:

1. **Conservative** — Read-only, no execution
2. **Safe** — Read + execute, no write
3. **Full** — All permissions

Then switch profiles based on the task.

(This requires manual configuration — see [Execution Profiles](../AI_MODELS/EXECUTION_PROFILES.md) for details)

---

## Privacy and Security

- **Agent Mode sees your commands** — It reads your terminal output for context
- **Files are read locally** — Nothing leaves your machine without your consent
- **API calls are encrypted** — All requests to AI providers use TLS
- **You approve everything** — Agent Mode never runs commands without your approval

---

## Examples

### Example 1: Find Large Projects

```
Find all Node.js projects in ~/code/. 
Show their name, size, and last git commit date.
Sort by size largest first.
```

Agent Mode will:
- List Node.js projects
- Get directory sizes
- Get git commit dates
- Sort and display

### Example 2: Backup Strategy

```
Create timestamped backups of:
1. ~/.ssh/
2. ~/.config/
3. ~/projects/important/

Place them in ~/backups/
Compress with gzip
Show me the backup sizes
```

Agent Mode will:
- Create backup directories
- Copy files
- Compress
- Display results

### Example 3: Test All Projects

```
I have multiple React projects in ~/dev/.
For each project:
1. Install dependencies (if not already installed)
2. Run tests
3. Show me which ones fail

Summarize: How many passed, how many failed?
```

---

## Next Steps

- **[Terminal AI Help](./TERMINAL_AI.md)** — Quick command help
- **[Execution Profiles](../AI_MODELS/EXECUTION_PROFILES.md)** — Configure permissions
- **[Selecting Models](../AI_MODELS/SELECTING_MODELS.md)** — Choose your AI model
- **[Back to Features](./README.md)**
