# Technical Deep Dive: Nutrition Tracker Comparison

This article compares the technical architecture behind two adaptive nutrition trackers — MacroPhase and traditional cloud-based trackers (like MacroFactor). If you care about EWMA parameters, Kalman filtering, state-space models, algorithm versioning, and local-first data architecture, this is for you.

---

## TDEE & Trend Weight Calculation

Both apps derive a dynamic Total Daily Energy Expenditure (TDEE) by coupling daily scale weights with food intake. However, their mathematical foundations and algorithms diverge significantly.

### Trend Weight Smoothing

- **Traditional Tracker**: Uses a causal 1st-order Exponential Weighted Moving Average (EWMA) with selectable alpha speeds (EMA-9 and EMA-20). Causal filters look only backward in time.
- **MacroPhase**: Offers two selectable algorithm classes in **Settings → Algorithms**:
  - **V1 (EWMA)** — Causal exponential weighted moving average with $\alpha = 0.1$ (~19-day half-life).
  - **V2 (3-State Kalman Filter)** — A non-causal 3rd-order state-space model tracking **Position** (true weight), **Velocity** (rate of change in kg/day), and **Acceleration** (change in rate). It runs a forward Kalman filter followed by a Rauch-Tung-Striebel (RTS) backward smoother and an EMA post-filter. Transient scale noise (water retention, sodium spikes, big meals) is filtered out without lagging behind genuine weight changes.

### Expenditure Algorithms

- **Traditional Tracker**: Maintains versioned expenditure engines (V1, V2, V3) with rate-limited goal-timeline budgets to prevent diet start-up distortions.
- **MacroPhase**: Implements a modular versioned engine:
  - **V1 / V2**: Rolling 14-day energy expenditure window with non-linear tissue density tables and adaptive allometric BMR baselines.
  - **V3 (Goal-Aware)**: Integrates the 3-State trend velocity vectors with a dynamic goal budget cursor (`GoalAdjustmentBudget`). When switching phases (e.g. from maintenance to deficit), the algorithm prevents sudden artificial metabolic drops caused by initial glycogen/water depletion.
  - **Incomplete Days & Gaps**: Automatically freezes and excludes partial logging days from rolling averages, ensuring skipped meals or fasting days do not skew your expenditure.

---

## Food Logging & Regional Databases

### Search Architecture

- **Traditional Tracker**: Cloud-hosted Typesense cluster with remote indexing. Highly responsive, but requires an active internet connection.
- **MacroPhase**: On-device **Unified SQLite FTS5 (Full-Text Search 5) Inverted Index** (`FoodSearchIndex` + `FoodSearcher`):
  - **BM25 & Multi-Factor Behavioral Ranking**: Computes BM25 textual relevance combined with lifetime log frequency boosts, brand matching, exact name boosts, and 7-day recency.
  - **Bundled & Regional Offline Packs**: Indexes USDA Foundation, Swedish Livsmedelsverket, Korean RDA 10th Rev (K-FIND), South Asian IFCT 2017, and Open Food Facts regional packs.
  - **Live Web Grounding Fallback**: When an item isn't in local databases, the AI Coach uses real-time web search (DuckDuckGo, Brave, Google, Bing, Jina, Tavily) to verify exact brand nutrition facts before saving.

---

## Recipes & Custom Foods

### Custom Foods

- **Traditional Tracker**: Step-by-step custom food wizard.
- **MacroPhase**: Multi-step AI Creation Wizard (`CreateFoodMethodScreen`):
  - **Dual-Photo OCR**: Takes front photo (brand/name) and back photo (nutrition facts panel) simultaneously.
  - **Tagging & Custom Servings**: Custom food tags (e.g. *High Protein*, *Snack*) and custom serving heuristics.

### Recipe System

- **Traditional Tracker**: Recipe crawler (JSON-LD, Microdata) and AI photo parser.
- **MacroPhase**:
  - **AI Recipe Vision & Description**: Snap a picture of a meal or paste recipe notes (`CreateRecipeWithAIScreen`) to extract ingredients, gram portions, and macros automatically.
  - **URL Browsing**: AI Coach can fetch public recipe URLs via `/browse` and parse ingredients into your recipe library.

---

## Smart Shopping & Receipt Scanner

| Feature | Traditional Tracker | MacroPhase |
|---|---|---|
| **Shopping List** | None (Third-party app needed) | Built-in local-first Smart Shopping with pantry tracking & restock alerts |
| **Receipt Scanner** | None | AI camera/gallery receipt OCR with batch item approval & 1-tap custom food import |
| **AI Integration** | None | AI Coach can generate grocery lists directly from your meal plans |

---

## AI Coach & Local Memory Architecture

- **Traditional Tracker**: Rule-based check-in algorithms and static coaching modules.
- **MacroPhase**: Local-first Conversational AI Assistant with a 4-layer Room memory hierarchy:
  1. `user_profile_memory` — Persistent preferences, dietary restrictions, and macro goals.
  2. `food_memory` — Frequency-ranked food items and aliases.
  3. `behavior_memory` — Observed logging consistency and meal patterns.
  4. `episode_memory` — Short-term contextual events (vacation, diet breaks, training cycles).
  - Equipped with **33 agent tools** (logging, recipe creation, memory recall, web search, shopping lists).

---

## Architectural Comparison Summary

| Technical Dimension | Traditional Tracker | MacroPhase |
|---|---|---|
| **Data Storage** | Cloud-First (Firestore / Firebase) | **Local-First (Room SQLite)** |
| **Account / Login** | Mandatory account & login | **Zero account required (100% private)** |
| **Trend Weight Algorithm** | 1st-Order Causal EWMA (EMA-9 / EMA-20) | **V1 (EWMA) & V2 (3-State Kalman + RTS Smoother)** |
| **Expenditure Engine** | V1 / V2 / V3 with Goal Timeline | **V1 / V2 / V3 Goal-Aware with Velocity Vectors** |
| **Incomplete Day Handling** | Logging break module | **Automatic break detection & expenditure freeze** |
| **Food Databases** | Cloud Typesense Cluster | **Bundled offline packs + live web grounding** |
| **Recipe AI & Import** | Web scraping & AI image parsing | **AI photo scan, natural language parser, & web browse** |
| **Smart Shopping List** | Not available | **Local-first shopping list & AI receipt scanner** |
| **AI Assistant & Memory** | Not available | **AI Coach with 4-layer memory & 33 agent tools** |
| **Chart Rendering** | Third-party charting library | **Custom Jetpack Compose Canvas charts** |
| **Home Screen Widgets** | Standard macro/weight widgets | **Glanceable rings, Weight, & Quick Ask Coach widget** |
