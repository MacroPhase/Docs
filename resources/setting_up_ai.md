# Setting Up AI Features

MacroPhase's AI features — the coach, food scanner, and meal analysis — work best with an API key. Here's how to set it up, which models to pick, and how web grounding works.

## Quick Strategy

- **Web Grounding (Default: DuckDuckGo)**: Live web search is completely free and enabled by default via DuckDuckGo (no API key needed). You can optionally connect BYOK providers (Jina, Tavily, Gemini 3, or SearXNG) in **Settings → AI Features → Web Grounding**.
- **Ultra-budget paid:** Use OpenRouter. It taps into efficient models like DeepSeek, Xiaomi, and Gemini for fractions of a cent.

## Supported Providers

- **Gemini** — Default. Free tier available. Requires API key.
- **OpenAI Compatible** — GPT-4o-mini, GPT-5, etc. Requires API key.
- **OpenRouter** — Hundreds of models filtered by use case. Requires API key.
- **Anthropic** — Claude models. Requires API key.
- **Local Gemma** — Fully offline, on-device. No API key.

---

## Finding the Best Live Models on OpenRouter

You can paste any model ID from OpenRouter directly into MacroPhase (**More → AI Settings → Custom Model ID**). OpenRouter updates models and pricing daily. Use the filtered links below to find the current top models for each use case:

### 1. AI Coach (Tool Calling & Fast Conversation)
The AI Coach requires models that support **tool calling** (function calling) and at least 16k context to handle meal logging, memory retrieval, and recipe generation.

- 🔗 **[Explore Live Tool-Calling Models on OpenRouter](https://openrouter.ai/models?supported_parameters=tools&order=pricing-low-to-high)**
- *Look for: Models with the `tools` tag and low input/output pricing per 1M tokens.*

### 2. Food & Receipt Scanner (Vision & Multimodal)
The food and receipt scanners require models with **multimodal image input** to read nutrition panels, plate photos, and printed receipts.

- 🔗 **[Explore Live Vision Models on OpenRouter](https://openrouter.ai/models?order=pricing-low-to-high)**
- *Look for: Models with `Image` modality support.*

### 3. Ultra-Budget / Background Insights
Used for background analytics and weekly summaries where low cost-per-token is the main priority.

- 🔗 **[Explore Lowest-Cost Models on OpenRouter](https://openrouter.ai/models?order=pricing-low-to-high)**
- *Look for: Sub-cent models with 32k+ context windows.*

> 💡 **Key Idea**
> You don't need to use the same model for everything. In **More → AI Settings**, you can configure different models for the Coach, the Food Scanner, and Background Insights independently. Copy any `provider/model-id` from OpenRouter and paste it into the custom model field.

---

## Grounded Product & Web Search

MacroPhase first searches its local databases and Open Food Facts at no cost. For exact restaurant items or branded products that are still missing, the coach can search the web:

- **DuckDuckGo (Free & Default):** Built-in zero-key search. No configuration or API key needed.
- **Jina:** Free starter tokens; returns clean markdown page contents for AI grounding.
- **Tavily:** Free monthly credits with personal API key; structured web research.
- **Gemini 3:** Native Google search grounding with a Gemini API key.
- **Custom SearXNG:** Self-hosted open-source meta-search instance.

Web search is seamlessly integrated into the coach and can also be triggered explicitly via the `/search` and `/browse` slash commands.

---

## Getting an API Key

### Gemini (Recommended Start)

1. Go to [aistudio.google.com/apikey](https://aistudio.google.com/app/apikey)
2. Sign in with your Google account
3. Click **Create API Key**
4. Copy the key

### OpenRouter

1. Go to [openrouter.ai/keys](https://openrouter.ai/keys)
2. Create an account
3. Navigate to **Keys**
4. Create a new key and add credits ($5 will last months)

### OpenAI

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Sign in and create a new secret key

### Anthropic

1. Go to [console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys)
2. Sign in and generate a key

---

## Configuring in MacroPhase

1. Open the **More** tab
2. Tap **AI Settings**
3. Select your **AI Provider**
4. Paste your API key
5. Select or type your desired model IDs

> 🔒 **Security:** Your API key is stored in your device's encrypted preferences. It is never sent to any MacroPhase server — requests go directly from your phone to your chosen provider.

## Troubleshooting

- **"API key invalid"** — Check you copied the full key. Some providers need a credit balance.
- **Coach not responding** — Check internet connectivity. Try switching models.
- **Slow responses** — Free tiers may have rate limits. Switch to a faster flash model.
- **Scanner missing foods** — Take photos from directly above with good lighting.
