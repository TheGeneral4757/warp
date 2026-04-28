# Terminal AI Help

Get help with commands right in your terminal.

---

## What Is Terminal AI Help?

Press **Cmd+K** (Mac) or **Ctrl+K** (Windows/Linux) to ask Warp for help with commands.

Warp's AI:
- Understands your question
- Suggests relevant commands
- Explains what commands do
- Learns from your terminal context

---

## How to Use It

### Ask a Question

1. **Press Cmd+K** (Mac) or **Ctrl+K** (Windows/Linux)
2. **Type your question:**
   ```
   How do I list files by size?
   ```
3. **Warp suggests commands:**
   ```
   ls -laSh | sort -rh
   ```
4. **Review the suggestion**
5. **Press Enter** to run it

---

## Common Questions

### File Operations

```
How do I find all PNG files?
→ find . -name "*.png"

How do I rename all .txt files to .md?
→ for f in *.txt; do mv "$f" "${f%.txt}.md"; done

How do I count files in each directory?
→ du -h --max-depth=1 | sort -rh

How do I find large files?
→ find . -size +100M -type f
```

### Text Processing

```
How do I search for a string in files?
→ grep -r "search term" .

How do I replace text in multiple files?
→ find . -name "*.txt" -exec sed -i 's/old/new/g' {} +

How do I see the last 10 lines of a file?
→ tail -n 10 filename
```

### System Information

```
How do I check disk usage?
→ df -h

How do I see running processes?
→ ps aux

How do I check available memory?
→ free -h

How do I see network connections?
→ netstat -an
```

### Git Operations

```
How do I see recent commits?
→ git log --oneline -n 10

How do I check git status?
→ git status

How do I stash changes?
→ git stash

How do I merge branches?
→ git merge branch_name
```

### Development

```
How do I install Python dependencies?
→ pip install -r requirements.txt

How do I run Node tests?
→ npm test

How do I build my project?
→ cargo build --release

How do I start a local server?
→ python -m http.server 8000
```

---

## Understanding the Suggestions

### Example: Listing Files by Size

You ask: "Show files sorted by size, largest first"

Warp suggests:
```bash
ls -laSh | sort -rh
```

**Breaking it down:**
- `ls -laSh` — List all files with sizes in human-readable format
- `|` — Pipe (send output to next command)
- `sort -rh` — Sort by numbers in reverse order (largest first)

Warp can explain any command — just ask!

---

## Tips for Best Results

### 1. Be Specific

❌ Bad:
```
Show me files
```

✅ Good:
```
Show me all Python files modified in the last week, sorted by date
```

### 2. Give Context

```
I have a Docker setup. How do I see logs from container X?
```

Warp understands the context and gives relevant suggestions.

### 3. Ask Follow-ups

```
You: How do I find all PDFs?
Warp: find . -name "*.pdf"

You: How do I also exclude the Downloads folder?
Warp: find . -name "*.pdf" ! -path "*/Downloads/*"
```

### 4. Request Explanations

If you don't understand a suggestion:

```
You: (After getting a command suggestion)
What does this command do?
→ Warp explains each part
```

---

## Customizing Terminal AI

### Choose Your Model

1. **Open Settings** (⚙️)
2. **Go to "AI Models"**
3. **Select model for "CLI Agent"** or **"Terminal AI"**
4. **Save**

**Recommended models:**
- **Claude 3.5 Sonnet** — Best explanations
- **GPT-4o** — Great for commands
- **Gemini 2.0** — Fast responses

### Set Permissions

1. **Open Settings** (⚙️)
2. **Go to "AI"** → **"Permissions"**
3. **Check "Can view output"** — Allows AI to see command results for context
4. **Save**

---

## What Terminal AI Can Do

✅ **Suggest commands** for what you want to do
✅ **Explain commands** — What do they do?
✅ **Combine commands** — Pipe operators, chains
✅ **Learn your context** — Based on your terminal history
✅ **Provide alternatives** — "Here's another way to do it"

❌ **Run commands automatically** — You always review first
❌ **Destructive operations** — Use Agent Mode for those
❌ **Access files** — Read-only access to context

---

## Safety

### You Always Review

Terminal AI never automatically runs commands:

1. **You ask a question**
2. **Warp suggests a command**
3. **You see it in the prompt**
4. **You press Enter** to run it (or Escape to cancel)

### Safe by Default

- **Read-only** — AI can only suggest, not execute
- **Context aware** — AI understands your directory and history
- **No secrets** — API keys and passwords aren't logged

### If Something Looks Wrong

If a suggestion seems dangerous:

1. **Don't press Enter**
2. **Ask Warp to explain** the command
3. **Ask for an alternative** — "Can you use -rf recursion instead?"
4. **Use `man` command** to verify — `man ls`

---

## Terminal AI vs Agent Mode

| Feature | Terminal AI (Cmd+K) | Agent Mode (Cmd+Shift+K) |
|---------|-----|---|
| **What it does** | Suggests commands | Runs them autonomously |
| **Your role** | Review each command | Approve overall plan |
| **Best for** | Quick questions | Complex tasks |
| **Safety** | Suggests only | Can execute |
| **Speed** | Instant | Takes time |

---

## Examples

### Example 1: Finding Files

```
You: Find all config files and show their sizes

Warp: find ~/.config -type f -exec ls -lh {} \;

You: (Asks to run it) → Runs the command and shows results
```

### Example 2: Analyzing Logs

```
You: Show me errors from the last hour in app.log

Warp: grep -i "error" app.log | awk '$1 >= "'"$(date -u -d '1 hour ago' +%H:%M)"'"'

You: (Runs it) → Shows recent errors
```

### Example 3: System Check

```
You: Check if my disk is full

Warp: df -h | grep -E "^/dev/"

You: (Runs it) → Shows disk usage

You: (Follow-up) What's using the most space?

Warp: du -sh /* | sort -rh

You: (Runs it) → Shows largest directories
```

---

## Troubleshooting

### "Terminal AI isn't showing up"

1. **Check keyboard shortcut** — Cmd+K (Mac) or Ctrl+K (Windows/Linux)
2. **Verify it's enabled** — Settings → AI
3. **Restart Warp** — Sometimes fixes UI issues

### "Suggestions are wrong"

1. **Be more specific** — Vague questions get vague answers
2. **Give more context** — "I'm in a Python project..."
3. **Try different model** — Claude works better for complex questions

### "Explanation doesn't make sense"

1. **Ask to simplify** — "Explain this more simply"
2. **Ask for alternatives** — "Is there a simpler way?"
3. **Use `man` command** — `man command` for official docs

---

## Pro Tips

### Use Terminal Context

Warp reads your terminal history:

```
You: (After running several git commands)
You: Commit these changes

Warp: (Understands you're doing git)
git commit -am "Your message here"
```

### Combine with Pipes

Terminal AI understands piping:

```
You: How do I find large files and show their size in MB?

Warp: find . -size +1M -exec ls -lh {} \; | awk '{print $5, $9}'
```

### Export Complex Commands

For complex commands, save to a script:

```
You: How do I backup my project excluding node_modules?

Warp: (Suggests rsync command)

You: (Save it)
echo 'rsync -av --exclude node_modules . backup/' > backup.sh
chmod +x backup.sh
```

---

## Next Steps

- **[Agent Mode](./AGENT_MODE.md)** — For autonomous task execution
- **[Code Editing](./CODE_EDITING.md)** — AI code suggestions
- **[Selecting Models](../AI_MODELS/SELECTING_MODELS.md)** — Choose your model
- **[Back to Features](./README.md)**
