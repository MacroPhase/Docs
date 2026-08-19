# Privacy and API Usage

MacroPhase is built on a **local-first** principle. Your personal data stays on your device. AI features require sending some data externally, but you control what and when.

## What Stays on Your Device

Everything. Your food logs, weight entries, custom foods, favorites, recipes, body metrics, progress photos, memory, conversations, and settings — all stored in an encrypted SQLite database on your phone.

> **No cloud sync.** There is no MacroPhase server. Your data never touches our infrastructure because we don't have any.

## What Goes to an External API

When you use AI features:

- **AI Coach** — Your conversation messages, system context (current targets, recent logs), and tool results
- **Food Scanner** — Photo of your plate
- **Meal Analysis** — Text description of your meal

## What Is NOT Sent

- Your complete food log history
- Your weight history
- Your personal profile (age, height, sex)
- Your custom foods or recipes
- Your body metrics or photos
- Any data from non-AI features

## Where Data Goes

- **Gemini** → `generativelanguage.googleapis.com`
- **OpenAI** → `api.openai.com`
- **OpenRouter** → `openrouter.ai`
- **Anthropic** → `api.anthropic.com`
- **Local Gemma** → Nowhere. Runs on-device with zero network traffic.

## Your API Key

Stored in your device's encrypted preferences. Never sent to MacroPhase. Never shared with other apps. Only transmitted to the provider you configured.

## Data Retention

Each provider has its own policy:

- **Gemini:** Prompts not stored for training by default
- **OpenAI:** API data not used for training by default
- **OpenRouter:** Passes through to the underlying provider
- **Anthropic:** API data not used for training

> **Recommendation:** Check your provider's current policy. These change over time.

## Opting Out

Use MacroPhase without AI entirely — don't configure an API key, disable AI features in More → AI, and use manual logging, barcode scanning, and food search. The core app works completely offline.

## Note on Experimental Sync Endpoint

For full open-source transparency if you inspect network traffic or source code: MacroPhase includes a dormant cloud sync stub (`macrophase.up.railway.app`). This was an experimental sync prototype for invited beta testers. It is inactive/404 by default and does not receive or store any user data unless a custom sync server URL and auth JWT are explicitly configured in developer settings.

## Export and Deletion

- **Export** — CSV from More → Profile Settings → Data Management
- **Delete** — All data from More → Profile Settings → Data Management
- **Uninstall** — Deletes all local data

There is no persistent server-side profile to delete because your primary database lives entirely on your device.
