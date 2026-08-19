# Data Storage

Everything MacroPhase stores lives on your device. No cloud. No account. No backend server.

## What's Stored

### Nutrition Data

- **Food logs** — Every entry with 30+ nutrient columns
- **Weight entries** — Daily weigh-ins
- **Custom foods** — Your created items
- **Favorites** — Hearted foods
- **Search cache** — Cached API results (30-day TTL)
- **Scanned products** — Barcode cache (max 2,000)
- **Serving memory** — Per-food serving preferences

### AI & Memory

- **User profile memory** — Preferences, goals, constraints
- **Food memory** — Frequently eaten foods with aliases
- **Behavior memory** — Observed patterns
- **Episode memory** — Time-bound context (auto-expires)
- **Food picks** — Time-based suggestions
- **Conversations** — Coach chat sessions
- **Messages** — Individual chat messages

### Body & Progress

- **Body metrics** — Body fat %, measurements
- **Progress photos** — File paths and metadata
- **Check-in records** — Weekly history
- **Subjective scores** — Hunger, energy, sleep, stress
- **Goal timeline** — Goal change history

### Recipes

- **Recipes** — Metadata
- **Ingredients** — Components with nutrition data

## Database Details

- **Engine:** Room (SQLite)
- **Version:** 22
- **Migrations:** 20 over the app's lifetime
- **Location:** App-private storage

## Settings

Preferences (theme, units, AI provider, API keys) are stored in Android DataStore, separate from the Room database.

## Progress Photos

Saved as JPEG files in `{filesDir}/progress_photos/{date}_{angle}.jpg`. The database stores only the file path. Deleting a photo removes both the record and the file.

## Data Integrity

Room enforces type safety, migration-based schema upgrades, foreign key constraints (recipe ingredients cascade-delete), and unique indices (one weight entry per date).

## Accessing Your Data

- **In-app** — Dashboard, Food Log, Progress, Memory screens
- **Export** — CSV from More → Profile Settings → Data Management
- **AI Coach** — Can query via tools (only what it needs for the current conversation)
