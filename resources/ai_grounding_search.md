# AI Web Grounding & Slash Commands

The AI Coach in MacroPhase is connected to real-time web search, enabling it to research restaurant menus, verify nutrition facts, find recipes, and answer questions beyond its offline training cutoff.

## Real-Time Web Grounding

When you ask the AI Coach about specific branded items, restaurant chains, or recent nutrition topics, it can browse the web to find verified, up-to-date facts:

- **Restaurant Menus**: *"What are the macros for a Chipotle burrito bowl with chicken, brown rice, black beans, and fajita veggies?"*
- **Packaged Items**: *"Find the nutrition label for the new Oikos Pro Greek Yogurt flavor."*
- **Culinary & Nutritional Research**: *"Look up how many grams of protein are typically in 100g of cooked tempeh vs seitan."*

---

## Search Providers & BYOK (Bring Your Own Key)

You can choose how the AI Coach searches the web in **Settings → AI Features → Web Grounding**:

1. **Zero-Key DuckDuckGo (Default)**: Works immediately out of the box with zero setup and no API key required.
2. **Bring Your Own Key (BYOK)**: Connect your own provider API key for faster latency and higher search depth:
   - **Brave Search**
   - **Google Custom Search**
   - **Bing Search**
   - **Perplexity AI**
   - **Tavily AI**

---

## AI Chat Slash Commands

Use slash commands directly in the AI Coach message bar for instant shortcuts:

- `/search <query>` — Forces the coach to execute a live web search for specific facts.
- `/browse <url>` — Fetches and parses the text contents of a public recipe or article URL.
- `/plan` — Generates a structured meal plan tailored to your remaining macro targets.
- `/clear` — Clears the active chat context for a fresh session without deleting long-term memory.
