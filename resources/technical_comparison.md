# Technical Deep Dive: Nutrition Tracker Comparison

This article compares the technical architecture behind two adaptive nutrition trackers — the one you're using now and the one many of you migrated from. If you care about EWMA parameters, algorithm versioning, and data model design, this is for you.

## TDEE Calculation

Both apps use exponential weighted moving average to smooth weight trends and derive dynamic total daily energy expenditure. The math is well-established. The implementation details are where things diverge.

### Trend Weight Smoothing

The concept is identical: take daily scale readings, apply mathematical smoothing, and produce a "trend weight" that strips out water retention noise. Both apps now offer two smoothing speeds, though the implementations differ substantially.

The migrated app uses a **user-selectable alpha parameter** via a `TrendWeightAlpha` enum — EMA-9 (faster) and EMA-20 (slower). Both are causal EWMA filters that only look backward. They also expose the EMA-9 stream alongside the main trend, allowing users to compare two smoothing speeds.

MacroPhase offers two fundamentally different algorithms:
- **V1 (EWMA)** — causal exponential weighted moving average with alpha 0.1, identical in spirit to the migrated app's EMA-20
- **V2 (3-State)** — a 3-state Kalman filter tracking position, velocity, and acceleration as hidden states, followed by a Rauch-Tung-Striebel backward smoother and an EMA post-filter. This is a non-causal state-space model that separates transient water noise from genuine weight changes by modeling fluctuation as its own process noise channel.

The architectural difference is significant: the migrated app's V1 and V2 differ only in EMA speed (both are 1st-order causal filters), while MacroPhase's V1 and V2 are different *classes of algorithm* (1st-order EMA vs 3rd-order state-space model).

### Expenditure Algorithm

The migrated app maintains **three versions** of their expenditure algorithm (`ExpenditureAlgorithmV1`, `V2`, `V3`), with V3 introducing goal-timeline-aware adjustments via `GoalAdjustmentBudget` — rate-limited TDEE corrections that prevent starting a new diet from appearing as a metabolism spike.

MacroPhase now also maintains three versions via a factory pattern. V3 implements the same goal-budget mechanism: a per-goal-period budget initialized from the expected weight delta, depleted daily by capped adjustment steps (starting at ~21.4 kcal/day, scaling with goal magnitude), with the budget resetting on goal transitions. When no goal timeline is available (or when V1/V2 is selected), V3 falls back to the standard 14-day rolling window with tissue-density tables and allometric BMR baseline — producing identical output to V1.

The practical difference: both apps can now roll out algorithm improvements without breaking historical data, and both apply goal-aware TDEE adjustment during active dieting periods. MacroPhase's `GoalTimelineCursor` builds the timeline from stored goal history entries with the current settings goal as a fallback for the open-ended period.

### BMR Equations

The migrated app supports **two BMR equations** via `BmrEquationV1` and `BmrEquationV2`, selected by `BmrVersion`. This likely maps to Mifflin-St Jeor and Katch-McArdle (or similar), letting users choose based on whether they know their body fat percentage.

MacroPhase uses a single allometric BMR baseline that adapts over time. The equation itself is fixed, but the system adjusts as your logged data accumulates.

## Weight Tracking

### Scale Entry Model

Both apps store scale entries with a weight value, timestamp, and data source. The migrated app's `ScaleEntry` implements both `HasTimestamp` and `SourcedData` interfaces, tracking where the weight came from (manual entry, smart scale, Health Connect sync).

MacroPhase's `BodyWeightEntry` uses `LocalDate` as the key (not timestamp), while `WeightEntry` uses a `String` date. This dual-table design is a known technical debt item — two tables storing the same concept in different formats.

### Trend Weight

The migrated app's `TrendWeightEntry` is 32 bytes with a timestamp, exposed through multiple provider streams including the EMA-9 variant.

MacroPhase's trend weight is computed by the `WeightTrendAlgorithm` interface (V1: `TrendWeightEngine` EWMA, V2: `WeightTrendV2` Kalman + RTS + EMA) and exposed through `TrendSeries` objects that include velocity and acceleration estimates for downstream expenditure calculations. The chosen algorithm is user-configurable in Settings → Algorithms, with V2 (3-State) automatically enabling expenditure V3.

## Food Logging

### Data Model

The migrated app's `NutritionEntry` is ~260 bytes — one of the largest models in the codebase. It stores a full day's nutrition data in a single object, including individual micronutrient values as `Micronutrient` objects (title, unit, value, hint, additional data per 100g or daily value).

MacroPhase's food logging stores individual `FoodLogEntry` objects per food item, with daily aggregation computed on demand. Micronutrients are tracked but the model is less monolithic.

The architectural difference: the migrated app pre-aggregates daily totals into a single entry. MacroPhase computes them when needed. Pre-aggregation is faster for display but requires write-time computation on every log. On-demand computation is simpler to write but requires read-time aggregation.

### Nutrient Tracking

