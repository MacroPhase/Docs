# Setting Up AI Features

MacroPhase's AI features — the coach, food scanner, and meal analysis — work best with an API key. Here's how to set it up, which models to pick, and how web grounding works.

## Quick Strategy

- **Web Grounding (Default: DuckDuckGo)**: Live web search is completely free and enabled by default via DuckDuckGo (no API key needed). You can optionally connect BYOK providers (Jina, Tavily, Gemini 3, or SearXNG) in **Settings → AI Features → Web Grounding**.
- **Ultra-budget paid:** Use OpenRouter. It taps into efficient models like DeepSeek and Xiaomi for fractions of a cent.

## Supported Providers

- **Gemini** — Default. Free tier available. Requires API key.
- **OpenAI Compatible** — GPT-5.4-mini and others. Requires API key.
- **OpenRouter** — Many models including budget options. Requires API key.
- **Anthropic** — Claude models. Requires API key.
- **Local Gemma** — Fully offline, on-device. No API key.

## Live Top Models by Use Case (Auto-Updating)

<div id="dynamic-ai-widget" style="background:#1E1E20; border:1px solid #2E2E30; border-radius:14px; padding:18px; margin:20px 0; font-family: sans-serif;">
  <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:12px;">
    <h3 style="color:#fff; margin:0; font-size:16px;">⚡ Live Models from OpenRouter</h3>
    <select id="usecase-select" onchange="renderModels()" style="background:#2A2A2C; color:#fff; border:1px solid #3A3A3C; border-radius:8px; padding:6px 12px; font-size:13px; cursor:pointer;">
      <option value="coach">AI Coach (Tool Calling & Fast)</option>
      <option value="vision">Food Scanner (Vision & Multimodal)</option>
      <option value="budget">Ultra-Budget / Background (Lowest Cost)</option>
    </select>
  </div>

  <div id="ai-loading" style="color:#A1A1A6; font-size:13px;">Fetching latest models from OpenRouter API...</div>

  <table id="ai-table" style="width:100%; display:none; border-collapse:collapse; font-size:13px; color:#fff; margin-top:8px;">
    <thead>
      <tr style="border-bottom:1px solid #2E2E30; color:#A1A1A6; text-align:left;">
        <th style="padding:8px;">Model ID (Paste into App)</th>
        <th style="padding:8px;">Context</th>
        <th style="padding:8px;">Input / 1M</th>
        <th style="padding:8px;">Output / 1M</th>
      </tr>
    </thead>
    <tbody id="ai-body"></tbody>
  </table>
</div>

<script>
  let allModels = [];

  async function fetchOpenRouter() {
    try {
      const res = await fetch('https://openrouter.ai/api/v1/models');
      const json = await res.json();
      allModels = json.data || [];
      renderModels();
    } catch(e) {
      document.getElementById('ai-loading').innerText = 'Check OpenRouter for live status.';
    }
  }

  function renderModels() {
    if (!allModels.length) return;
    const useCase = document.getElementById('usecase-select').value;
    let filtered = [];

    if (useCase === 'coach') {
      // Must support tools (function calling), max $1.50/M input, min 16k context
      filtered = allModels.filter(m => {
        const cost = parseFloat(m.pricing?.prompt || 0) * 1000000;
        const tools = m.supported_parameters && m.supported_parameters.includes('tools');
        return tools && cost > 0 && cost <= 1.50 && m.context_length >= 16000;
      }).sort((a, b) => (parseFloat(a.pricing.prompt) - parseFloat(b.pricing.prompt)));
    } 
    else if (useCase === 'vision') {
      // Must support vision/image input, max $2.00/M input
      filtered = allModels.filter(m => {
        const cost = parseFloat(m.pricing?.prompt || 0) * 1000000;
        const isVision = m.architecture?.modality?.includes('image') || m.description?.toLowerCase().includes('vision');
        return isVision && cost <= 2.00;
      }).sort((a, b) => (parseFloat(a.pricing.prompt) - parseFloat(b.pricing.prompt)));
    } 
    else if (useCase === 'budget') {
      // Lowest cost (< $0.30/M) with at least 32k context
      filtered = allModels.filter(m => {
        const cost = parseFloat(m.pricing?.prompt || 0) * 1000000;
        return cost > 0 && cost <= 0.30 && m.context_length >= 32000;
      }).sort((a, b) => (parseFloat(a.pricing.prompt) - parseFloat(b.pricing.prompt)));
    }

    const tbody = document.getElementById('ai-body');
    tbody.innerHTML = '';

    filtered.slice(0, 6).forEach(m => {
      const inPrice = '$' + (parseFloat(m.pricing.prompt) * 1000000).toFixed(2);
      const outPrice = '$' + (parseFloat(m.pricing.completion) * 1000000).toFixed(2);
      const contextK = Math.round(m.context_length / 1024) + 'k';
      tbody.innerHTML += `
        <tr style="border-bottom:1px solid #2E2E30;">
          <td style="padding:8px; font-weight:bold; color:#8B5CF6;">
            <div>${m.name}</div>
            <code style="font-size:11px; color:#A1A1A6; background:#151517; padding:2px 6px; border-radius:4px;">${m.id}</code>
          </td>
          <td style="padding:8px; color:#A1A1A6;">${contextK}</td>
          <td style="padding:8px;">${inPrice}</td>
          <td style="padding:8px;">${outPrice}</td>
        </tr>
      `;
    });

    document.getElementById('ai-loading').style.display = 'none';
    document.getElementById('ai-table').style.display = 'table';
  }

  fetchOpenRouter();
</script>

> 💡 **Key Idea**
> You don't need to pick the same model for everything. Use Gemini's free tier for the coach and scanner, and upgrade specific features if you need more capability.

## Grounded Product & Web Search

MacroPhase first searches its local databases and Open Food Facts at no cost. For exact restaurant items or branded products that are still missing, the coach can search the web:

- **DuckDuckGo (Free & Default):** Built-in zero-key search. No configuration or API key needed.
- **Jina:** Free starter tokens; returns clean markdown page contents for AI grounding.
- **Tavily:** Free monthly credits with personal API key; structured web research.
- **Gemini 3:** Native Google search grounding with a Gemini API key.
- **Custom SearXNG:** Self-hosted open-source meta-search instance.

Web search is seamlessly integrated into the coach and can also be triggered explicitly via the `/search` and `/browse` slash commands.

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

> **Security:** Your API key is stored in your device's encrypted preferences. It is never sent to any intermediary server — requests go directly from your phone to the configured AI provider.

## Troubleshooting

- **"API key invalid"** — Check you copied the full key. Some providers need credits first.
- **Coach not responding** — Check internet (except Local Gemma). Try a different model.
- **Slow responses** — Free tiers may have rate limits. Try a faster model.
- **Scanner missing foods** — Photo from above, good lighting, separated items.
