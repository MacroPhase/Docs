# Food Databases & Search Engine

MacroPhase features a custom-built, local-first search engine that indexes multiple international databases and adapts to your eating habits.

## The Search Engine: SQLite FTS5 + BM25

Rather than running slow database queries or relying on external cloud servers, MacroPhase compiles all your installed food databases into a unified **SQLite FTS5 (Full-Text Search 5) Inverted Index**:

- **Sub-Millisecond Speed**: Instant prefix matching and diacritic normalization (accents, umlauts) as you type.
- **BM25 Relevance Ranking**: Standard information-retrieval scoring ensures the most textually relevant matches appear first.
- **Behavioral Re-Ranking**: Dynamically boosts results based on your personal usage:
  - **Exact Name Match**: $+3,000$ boost
  - **Brand Match**: $+2,000$ boost
  - **Lifetime Log Frequency**: Up to $+1,500$ boost (your favorite staples always beat out obscure items)
  - **Favorited Items**: $+1,000$ boost
  - **7-Day Recency**: $+500$ boost

---

## Bundled & Regional Offline Databases

Install or manage regional packs from **More → Food Databases** for complete offline search coverage:

- **USDA Foundation Foods** — 6,000+ whole foods with 38 nutrients.
- **Swedish Food Database (Livsmedelsverket)** — Verified Swedish food composition data.
- **Korean Food Composition DB (RDA 10th Rev & K-FIND)** — Extensive Korean food and brand coverage.
- **South Asia DB (IFCT 2017)** — Indian and South Asian regional ingredients and dishes.
- **CIQUAL** — French food database, 3,200+ foods with complete micronutrient profiles.
- **CoFID** — UK database, strong vitamin and mineral data.
- **BEDCA** — Spanish commercial foods, 29,600+ items.
- **AUSNUT** — Australian/NZ database, 3,700+ foods.
- **Open Food Facts Regional Packs** — Thousands of products for North America, Western Europe, Northern Europe, Eastern Europe, UK & Ireland, Oceania, East Asia.

> 💡 **Key Idea**
> Installing regional packs downloads them to your local FTS5 search index. Barcode scanning and text search work completely offline at airplane mode speeds.

---

## Online Web Search Fallback

If an item is not found in your offline databases, MacroPhase can search the live **Open Food Facts API** or use the AI Coach's **Web Grounding** (DuckDuckGo, Brave, Google, Bing) to find and verify nutrition facts in seconds.
