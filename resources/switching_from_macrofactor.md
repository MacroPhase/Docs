# Switching from Other Nutrition Apps

If you're coming from another adaptive nutrition tracker, you already understand the core concepts — dynamic TDEE, macro targets, weekly check-ins. MacroPhase builds on that foundation with several key differences that matter in daily use.

## Data Stays on Your Device

The biggest philosophical difference: your data lives locally, period.

Other apps require account creation, cloud sync, and server-side storage. Every food log, weight entry, and check-in travels through their servers before reaching your screen. MacroPhase stores everything in a local database on your phone. No account required. No cloud dependency.

When you export your data, you get a CSV file — not a proprietary format locked inside their ecosystem. You can open it in any spreadsheet, import it into another app, or just keep it as a backup.

> 🔒 **Why this matters**
> Nutrition data is personal. It reveals your eating habits, body composition, health conditions, and daily routines. MacroPhase treats that data as yours, not as a product to store on someone else's server.

## Food Search Without a Server

MacroPhase bundles food databases directly on your device — over 200,000 items from CIQUAL and Open Food Facts, searchable offline. No internet connection needed. No API rate limits. No waiting for results to load.

Other trackers rely on cloud-based search engines like Typesense or proprietary backends. When their servers are slow or down, food search breaks. MacroPhase's search works on a plane, in a basement gym, or anywhere with a weak signal.

Barcode scanning uses ML Kit on-device. The food database includes regional packs, so you can download only the data relevant to your country and skip what you don't need.

## An AI Coach That Actually Does Things

Most nutrition apps bolt on an AI chatbot that answers questions. MacroPhase's coach is an agent — it doesn't just suggest, it acts.

The coach has 18+ tools it can use during a conversation:

- **Log food** directly from a description ("had 200g chicken breast with rice for lunch")
- **Search the database** on your behalf
- **Analyze meal photos** with per-item macro breakdowns
- **Parse nutrition labels** from a photo
- **Create and manage recipes**
- **Query your weight trends** and expenditure data
- **Adjust your calorie targets** based on your goals
- **Access Health Connect** data (steps, sleep, heart rate)

This is the difference between an AI that tells you what to do and one that actually does it for you.

### Dual AI Architecture

MacroPhase supports two AI modes:

1. **Cloud AI** — Connect your own Gemini, OpenAI, Anthropic, or OpenRouter API key. Full-powered models with web search capabilities.
2. **On-device AI** — Gemma 4 runs entirely on your phone via LiteRTLM. No API key needed. No data leaves your device. Works fully offline.

You can configure which model handles which task. Want the cloud for photo analysis but local for food logging? That's a setting, not a compromise.

Other apps lock you into their proprietary AI backend with usage limits and opaque pricing. MacroPhase lets you bring your own key and choose your provider.

## A Memory System That Learns

MacroPhase's coach maintains four layers of memory that persist across conversations:

- **Profile Memory** — Your preferences, goals, diet type, and phase
- **Food Memory** — Frequently eaten foods with aliases and frequency ranking
- **Behavior Memory** — Observed patterns like late-night eating, weekend deviations, adherence trends
- **Episode Memory** — Short-term context that expires (vacation, plateau, poor sleep streak)

These memories decay naturally over time and are relevance-scored when retrieved. The coach doesn't dump your entire history into every prompt — it pulls what's relevant to the current conversation.

This is fundamentally different from stateless coaching modules that reset each session. The coach remembers that you prefer high-protein breakfasts, that you tend to overeat on weekends, and that you mentioned a plateau last Tuesday — without you repeating it.

## Adaptive Expenditure Tracking

Both apps use exponential weighted moving average (EWMA) to smooth your weight trend and calculate dynamic TDEE. The math is similar. The implementation details differ.

MacroPhase's TDEE engine uses a 14-day rolling window with a minimum of 5 logged days before switching from calculator estimates to data-driven calculations. Trend weight uses a ~19-day half-life (alpha 0.1). The engine accounts for non-linear tissue density and uses an allometric BMR baseline.

You can see the confidence level of your estimate and understand exactly why targets change. No black boxes.

## Weekly Check-Ins

MacroPhase's check-in system reviews your logged data weekly and adjusts targets based on actual progress. You approve or override every change.

The check-in engine includes:

- **Adherence tracking** — How closely you followed your targets
- **Trend analysis** — Whether your weight is moving in the right direction
- **Target adjustment** — Calorie and macro modifications based on results
- **Goal alignment** — Ensuring changes match your stated goal (lose, maintain, gain)

The check-in is transparent. You see the reasoning before accepting any change.

## Dashboard Customization

MacroPhase gives you direct control over what appears on your dashboard. Rearrange sections, show or hide widgets, and configure what each tile displays.

The dashboard supports:

- **Weight trend visualization** with scale weight overlay
- **Weekly nutrition summaries** with macro breakdowns
- **Calorie and macro targets** with remaining/consumed views
- **Body metrics** tracking (measurements, body fat, progress photos)
- **Food logging timeline** for the current day
- **AI coach quick access**

No focus modes to toggle between. No confusing tile density options. Just a clean, configurable layout that shows what you care about.

## Food Logging Flexibility

MacroPhase supports multiple ways to log food:

- **Search** — Type a food name, get instant results from the local database
- **Barcode scan** — Point your camera at a package
- **AI photo analysis** — Take a picture of your plate, get per-item macros
- **AI conversation** — Tell the coach what you ate in natural language
- **Custom foods** — Create and save your own entries
- **Recipes** — Multi-ingredient dishes with per-serving calculations
- **Favorites** — Quick-access hearted foods
- **Picks** — Time-based suggestions that learn what you eat and when

The natural serving size system translates "1 chicken breast (166 grams)" into meaningful portions instead of forcing you to think in raw grams.

## What You Won't Miss

If you're switching from a cloud-dependent tracker, here's what changes:

| Cloud Tracker | MacroPhase |
|---|---|
| Account required | No account needed |
| Data stored on their servers | Data stays on your device |
| AI features require their API | Bring your own API key or use on-device AI |
| Food search needs internet | 200K+ items searchable offline |
| Subscription required for features | All features included |
| Export in proprietary format | Export as CSV anytime |
| AI has usage limits | No limits with your own API key |

## What You'll Notice First

The switch is straightforward because the core workflow is the same: log food, log weight, check in weekly. The differences show up in the details:

- **Speed** — Local search is instant. No loading spinners.
- **Offline** — Everything works without a connection.
- **Privacy** — Your data never leaves your phone unless you choose to share it.
- **Control** — You own your data, your AI provider, and your workflow.

## Getting Started

1. Complete onboarding with your profile details
2. Import your previous data via CSV (see the Backup and Restore article)
3. Set your goal and coaching program
4. Start logging food and weight
5. After 5+ days, your dynamic TDEE activates
6. Complete your first check-in

The transition takes about five minutes. Your data is yours from day one.
