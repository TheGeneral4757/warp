# Supported AI Models

Complete list of models available in Warp and their capabilities.

---

## Anthropic Models

### Claude 3.5 Sonnet
- **Speed:** Moderate (2-5 sec typical)
- **Capability:** ⭐⭐⭐⭐⭐ Excellent reasoning, best for complex tasks
- **Cost:** Medium
- **Best for:** Agent mode, complex problem-solving, code reasoning
- **Features:**
  - Excellent at planning and step-by-step reasoning
  - Can use tools and execute commands
  - Great at understanding context
  - Supports extended thinking (reasoning mode available)

### Claude 3.5 Haiku
- **Speed:** ⚡⚡⚡ Very fast (< 1 sec typical)
- **Capability:** ⭐⭐⭐ Good for most tasks
- **Cost:** Very low
- **Best for:** Quick command help, testing, budget-conscious users
- **Features:**
  - Lightweight and responsive
  - Good reasoning for simple tasks
  - Can use tools

### Claude 3 Opus
- **Speed:** Slower (5+ sec typical)
- **Capability:** ⭐⭐⭐⭐⭐ Most capable (legacy)
- **Cost:** Higher
- **Best for:** Maximum quality when other models aren't sufficient
- **Note:** Consider Claude 3.5 Sonnet instead (better performance, lower cost)

---

## OpenAI Models

### GPT-4o
- **Speed:** Fast (1-3 sec typical)
- **Capability:** ⭐⭐⭐⭐⭐ Excellent all-around
- **Cost:** Medium-high
- **Best for:** Code generation, general purpose, multimodal (image) understanding
- **Features:**
  - Excellent at code understanding and generation
  - Fast and responsive
  - Good at instruction following
  - Can analyze images

### GPT-4o Mini
- **Speed:** ⚡⚡ Fast (< 1 sec typical)
- **Capability:** ⭐⭐⭐ Good lightweight option
- **Cost:** Low
- **Best for:** Quick responses, lightweight tasks
- **Features:**
  - Lightweight version of GPT-4o
  - Good quality for the speed
  - Budget-friendly

### GPT-4 Turbo
- **Speed:** Moderate (2-4 sec typical)
- **Capability:** ⭐⭐⭐⭐ Very capable (legacy)
- **Cost:** Medium-high
- **Best for:** Maximum quality (consider GPT-4o instead)
- **Note:** GPT-4o is recommended as a replacement

---

## Google Models

### Gemini 2.0 Flash
- **Speed:** ⚡⚡⚡ Very fast (< 1 sec typical)
- **Capability:** ⭐⭐⭐⭐ Good reasoning + speed
- **Cost:** Low
- **Best for:** Quick responses, good balance of speed and quality
- **Features:**
  - Fast response times
  - Good reasoning capabilities
  - Multimodal support (text, images, video)
  - Native tool use

### Gemini 1.5 Pro
- **Speed:** Moderate (2-4 sec typical)
- **Capability:** ⭐⭐⭐⭐ Very capable
- **Cost:** Medium
- **Best for:** Complex reasoning, large context windows
- **Features:**
  - Excellent at processing long documents
  - Strong reasoning
  - Multimodal support

---

## AWS Bedrock Models

Warp can route requests to AWS Bedrock, allowing you to use:

- **Claude models** via Bedrock
- **Mistral** models
- **Llama** models
- Custom fine-tuned models

**Setup:** See [Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md) for Bedrock configuration.

---

## Custom / Bring Your Own

You can use any model from:
- Your own OpenAI organization
- Your own Anthropic API key
- Your own Google API key
- Private deployments (Bedrock, etc.)

See [Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md) for setup instructions.

---

## Quick Comparison Table

| Model | Speed | Quality | Cost | Best For |
|-------|-------|---------|------|----------|
| Claude 3.5 Sonnet | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | $$ | Reasoning, agent mode |
| Claude 3.5 Haiku | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | $ | Speed + budget |
| GPT-4o | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | $$ | Code, general |
| GPT-4o Mini | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | $ | Budget + speed |
| Gemini 2.0 Flash | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | $ | Speed + quality |
| Gemini 1.5 Pro | ⭐⭐⭐ | ⭐⭐⭐⭐ | $$ | Complex tasks |

---

## Feature Support by Model

| Feature | Claude | GPT-4o | Gemini | Bedrock |
|---------|--------|--------|--------|---------|
| Agent Mode | ✅ | ✅ | ✅ | ✅ |
| Code Editing | ✅ | ✅ | ✅ | ✅ |
| CLI Agent | ✅ | ✅ | ✅ | ✅ |
| Computer Use | ✅ | ❌ | ❌ | Varies |
| Reasoning Mode | ✅ | ✅ | ❌ | Varies |
| Multimodal | ✅ | ✅ | ✅ | Varies |

---

## Choosing the Right Model

**If you want the best results:**  
→ Claude 3.5 Sonnet (excellent reasoning, fast enough for most)

**If you want speed:**  
→ Gemini 2.0 Flash or Claude 3.5 Haiku

**If you want to save money:**  
→ Claude 3.5 Haiku or GPT-4o Mini

**If you want maximum capability:**  
→ Claude 3.5 Sonnet or GPT-4o

**If you're unsure:**  
→ Let Warp pick a model for you (default settings are optimized)

---

## Model Updates

Warp automatically gets access to new models as they're released by providers. Check back here for updates, or see [Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md) to use the absolute latest models.

---

[👈 Back to AI Models](./README.md) | [Next: Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md)
