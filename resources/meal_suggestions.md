# Favorites & Picks

MacroPhase has two systems for quickly finding foods you eat regularly: **Favorites** (foods you deliberately save) and **Picks** (algorithmic suggestions based on when you eat them).

## Favorites — Your Curated List

Favorites are foods you heart in the food logger. They show up at the top of your search results and are always available, regardless of time of day.

### How to Add a Favorite

When searching for food, tap the heart icon next to any item. It's saved instantly.

### When to Use Favorites

- Staples you eat almost every day (chicken breast, rice, oats)
- Go-to snacks you don't want to search for each time
- Foods with specific brands or entries you've verified are accurate

> 💡 **Key Idea**
> Favorites are your short list. If you're searching for the same food more than twice a week, favorite it. The 2 seconds it saves per meal adds up to hours over a year.

## Picks — Time-Based Suggestions

Picks are smarter than they look. Every time you log a food, MacroPhase records what you ate, when, and how much. Over time, it builds a picture of your eating routine and suggests foods you're likely to want at any given hour.

### How It Works

When you open the food logger, MacroPhase looks for foods you've logged at the current hour. If there aren't enough suggestions, it expands:

1. **Exact hour** — Foods you've logged at this specific time
2. **±1 hour** — The hour before and after
3. **±2 hours** — Further expansion if still sparse

Results are ranked by how often you eat them at that time.

### Where Picks Show Up

- **Log Food screen** — The "Picks" section
- **AI Coach** — The coach can query your picks via the `get_hour_picks` tool
- **Dashboard** — Suggestion cards

### Favorites vs Picks

- **Favorites** — You control them. Always available. You heart a food to add it.
- **Picks** — Algorithmic. Time-relevant. Automatically tracked from your logs. Decay over time if unused.

Both are useful. Favorites are your go-to staples. Picks are the app learning your routine.

## Meal Suggestions

When you log 3+ foods within 10 minutes, MacroPhase may offer to save the combination as a recipe. This catches your go-to meals before you forget the exact composition.

Configurable in **More → App Settings → Recipe Suggestions**.

## Managing Your Data

- **View favorites** — Library tab, Favorites section
- **Remove a favorite** — Tap the heart again to unheart
- **View picks** — Memory screen from bottom navigation
- **Delete a pick** — Long-press or swipe
- **Clear all picks** — "Clear All" option in Memory screen
- **Auto-decay** — Unused picks fade at ~2% per week

> 📊 **In MacroPhase**
> Picks are self-maintaining. Frequent foods stay at the top. Foods you stop eating gradually fade. You don't need to actively manage them — just log consistently and the system handles the rest.
