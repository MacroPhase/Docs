# Data Storage

Everything MacroPhase stores lives entirely on your device. No cloud account. No central server. Complete local privacy.

## What's Stored Locally

### Nutrition & Food
- **Food logs** — Every entry with 30+ nutrient columns
- **Weight entries** — Daily weigh-ins and scale sources
- **Custom foods** — Created items with custom tags and emojis
- **Favorites** — Curated foods with custom tags
- **Search cache** — Cached API results (30-day TTL)
- **Scanned products** — Barcode cache
- **Serving memory** — Per-food serving preferences and custom servings

### Smart Shopping & Pantry
- **Shopping lists & items** — Active grocery lists, categories, and checked items
- **Scanned receipts** — OCR line items, prices, currencies, and timestamps
- **Pantry inventory** — In-stock items and purchase history for restocking

### AI & Long-Term Memory
- **User profile memory** — Preferences, goals, dietary restrictions
- **Food memory** — Frequently eaten foods with aliases
- **Behavior memory** — Observed meal timing and adherence patterns
- **Episode memory** — Contextual events (auto-expiring)
- **Food picks** — Time-of-day suggestions with weekly decay
- **Conversations & messages** — Chat history with the AI Coach

### Body & Progress
- **Body metrics** — Body fat %, waist, chest, arms circumferences
- **Progress photos** — JPEG files in app-private storage (`{filesDir}/progress_photos/`)
- **Check-in records** — Weekly check-in reviews and adjustments
- **Subjective scores** — Hunger, energy, sleep, and stress ratings
- **Goal timeline** — Dynamic goal history

### Recipes
- **Recipes & ingredients** — Multi-ingredient components, total prepared weights, and serving portions

---

## Database Details

- **Engine:** Room (SQLite)
- **Entities:** 22 local tables
- **Location:** App-private encrypted storage
- **Settings:** Preferences and API keys stored in Android DataStore

## Data Management & Exports

- **Full Export / Backup**: Export all database tables as standard CSV from **More → Settings → Data Management → Export Data**.
- **Developer Wipes**: Granular data wipe controls in **Settings → Developer Settings**.
