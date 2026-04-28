# Code Editing with AI

AI-powered code suggestions and edits in your editor.

---

## Overview

Warp integrates with your code editor to provide AI-powered suggestions and improvements.

---

## Supported Editors

- **VS Code** — Full integration
- **JetBrains IDEs** — IntelliJ IDEA, PyCharm, WebStorm, etc.
- **Vim/Neovim** — Via Warp plugin
- **Other editors** — Via Warp Terminal

---

## Features

### Code Suggestions

Get suggestions for:
- **Completing functions** — Warp suggests the rest of the function
- **Fixing errors** — Warp identifies and explains issues
- **Refactoring** — Suggestions to improve code quality
- **Documentation** — Auto-generate docstrings and comments

### Code Diffs

Warp can show diffs and apply changes:
- **Review changes** — See what will be modified
- **Apply automatically** — Accept the change with one click
- **Reject** — If you don't like the suggestion

---

## Using Code Editing

### In VS Code

1. **Install Warp extension** (from VS Code Marketplace)
2. **Make sure Warp is running**
3. **Highlight code** you want help with
4. **Press Cmd+Shift+P** (Mac) or **Ctrl+Shift+P** (Windows/Linux)
5. **Search for "Warp"** to see available commands:
   - Explain code
   - Generate tests
   - Refactor
   - Document
6. **Select action** and Warp will help

### In JetBrains

1. **Install Warp plugin** (from JetBrains Plugin Marketplace)
2. **Make sure Warp is running**
3. **Right-click code** and select "Warp AI"
4. **Choose action** from context menu

### In Terminal

If not using an editor integration:

1. **Open Warp**
2. **Press Cmd+K** (or Ctrl+K)
3. **Ask for code help:**
   ```
   cat my_file.py | warp "add type hints"
   ```

---

## Configuring Code Editing

### Choose Your Model

1. **Open Settings** (⚙️)
2. **Go to "AI Models"**
3. **Select model for "Code Editing"**
4. **Save**

**Recommended models:**
- **Claude 3.5 Sonnet** — Excellent code understanding
- **GPT-4o** — Great for code generation
- **Gemini 2.0** — Fast, good quality

### Set Permissions

1. **Open Settings** (⚙️)
2. **Go to "AI Models"** or **"Code Editor"**
3. **Configure permissions:**
   - Can read files?
   - Can suggest diffs?
   - Can apply changes?
4. **Save**

---

## Use Cases

### Generate Tests

Highlight a function, ask Warp to "Generate unit tests":

```python
def calculate_tax(amount, rate):
    return amount * rate
```

Warp generates:

```python
def test_calculate_tax():
    assert calculate_tax(100, 0.1) == 10.0
    assert calculate_tax(0, 0.1) == 0
```

### Refactor Code

```python
# Before
if x > 0:
    if y > 0:
        result = x + y
    else:
        result = x
else:
    result = y

# Ask: "Simplify this logic"
# Warp suggests:
result = x if x > 0 else y
if y > 0 and x > 0:
    result = x + y
```

### Add Documentation

```python
def merge_datasets(left, right, on_key):
    # Ask Warp: "Add docstring"
```

Warp adds:

```python
def merge_datasets(left, right, on_key):
    """
    Merge two datasets on a common key.
    
    Args:
        left: Left dataset
        right: Right dataset  
        on_key: Key to merge on
        
    Returns:
        Merged dataset
    """
```

### Fix Issues

Paste code with an error:

```javascript
const user = {name: "John"
  age: 30
};
```

Ask: "Fix syntax errors"

Warp suggests:

```javascript
const user = {
  name: "John",
  age: 30
};
```

---

## Tips for Best Results

### 1. Give Context

Include surrounding code, not just the snippet:

❌ Bad:
```python
x = calculate(y)
```

✅ Good:
```python
def process_data(data):
    x = calculate(y)  # Calculate something
    return x * 2
```

### 2. Be Specific

❌ Bad:
```
Fix this
```

✅ Good:
```
Add type hints to this function
```

### 3. Review Changes

Always review diffs before applying:

- Does it match your style?
- Does it change behavior?
- Are imports handled correctly?

### 4. Use Appropriate Models

- **Claude** — Complex refactoring, documentation
- **GPT-4o** — Code generation, tests
- **Gemini** — Quick suggestions, syntax highlighting

---

## Common Tasks

| Task | How to Ask |
|------|-----------|
| Generate tests | "Write unit tests for this function" |
| Add docstring | "Add Python docstring" |
| Refactor | "Simplify this logic" |
| Fix syntax | "Fix syntax errors" |
| Add comments | "Add inline comments" |
| Explain code | "Explain what this does" |
| Optimize | "Make this more efficient" |
| Add types | "Add TypeScript types" or "Add type hints" |

---

## Privacy

- **Code stays on your device** — Sent to AI provider only when you ask
- **Nothing auto-uploads** — You choose when Warp sees your code
- **Encrypted in transit** — TLS for all API requests
- **Not stored** — Provider doesn't keep your code

---

## Limitations

- **Context window** — Very large files may not fit in context
- **No real-time** — Not a real-time linter; use those tools
- **Language support** — Better with common languages (Python, JavaScript, Java)
- **Style variations** — Suggestions reflect the model's training

---

## Troubleshooting

### "Code editing not showing suggestions"

1. **Check Warp is running**
2. **Check editor integration is installed**
3. **Check permissions** — Does code editing have permission to read files?
4. **Restart editor** — Some editors need restart after installing plugin

### "Suggestions are bad quality"

1. **Try a better model** — Use Claude 3.5 Sonnet
2. **Give more context** — Include surrounding code
3. **Be more specific** — "Add docstring" vs "Fix"

### "Applying diffs breaks code"

1. **Review diff before applying** — Always check
2. **Reduce context** — Fewer lines = fewer changes
3. **Use undo** — Cmd+Z if something breaks

---

## Next Steps

- **[Agent Mode](./AGENT_MODE.md)** — Autonomous task completion
- **[Terminal AI](./TERMINAL_AI.md)** — Command help in terminal
- **[Selecting Models](../AI_MODELS/SELECTING_MODELS.md)** — Choose your model
- **[Back to Features](./README.md)**
