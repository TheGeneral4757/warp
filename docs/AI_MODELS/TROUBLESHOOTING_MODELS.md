# Troubleshooting AI Models

Common issues and how to fix them.

---

## Model Not Responding

**Symptoms:** The AI doesn't respond, or takes forever.

**Fixes:**

1. **Check your internet connection** — Warp needs internet to reach the AI model
2. **Check model availability** — Visit your provider's status page:
   - [Anthropic Status](https://status.anthropic.com/)
   - [OpenAI Status](https://status.openai.com/)
   - [Google Cloud Status](https://cloud.google.com/status)
3. **Try a different model** — The model may be overloaded. Use a lighter model (e.g., Haiku instead of Sonnet)
4. **Check your API quota** — If using your own API key, verify you have available credits
5. **Restart Warp** — Sometimes the connection gets stuck. Close and reopen Warp

---

## "Authentication Failed" or "Invalid API Key"

**Symptoms:** Error message saying your API key is invalid.

**Fixes:**

1. **Verify your API key** — Copy it again from your provider's console
   - Anthropic: [console.anthropic.com/api_keys](https://console.anthropic.com/api_keys)
   - OpenAI: [platform.openai.com/api_keys](https://platform.openai.com/api_keys)
   - Google: [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. **Check for typos** — API keys are long and easy to mistype
3. **Check for extra spaces** — Copy/paste may include leading/trailing spaces
4. **Check if key expired** — Some providers auto-rotate keys
5. **Reset the key** in Warp:
   - Settings → AI Models → Remove the old key → Add new key

---

## "Model Not Found"

**Symptoms:** Error saying the model doesn't exist or isn't available.

**Fixes:**

1. **Verify the model exists** — Check your provider's API docs
   - [Anthropic Models](https://docs.anthropic.com/en/docs/about-claude/models/latest)
   - [OpenAI Models](https://platform.openai.com/docs/models)
   - [Google Models](https://ai.google.dev/models)
2. **Check your region** — Some models are only available in certain regions (especially Bedrock)
3. **Check your plan** — The model may require a paid account
4. **Try a different model** — Use a model you know is available

---

## "Rate Limited" or "Too Many Requests"

**Symptoms:** Error saying you've made too many requests.

**Fixes:**

1. **Wait a few minutes** — Rate limits reset after a short time
2. **Check your usage** — Visit your provider's dashboard to see current usage
3. **Upgrade your plan** — Higher tier accounts get higher rate limits
4. **Use a lighter model** — Haiku and GPT-4o Mini have lower usage
5. **Batch your requests** — Don't make multiple requests simultaneously

---

## "No Available Balance" or "Insufficient Quota"

**Symptoms:** Error saying you've run out of credits or quota.

**Fixes:**

1. **Add a payment method** — If using a free trial, you may need to pay
2. **Check your usage** — Visit billing dashboard to see what happened
   - [Anthropic Billing](https://console.anthropic.com/settings/billing)
   - [OpenAI Billing](https://platform.openai.com/account/billing/overview)
   - [Google Cloud Billing](https://console.cloud.google.com/billing)
3. **Wait for reset** — If on a monthly plan, wait for your monthly reset
4. **Use Warp's default models** — If using your own API key, switch back to Warp's provided models

---

## Model Selected but Not Working

**Symptoms:** The right model shows in settings, but it's not actually being used.

**Fixes:**

1. **Check the feature** — Different features use different models. You may have set:
   - Agent Mode model (Cmd+Shift+K)
   - But you're using regular mode (Cmd+K)
   - Try using the correct feature
2. **Check your profile** — Your workspace admin may have override settings
   - Ask your admin if they've locked the model selection
3. **Verify the model supports that feature** —
   - Computer Use is only available in certain models (Claude 3.5 Sonnet)
   - See [Supported Models](./SUPPORTED_MODELS.md) for feature support
4. **Check feature flags** — Some new models require feature flag enablement
   - Ask your admin about experimental models

---

## Model Keeps Changing Unexpectedly

**Symptoms:** You set a model, but Warp switches back to a different one.

**Fixes:**

1. **Check workspace settings** — Your admin may have set defaults
   - Ask your workspace admin
2. **Clear cache** — Sometimes cached settings cause issues:
   - Close Warp completely
   - Reopen it
3. **Check your internet** — If syncing is interrupted, settings might revert
4. **Check cloud sync** — If you use multiple devices:
   - Settings sync across devices
   - Check what's selected on your other devices

---

## Poor Model Quality or Wrong Answers

**Symptoms:** The model gives bad suggestions or incorrect information.

**Fixes:**

1. **Try a better model** — Use Claude 3.5 Sonnet or GPT-4o instead of Haiku
   - See [Supported Models](./SUPPORTED_MODELS.md) for quality comparison
2. **Give more context** — AI does better with more information
   - Include error messages, code, recent context
3. **Break down the task** — Complex tasks work better as smaller steps
4. **Try rephrasing** — How you ask matters. Be specific and clear
5. **Check the model's docs** — Some models have known limitations
   - [Claude Docs](https://docs.anthropic.com/)
   - [OpenAI Docs](https://platform.openai.com/docs)

---

## Timeout or Hanging

**Symptoms:** Warp freezes or the response never comes.

**Fixes:**

1. **Check internet** — Is your connection stable?
2. **Try a faster model** — Gemini 2.0 Flash or Haiku are faster
3. **Restart Warp** — Sometimes the connection gets stuck
4. **Check if process is stuck** —
   - Mac: Force quit (Cmd+Q won't work, use Activity Monitor)
   - Windows: Task Manager → End Task
5. **Check server status** — Is the provider's API up?

---

## Using Bedrock but Getting Errors

**Symptoms:** AWS Bedrock errors like "Missing credentials" or "Model not found".

**Fixes:**

1. **Check AWS credentials** — Verify your access key ID and secret
   - Get credentials: [AWS IAM Console](https://console.aws.amazon.com/iam/)
   - [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
2. **Check region** — Some models only available in certain regions:
   - US East (N. Virginia) — us-east-1
   - US West (Oregon) — us-west-2
3. **Enable Bedrock models** — You must enable models in your AWS account
   - AWS Console → Bedrock → Model access
4. **Check IAM permissions** — Your IAM user needs Bedrock permissions:
   ```
   bedrock:InvokeModel
   bedrock:InvokeModelWithResponseStream
   ```

---

## Custom API Endpoint Not Working

**Symptoms:** You've configured a custom endpoint (local model, private server) but it's not working.

**Fixes:**

1. **Verify endpoint is running** — Is your server actually up?
   ```bash
   curl http://localhost:8000/health
   ```
2. **Check the URL** — Warp should use the **base URL**, not the full endpoint:
   - ✅ Correct: `http://localhost:8000`
   - ❌ Wrong: `http://localhost:8000/v1/chat/completions`
3. **Check API key format** — Should match what your endpoint expects
4. **Check CORS if web-based** — Browser-based Warp may need CORS headers
5. **Check server logs** — Is your custom server receiving requests?

---

## Model Performance is Degraded

**Symptoms:** The model is slow or using lots of resources.

**Fixes:**

1. **Try a lighter model** — Haiku or GPT-4o Mini use fewer resources
2. **Check system resources** — Is your computer running low on RAM?
3. **Close other apps** — Free up CPU and memory
4. **Check background tasks** — Is something else consuming resources?
5. **Check the provider** — The remote service may be slow (check status page)

---

## Getting Help

If none of these solutions work:

1. **Check the FAQ** — See [FAQ.md](../FAQ.md)
2. **Check detailed logs** — Warp logs may have more info:
   - Mac/Linux: `~/.local/share/warp/logs/`
   - Windows: `%AppData%\Warp\logs\`
3. **Report an issue** — Open a GitHub issue with:
   - The error message you see
   - What you were trying to do
   - Which model and provider you were using
   - Your logs (from the location above)

---

## Next Steps

- **[Bring Your Own API Key](./BRING_YOUR_OWN_API_KEY.md)** — More details on API keys
- **[Execution Profiles](./EXECUTION_PROFILES.md)** — Understanding permissions
- **[Back to AI Models](./README.md)**
