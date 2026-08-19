# How the AI Coach Works

MacroPhase provides an intelligent conversational coach with long-term memory and direct tool integration to help you manage your nutrition and progress effortlessly.

## What It Can Do

The AI Coach is equipped with **33 agent tools** to take direct actions across the app:

### Food & Logging Tools
- Search offline databases, regional packs, and online food catalogs
- Log food with full macro and micronutrient breakdowns
- QuickLog natural language food descriptions
- Analyze meal photos and parse nutrition facts labels
- Delete or update recent food log entries
- Calculate macro fit against your remaining daily budget

### Smart Shopping & Pantry Tools
- View, add, and clear items on your Smart Shopping list
- Suggest grocery lists based on your recipes and protein targets

### Web Grounding & Research Tools
- Real-time web search (DuckDuckGo, Brave, Google, Bing, Jina, Tavily) for restaurant menus and product facts
- Fetch and summarize webpage recipes via `/browse`

### Weight & Strategy Tools
- Log scale weights and view 3-State trend weight projections
- Check progress toward your goal weight and upcoming milestones
- Propose and apply calorie/macro target adjustments

### Recipe & Memory Tools
- Create recipes and custom foods directly from chat
- Store and recall long-term preferences, favorite foods, and dietary constraints

---

## The 4-Layer Memory System

The coach remembers your context across conversations using a 4-layer local memory hierarchy:

1. **Profile Memory** — Persistent dietary preferences, gym routines, and calorie targets.
2. **Food Memory** — Frequently eaten foods, preferred brands, and aliases.
3. **Behavior Memory** — Observed patterns (e.g. late-night eating, weekend intake spikes).
4. **Episode Memory** — Short-term contextual events (e.g. vacation week, plateau, sickness) that automatically expire.

> 🔒 **Local & Private**
> Memories decay naturally over time if not reinforced and are stored 100% locally in your device's database. You can view, search, edit, or delete memories anytime in **More → AI Coach Memory**.

---

## Configuration

In **More → AI Settings**, you can configure:

- **AI Provider**: Gemini, OpenAI, OpenRouter, Anthropic, or Local Gemma (offline).
- **Web Grounding**: Choose DuckDuckGo (free/default) or your own search API key.
- **Personality & Focus**: Adjust coaching tone and focus (Fat Loss, Muscle Gain, Performance, Consistency).