The migrated app treats micronutrients as first-class citizens with dedicated `Micronutrient` objects that include display hints and reference values. Their `NutrientNutritionDashboardDataTile` suggests nutrient-specific dashboard tiles — you can track individual vitamins and minerals with their own visualization.

MacroPhase tracks micronutrients but with less emphasis on per-nutrient dashboard tiles. The focus is more on macros with micros as supplementary data.

### Food Search

The migrated app uses **Typesense** — a search engine — with a `TypesenseConfig` pointing to multiple nodes for high availability. They use `TypesenseFoodNameQueryRequest` for name-based queries and `TypesenseDocumentFetchRequest` for fetching by ID, with LRU caching on top.

MacroPhase bundles food databases locally (CIQUAL 200K+ items) and uses `BundledDatabaseHelper` for offline search. Open Food Facts is available as an online fallback.

The tradeoff: Typesense provides faster, more sophisticated search (fuzzy matching, typo tolerance, relevance ranking) but requires a server. Local search works offline but with simpler query matching.

## Custom Foods

### Creation Flow

The migrated app uses a **wizard pattern** with a `CustomFoodWizardView` and step-by-step creation (general info step, nutrient step, etc.). They separate `CustomFoodMode` into create vs edit, and use `NutrientCalculation` with a `NutrientCalculationMethod` enum (likely per serving, per 100g, or total).

MacroPhase's custom food creation is integrated into the PremiumAddFoodSheet flow — you can save any food as a custom entry directly from the food logging sheet.

The migrated approach is more structured (dedicated wizard with clear steps). MacroPhase's is more contextual (create where you're already working).

### Food State

The migrated app has a `FoodState` enum (likely active/archived), suggesting custom foods can be deactivated without deletion. MacroPhase doesn't have an explicit food state enum — custom foods exist or they don't.

## Recipes

### Architecture

The migrated app's recipe system is substantial. The `RecipeWizardService` is 72 bytes — one of the largest services. Recipe creation runs in a **Dart isolate** (`recipePort: Port<Map<String, dynamic>>`), offloading nutritional aggregation to a background thread. This matters for complex recipes with many ingredients.

They support multiple import methods:
- **URL import** with a crawler system (`CrawlerClient`, `SinglePageCrawler`)
- **Image import** via AI parsing (`FirebaseRecipeImageParser`)
- **Text import** with structured parsing

Their recipe parser supports four schema formats: JSON-LD, Microdata, RDFa, and AI-powered HTML parsing. This is production-grade recipe import — it can scrape recipe data from almost any website.

MacroPhase's recipe system is simpler: create a recipe by adding ingredients, name it, get per-serving macros. No web import, no image parsing. The tradeoff is feature depth vs complexity.

### Recipe Viewer

Both apps have a recipe viewer. The migrated app's viewer uses `ChangeNotifierProvider` for state management. MacroPhase's uses `StateFlow` in the ViewModel pattern. Both display per-serving breakdowns.

## Check-Ins

### Architecture

The migrated app's check-in system is module-based with six coaching module types: introduction, partial logging, weigh-in, fasting, logging break, and program update. Each module has its own view and viewmodel, with priority and status tracking.

The `logging_break_module` is notable — it explicitly handles gaps in logging with `expenditurePauseStep` and `expenditureResumeStep`. When you stop logging for a few days, the migrated app pauses expenditure calculations and resumes them when you return, with coaching to explain what happened.

MacroPhase's check-in engine uses `CheckInEngine` with `GoalPlanner` and `BmrCalorieFloorPolicy`. The check-in reviews adherence, trend data, and goal alignment, then proposes target adjustments. You approve or override.

The migrated approach is more prescriptive (module-driven coaching that adapts to specific scenarios). MacroPhase's is more transparent (here's what changed, here's why, approve or not).

### Goal Adjustment

The migrated app's `GoalAdjustmentBudget` (40 bytes) suggests rate-limited goal changes — maximum calorie adjustments per check-in, preventing wild swings. Their `GoalTimelineCursor` in V3 expenditure explicitly tracks position through goal history.

MacroPhase adjusts goals through `GoalPlanner` with similar intent but different implementation.

## Goal System

### Goal Types

Both apps support the same fundamental goals: cut (lose), maintain, bulk (gain). The migrated app encodes these as `GoalType` with `toJson` serialization. They also have a separate `WeightChange` enum for rate of change (e.g., 0.25 lb/week, 0.5 lb/week).

MacroPhase uses `Goal` with similar types. The weight change rate is configured during goal setup.

### Goal Entry Model

The migrated app's `GoalEntry` is 68 bytes — a large model that likely stores start/end dates, target weight, rate, type, status, and associated macro targets. MacroPhase's goal tracking is spread across `GoalTimelineEntryEntity` in Room and computed on the fly.

## Programs / Macro Plans

### Program Types

The migrated app has four program types: low-fat, performance, balanced, and keto. Each maps to a `WeeklyPlan` — a `Map<WeekDay, MacroPlan>` with per-day customization. Their `WeeklyPlan` is 84 bytes with 7 static closures (one per day), confirming per-day macro flexibility.

