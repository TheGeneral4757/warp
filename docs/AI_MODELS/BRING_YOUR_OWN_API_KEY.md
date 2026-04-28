# Bring Your Own API Key

Use your own API keys from OpenAI, Anthropic, Google, or other providers.

---

## Why Use Your Own API Key?

- **Cost control** — Pay directly to the provider, often cheaper
- **Latest models** — Access bleeding-edge models immediately
- **Privacy** — Models you host privately or your own deployments
- **Organization account** — Use your company's API account
- **Unlimited quota** — Not limited by Warp's shared quota

---

## Supported Providers

- **Anthropic** (Claude) — OpenAI, self-hosted, Bedrock
- **OpenAI** — ChatGPT API, enterprise deployments
- **Google** (Gemini) — Official API
- **AWS Bedrock** — AWS-hosted model routing
- **Open Router** — Router for multiple models
- Custom deployments (if compatible with provider APIs)

---

## Getting Your API Key

### Anthropic (Claude)

**Step 1:** Go to [console.anthropic.com](https://console.anthropic.com)

**Step 2:** Sign in or create an account

**Step 3:** Click "API Keys" in the left sidebar

**Step 4:** Click "Create Key"

**Step 5:** Copy the key (starts with `sk-ant-`)

💡 Keep it secret! Don't share it or commit it to version control.

### OpenAI (GPT-4o, etc.)

**Step 1:** Go to [platform.openai.com](https://platform.openai.com)

**Step 2:** Sign in or create an account

**Step 3:** Click "API keys" → "Org settings"

**Step 4:** Click "Create new secret key"

**Step 5:** Copy the key (starts with `sk-`)

💡 Store it safely — you can't see it again after closing the dialog.

### Google (Gemini)

**Step 1:** Go to [aistudio.google.com](https://aistudio.google.com)

**Step 2:** Click "Get API Key"

**Step 3:** Click "Create API Key"

**Step 4:** Copy the key

💡 Enable the Gemini API in your Google Cloud project if prompted.

### AWS Bedrock

**Step 1:** Set up AWS credentials (via AWS CLI or IAM)

**Step 2:** Enable Bedrock models in your AWS region

**Step 3:** In Warp Settings, enter your AWS Access Key and Secret Key

See [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) for details.

---

## Adding Your API Key to Warp

### Simple Method (UI)

1. **Open Warp Settings** (⚙️)
2. **Go to "AI Models"**
3. **Look for "Add Custom API Key"** or **"Bring Your Own"** section
4. **Select the provider** (Anthropic, OpenAI, Google, etc.)
5. **Paste your API key** into the text field
6. **Click "Save"**

Warp stores your key securely in your system's keychain.

### Advanced Method (Environment Variables)

Set environment variables before launching Warp:

```bash
# Anthropic/Claude
export ANTHROPIC_API_KEY=sk-ant-...

# OpenAI
export OPENAI_API_KEY=sk-...

# Google Gemini
export GOOGLE_API_KEY=...

# AWS Bedrock
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_REGION=us-east-1

# Open Router (if using)
export OPEN_ROUTER_API_KEY=...
```

Then launch Warp:
```bash
warp
```

---

## Using Your Custom Models

Once your API key is added:

1. **Open Settings** → **AI Models**
2. **Select your provider** from the dropdown
3. **Choose your model** (you'll see models from that provider)
4. **Click "Save"**

---

## Verifying Your API Key Works

**Test it immediately:**

1. Open Warp
2. Ask a simple question with **Cmd+K** or **Ctrl+K**
3. If it works, your API key is correctly configured

**If it doesn't work:**

- Check the [Troubleshooting Models](./TROUBLESHOOTING_MODELS.md) section
- Verify your API key is valid (copy/paste it again)
- Ensure you have available credits with the provider
- Check that the model exists in that provider's API

---

## Cost and Billing

### How Billing Works

When using your own API key, **charges go directly to your provider account**, not Warp.

- **Anthropic** — See pricing at [console.anthropic.com/settings/billing](https://console.anthropic.com/settings/billing)
- **OpenAI** — See pricing at [platform.openai.com/account/billing](https://platform.openai.com/account/billing)
- **Google** — See pricing at [console.cloud.google.com/billing](https://console.cloud.google.com/billing)

### Estimating Costs

Costs vary by model and usage:

- **Claude 3.5 Sonnet** — ~$3-5 per million input tokens
- **GPT-4o** — ~$5-15 per million tokens (input)
- **Gemini 2.0** — ~$0.10 per million tokens

A typical command = 100-500 tokens. A complex task = 1000-5000 tokens.

Example: 1000 API calls with Claude Sonnet ≈ $0.50-$2.50

---

## Managing Multiple API Keys

You can add multiple API keys for different providers:

1. **Anthropic** for Claude models
2. **OpenAI** for GPT models
3. **Google** for Gemini models

Warp lets you choose which provider to use for each feature.

---

## Securing Your API Key

🔒 **Best Practices:**

1. **Never hardcode keys** — Use environment variables or Warp's secure storage
2. **Rotate regularly** — Recreate keys if you suspect exposure
3. **Use least privilege** — Create API keys with minimal required permissions
4. **Monitor usage** — Check your provider's dashboard for unexpected usage

If you accidentally expose a key:
1. Immediately delete it from your provider's console
2. Remove it from Warp's settings
3. Create a new key
4. Update Warp with the new key

---

## Troubleshooting

**"Authentication failed" or "Invalid API key"**

→ Your API key is wrong or expired. Get a new one from your provider's console.

**"Model not found"**

→ The model may not exist or isn't available in your region. Check your provider's API docs.

**"Rate limited"**

→ You've hit your provider's rate limit. Wait a bit or upgrade your plan.

**"No available balance"**

→ You've used your credits. Add a payment method or wait for monthly reset.

See [Troubleshooting Models](./TROUBLESHOOTING_MODELS.md) for more help.

---

## Advanced: Using a Reverse Proxy or Custom Deployment

If you have a **private deployment** of an LLM (e.g., local Ollama, or a custom API server):

1. Some providers allow custom base URLs (OpenAI-compatible servers)
2. In Warp Settings, you may see an option for "Custom Endpoint URL"
3. Enter your custom base URL
4. Configure your API key as normal

Example (OpenAI-compatible server):
```
Base URL: http://localhost:8000/v1
API Key: your-key
```

Check [AI System Architecture](../FOR_DEVELOPERS/AI_SYSTEM_ARCHITECTURE.md) for developer details.

---

## Next Steps

- **[Troubleshooting Models](./TROUBLESHOOTING_MODELS.md)** — Getting help with API key issues
- **[Supported Models](./SUPPORTED_MODELS.md)** — Available models and their specs
- **[Execution Profiles](./EXECUTION_PROFILES.md)** — Different models per feature
- **[Back to AI Models](./README.md)**
