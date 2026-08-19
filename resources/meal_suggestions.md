# Favorites & Picks

MacroPhase provides two distinct systems for rapid food logging: **Favorites** (your personally curated staples) and **Picks** (intelligent time-of-day suggestions).

## Favorites — Your Curated List

Favorites are items you explicitly heart in the food logger:

- **Quick Access**: Pinned at the top of your search results.
- **Custom Tagging**: Organize favorites with tags (e.g. *High Protein*, *Snacks*, *Zero Calorie*).
- **Custom Emojis**: Assign custom emojis to recognize your go-to items instantly.

### When to Use Favorites

- Daily staples (chicken breast, egg whites, whey protein, rice)
- Custom verified brand entries you trust

---

## Picks — Time-Based Suggestions

Picks learn your eating routine automatically from your logged history:

### How It Works

Every time you log a meal, MacroPhase records what you ate, the quantity, and the exact hour of day:

1. **Current Hour First**: When you open the logger at 12:00, it suggests foods you typically eat at 12:00.
2. **Progressive Fallback**: If you haven't logged enough foods at that exact hour, it smoothly expands to $\pm 1$ hour and $\pm 2$ hours.
3. **Decay**: Unused picks fade by 2% per week, automatically purging obsolete habits.

### Favorites vs Picks

| Property | Favorites | Picks |
|---|---|---|
| **Intent** | User-curated (hearted) | Algorithmic (time-of-day learned) |
| **Serving** | Saved at favoriting time | Uses your average serving grams |
| **Location** | Favorites bar & tag filters | Hourly Picks section on Log Food screen |
| **Management** | Manual tap to heart/unheart | Self-decaying & manageable in **More → AI Memory** |
