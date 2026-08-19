# Setting Up AI Features

MacroPhase's AI features — the coach, food scanner, and meal analysis — work best with an API key. Here's how to set it up, and which models to pick.

## Quick Strategy

- **BYOK web grounding:** Web search is separate from the coach model. Choose Jina, Tavily, Gemini 2.5 Free, Gemini 3, or a custom SearXNG instance in **Settings → AI → Web Grounding**.
- **Ultra-budget paid:** Use OpenRouter. It taps into efficient models like DeepSeek and Xiaomi for fractions of a cent.

## Supported Providers

- **Gemini** — Default. Free tier available. Requires API key.
- **OpenAI Compatible** — GPT-5.4-mini and others. Requires API key.
- **OpenRouter** — Many models including budget options. Requires API key.
- **Anthropic** — Claude models. Requires API key.
- **Local Gemma** — Fully offline, on-device. No API key.

## Recommended Models

### For the AI Coach

The coach handles real-time conversation, tool use, and user feedback. Speed matters here.

- **Gemini:** `gemini-3.6-flash` — Best current balance of intelligence, speed, and tool use
- **OpenRouter:** `deepseek-v4-flash` — Massive context, ~$0.14/M input tokens
- **Anthropic:** `claude-haiku-4.5` — Frontier-class reasoning, fast, $1.00/M input
- **OpenAI:** `gpt-5.4-mini` — Snappy and smart, slightly higher cost

### For the Food Scanner

The scanner processes plate photos, nutrition labels, and screenshots. Vision quality matters here.

- **Gemini:** `gemini-3.6-flash` — Strong current multimodal and OCR model
- **OpenRouter:** `xiaomi/mimo-v2.5` — Top vision benchmarks, ~$0.14/M input
- **Anthropic:** `claude-sonnet-4.6` — Peak accuracy for complex layouts, $3.00/M input
- **OpenAI:** `gpt-5.4-mini` — Good entry-level multimodal

### For Insights & Summaries

Background analytics and weekly summaries. Cost-per-token matters more than speed.

- **Gemini:** `gemini-3.5-flash-lite` — Fastest, lowest-cost current Gemini model
- **OpenRouter:** `deepseek-v4-flash` — Best large-scale text processing, ~$0.14/M input
- **Anthropic:** `claude-haiku-4.5` — Strong instruction following
- **OpenAI:** `gpt-5.5-instant` — Excellent synthesis, higher cost

> 💡 **Key Idea**
> You don't need to pick the same model for everything. Use Gemini's free tier for the coach and scanner, and upgrade specific features if you need more capability.

## Grounded Product Search

Phase first searches its local databases and Open Food Facts at no cost. For an exact branded product that is still missing, Phase can use the selected web grounding provider and show cited evidence before saving a custom food.

- **Jina:** free starter tokens; designed to return clean page contents for AI grounding.
- **Tavily:** recurring free monthly credits with a personal API key.
- **Gemini 2.5 Free:** up to 500 grounded requests per day on Google's free tier; scheduled to shut down October 16, 2026.
- **Gemini 3:** requires a billing-enabled Gemini project.
- **Custom SearXNG:** requires an HTTPS instance whose JSON search API is enabled.

Web search is off by default and is enabled per message from the chat **+** menu. If a provider quota is exhausted or evidence is ambiguous, Phase continues normally and asks for a nutrition-label photo when exact product macros are required.

## Getting an API Key

### Gemini (Recommended Start)

1. Go to aistudio.google.com/apikey
2. Sign in with your Google account
3. Click **Create API Key**
4. Copy the key

### OpenRouter

1. Go to openrouter.ai/keys
2. Create an account
3. Navigate to **Keys**
4. Create a new key
5. Add credits

### OpenAI

1. Go to platform.openai.com/api-keys
2. Sign in
3. Navigate to **API Keys**
4. **Create new secret key**
5. Copy it (won't be shown again)

### Anthropic

1. Go to console.anthropic.com/settings/keys
2. Sign in
3. Navigate to **API Keys**
4. Create a new key

## Configuring in MacroPhase

1. Open **More** tab
2. Tap **AI** under Feature Settings
3. Select your **AI Provider**
4. Paste your API key
5. Select models for coaching, scanning, and insights

> **Security:** Your API key is stored in your device's encrypted preferences. It's never sent to MacroPhase (there is no MacroPhase server). Only transmitted to the provider you configured.

## The Food Scanner

Tap the camera icon in the Food Log tab, take a photo, and the AI identifies foods and estimates portions. Works best with good lighting and clearly visible, separated items.

You can configure a different image model in AI Settings if your provider supports it.

## Troubleshooting

- **"API key invalid"** — Check you copied the full key. Some providers need credits first.
- **Coach not responding** — Check internet (except Local Gemma). Try a different model.
- **Slow responses** — Free tiers may have rate limits. Try a faster model.
- **Scanner missing foods** — Photo from above, good lighting, separated items.
