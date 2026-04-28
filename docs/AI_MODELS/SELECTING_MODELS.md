# Selecting and Switching AI Models

Learn how to choose which AI model powers your Warp experience.

---

## TL;DR — Switch Models in 3 Steps

1. **Open Warp Settings** → Click ⚙️ (bottom left)
2. **Go to AI Models** → Select the "Models" section
3. **Choose your model** and click "Save"

Done! Your choice is saved and takes effect immediately.

---

## Understanding Model Purposes

Warp uses different models for different features. You can customize each one:

### 1. **Agent Mode Model**
Used when you press **Cmd+Shift+K** (Mac) or **Ctrl+Shift+K** (Windows/Linux) to let Warp autonomously complete tasks.

- **Best choice:** Claude 3.5 Sonnet (excellent at planning)
- **Fast option:** Gemini 2.0 (quick responses)
- **Budget option:** Claude 3 Haiku (low cost)

### 2. **Code Editing Model**
Used when Warp suggests code edits and improvements in your editor.

- **Best choice:** Claude 3.5 Sonnet or GPT-4o (excellent code understanding)
- **Fast option:** Gemini 2.0
- **Budget option:** Claude 3 Haiku

### 3. **CLI Agent Model**
Used when you ask for command help (Cmd+K).

- **Best choice:** Any recent model (Claude, GPT-4o, Gemini)
- **Fast option:** Gemini 2.0
- **Budget option:** Claude 3 Haiku

### 4. **Computer Use Model** (when available)
Used for automation and computer vision tasks.

- **Best choice:** Claude 3.5 Sonnet (has computer use capability)

---

## Step-by-Step: Change a Model

### Via UI (Easiest)

1. **Click Settings** (⚙️ icon in bottom left)
2. **Select "AI Models"** from the left menu
3. **For each feature** (Agent Mode, Code Editing, etc.):
   - Click the dropdown next to the feature name
   - Select your preferred model
4. **Click "Save"** at the bottom

### Via Command Line (Advanced)

If you prefer using your own API keys, see [Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md).

---

## Comparing Models

### Speed vs Quality Tradeoff

**Fastest:**
- Gemini 2.0 Flash — Very fast, good quality
- Claude 3 Haiku — Very fast, lightweight

**Best Quality:**
- Claude 3.5 Sonnet — Best reasoning, slowest
- GPT-4o — Excellent all-around

**Best Balance:**
- Claude 3.5 Sonnet — Reasoning + speed
- GPT-4o — Versatility
- Gemini 2.0 — Speed with good quality

---

## Cost Considerations

Models have different pricing. If you're using Warp's provided models:

- **Claude 3 Haiku** — Most economical
- **Gemini 2.0 Flash** — Fast and cheap
- **Claude 3.5 Sonnet** — Higher quality, higher cost
- **GPT-4o** — Premium quality, premium cost

💡 **Tip:** Use Haiku for testing, Sonnet for important tasks.

---

## Using Different Models Per Feature

You don't have to use the same model everywhere! For example:

- **Agent Mode:** Claude 3.5 Sonnet (best reasoning)
- **Code Editing:** GPT-4o (great for code)
- **CLI Commands:** Gemini 2.0 (fast)

This way you optimize for what matters most in each context.

---

## What If I Want Custom Models?

See [Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md) to use your own OpenAI, Anthropic, or Google API keys.

---

## Troubleshooting Model Selection

**Model not showing up?**  
→ It may require a feature flag. Ask an admin or check [Troubleshooting Models](./TROUBLESHOOTING_MODELS.md).

**Model changed unexpectedly?**  
→ Your workspace admin may have set a default. Check your workspace settings or contact your admin.

**Want to revert to defaults?**  
→ Go back to Settings → AI Models → Click "Reset to Defaults"

---

## Next Steps

- **[Supported Models](./SUPPORTED_MODELS.md)** — Detailed specs for each model
- **[Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md)** — Use your custom API key
- **[Execution Profiles](./EXECUTION_PROFILES.md)** — Advanced: permissions per profile
- **[Back to AI Models](./README.md)**
