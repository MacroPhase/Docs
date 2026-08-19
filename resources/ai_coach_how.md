# How the AI Coach Works

Most nutrition apps give you a food database and wish you luck. MacroPhase gives you a coach that actually understands your situation.

## What It Is

The AI coach is a conversational assistant that knows your recent food logs, weight trends, macro targets, and preferences. You chat with it naturally, and it can take actions on your behalf — logging food, searching the database, creating recipes, tracking your weight.

## What It Can Do

### Food Tools
- Search the food database (local + Open Food Facts)
- Log food with full macro breakdown
- View today's nutrition and history
- Delete recently logged foods
- Analyze meal photos from your camera
- Parse nutrition labels from photos
- Check how a food fits your remaining budget

### Weight & Body Tools
- Log weight entries (with optional backdating)
- View weight trends over any period
- Log body measurements

### Recipe Tools
- Create recipes from ingredients
- View saved recipes
- Log recipes to your diary

### Memory Tools
- Save your preferences and patterns
- Recall information about you
- Remember foods you eat frequently

> **Safety:** The coach asks for approval before making any changes to your data. Memory saves happen automatically since they don't modify your plan.

> 📊 **In MacroPhase**
> The coach has 33 tools available. It can search multiple food databases, analyze photos, create recipes, and track your progress — all from a single conversation.

## How the Memory Works

The coach remembers you across conversations through four memory layers:

1. **Profile memory** — Your preferences, goals, constraints
2. **Food memory** — Frequently eaten foods with aliases
3. **Behavior memory** — Observed patterns (e.g., "under-eats on weekends")
4. **Episode memory** — Time-bound context (e.g., "on vacation this week")

Memory decays over time if not reinforced. This keeps it relevant to your current situation rather than cluttered with outdated information.

> 💡 **Key Idea**
> The more you use the coach, the better it gets. After a few weeks, it knows you prefer chicken over beef, always log lunch around 1pm, and tend to under-eat on Tuesdays. That context makes its suggestions genuinely useful.

## The Conversation Loop

When you send a message:

1. Your message goes to the AI provider (Gemini, OpenAI, etc.)
2. The model can call tools (search food, log food, etc.)
3. Results come back to the model
4. The model may call more tools (up to 15 iterations)
5. The final response appears

The coach might search for food, check your remaining calories, and suggest an option — all in one response.

## Configuration

In **More → AI**, you can set:

- **Provider** — Which AI service
- **Coach model** — The conversation model
- **Image model** — The vision model
- **Personality** — Warm, strict, concise, or balanced
- **Response length** — Quick, normal, or detailed
- **Focus mode** — Fat loss, muscle gain, performance, consistency, or recovery
- **Memory level** — How much context the coach retains

## Privacy

The coach sends conversation messages to your API provider. This includes your messages, tool results, and system context (current targets, recent logs).

It does not send your complete food log history, weight history, or personal profile beyond what's relevant.

See the Privacy and API Usage article for the full picture.