MacroPhase doesn't use "programs" in the same way. Macro targets are set during goal creation and adjusted at check-ins. There's no preset program library — targets are computed from your TDEE and goal, not selected from a menu.

The migrated approach gives you templates to start from. MacroPhase computes everything from scratch. Templates are faster to set up; computation is more personalized.

### Protein Preference

The migrated app has a `ProteinPreference` enum, suggesting configurable protein targets (standard, high-protein, etc.) independent of the overall program. MacroPhase sets protein during onboarding and adjusts at check-ins.

### Calorie Floor

Both apps have calorie floor concepts. The migrated app has `CalorieFloor` and `DynamicFloor` enums — likely a minimum calorie threshold that prevents dangerously low targets. MacroPhase has `BmrCalorieFloorPolicy` in the check-in engine — ensuring targets never drop below basal metabolic rate.

## Body Metrics

### Tracked Metrics

The migrated app tracks 27+ body metrics with a `BodyMetric` enum and `BodyMetricCategory` for organization. Their `BodyEntry` is 100 bytes — the largest data model in the codebase — storing weight, body fat percentage, lean mass, and circumferences (waist, chest, arms, thighs, etc.) in a single entry.

MacroPhase tracks similar metrics through `BodyMetric` entities in Room: weight, body fat percentage, waist, chest, arms, and progress photos. The data is stored per-metric rather than in a monolithic entry.

### Body Fat Estimation

The migrated app has a `visualBodyFatProvider` that estimates body fat from visual assessment (taking sex as input). MacroPhase doesn't have this feature — body fat is entered manually or from a smart scale.

### Visibility Controls

Both apps let you configure which metrics are visible. The migrated app has `DataVisibility` settings and a `body_metrics_visibility_view.dart`. MacroPhase shows/hides metrics through the body metrics screen.

## Dashboard

### Architecture

The migrated app's dashboard is organized into **experiences** (sections) with 15 categories: body metrics, energy balance, expenditure, full body, goal progress, habits, macros, nutrient explorer, nutrition data manager, period, progress photos, scale weight, steps, weight trend.

They use **"hat" enums** for switchable view modes on certain tiles — you can toggle between consumed vs remaining views on the nutrition tile, or different energy balance presentations. This is a compact way to offer multiple perspectives without creating separate tiles.

MacroPhase's dashboard uses a section-based system with `DashboardSection` entities that can be reordered, shown, or hidden. The sections are: weight trend, weekly nutrition, calorie targets, body metrics, food log, and AI coach quick access.

### Tile Density

The migrated app offers three tile density options: small/standard, compact, and full. MacroPhase doesn't have explicit density settings — the layout is fixed with configurable section visibility.

### Chart Rendering

The migrated app caches chart rendering with `DataGraphChartCache` and uses `LegendItem` components for chart legends. They use Syncfusion charting libraries (syncfusion_flutter_charts, syncfusion_flutter_gauges). MacroPhase uses custom Compose Canvas charts built from scratch — no third-party charting library.

## Data Storage

### Migration App

Cloud-first with Firestore. The `DatabaseRepository` in `firestore_database.dart` handles all persistence. Local caching exists but the source of truth is cloud. Requires Firebase Auth for account creation.

### MacroPhase

Local-first with Room (SQLite). No cloud dependency. No account required. Data export as CSV. Import from other apps via CSV.

This is the most fundamental architectural difference. It affects offline capability, privacy, data ownership, and the entire authentication flow.

## Summary

| Feature | Migrated App | MacroPhase |
|---------|-------------|------------|
| TDEE algorithm | 3 versioned algorithms | 1 current algorithm |
| Trend weight alpha | User-selectable | Fixed (0.1, ~19-day half-life) |
| Dual trend streams | Yes (main + EMA-9) | No |
| BMR equations | 2 selectable equations | 1 adaptive equation |
| Food search | Typesense (cloud) | Bundled local databases |
| Recipe import | URL, image, text (AI-powered) | Manual ingredient entry |
| Recipe parsing | JSON-LD, Microdata, RDFa, AI | None |
| Programs | 4 preset types | Computed from TDEE + goal |
| Check-in coaching | 6 module types | Transparent adjustment |
| Body metrics | 27+ in monolithic entry | Per-metric storage |
| Dashboard | 15 experience types, 3 densities | 6 configurable sections |
| Charts | Syncfusion library | Custom Canvas |
| Data storage | Firestore (cloud) | Room (local) |
| Account required | Yes | No |
| Algorithm versioning | Yes (V1/V2/V3) | No |
| Calorie floor | Dual enum system | BMR-based policy |

Neither approach is objectively better. The migrated app offers more configurability and algorithm transparency for power users. MacroPhase offers simplicity, local-first data, and opinionated defaults. The choice depends on whether you want to tune the knobs or trust the defaults.
