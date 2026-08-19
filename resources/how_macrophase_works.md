# How MacroPhase Works

Most people assume nutrition tracking is simple: eat less, lose weight. Then they have a week where the scale jumps 2 kg overnight after a pizza, panic, and think the plan stopped working.

MacroPhase is built around separating signal from noise. The app combines your food intake, weight logs, and check-ins to estimate what is actually happening beneath daily fluctuations.

## The Problem MacroPhase Solves

Most nutrition apps give you a calorie target from a calculator and never update it. But your body isn't static. As you lose weight, your expenditure drops. As you build muscle, it rises. Stress, sleep, and activity all shift the numbers.

A target that was perfect in week one is often wrong by week eight. MacroPhase fixes this by learning from your actual data instead of relying on population averages.

## How It Works

You do three things:

1. **Log what you eat.** Search foods, scan barcodes, or tell the AI coach what you had. Every entry captures calories, protein, fat, carbs, and micronutrients.

2. **Log your weight.** Step on the scale when you can. MacroPhase smooths out the noise to find your real trend — the direction your weight is actually moving, stripped of daily water swings.

3. **Complete your check-in.** Once a week, MacroPhase reviews your data and adjusts your targets if needed. You approve or override.

> 💡 **Key Idea**
> MacroPhase doesn't just track what you eat. It tracks what your body does in response, and adapts accordingly.

## How Your Targets Are Built

Your calorie target isn't a guess. It's the output of a chain:

- **Your profile** (age, height, weight, sex, activity level) produces a baseline energy expenditure estimate.
- **Your logged data** (food intake + weight trends) refines that estimate over time into a dynamic TDEE. You can choose between two weight trend algorithms — V1 (EWMA) for stability or V2 (3-State) for smoother noise rejection — in Settings → Algorithms.
- **Your goal** (lose, maintain, or gain) determines whether your target sits below, at, or above your expenditure. The V3 expenditure engine automatically accounts for your goal when interpreting weight changes, so starting a new diet doesn't distort your TDEE.
- **Your check-ins** fine-tune the target each week based on actual progress.

> 📊 **In MacroPhase**
> After 5+ days of consistent logging, your TDEE switches from a calculator estimate to a data-driven calculation. The more you log, the more accurate it gets.

## The AI Coach

The optional AI coach lets you chat naturally about your nutrition. It can:

- Log foods from descriptions like "200g chicken breast for lunch"
- Search the food database on your behalf
- Analyze meal photos
- Create and manage recipes
- Track your weight
- Answer nutrition questions

The coach has a memory system that learns your preferences, eating patterns, and habits over time. The more you use it, the more relevant its suggestions become.

> Your data stays on your device. The coach sends conversation messages to your chosen API provider, but food logs, weight entries, and personal data never leave your phone. See the Privacy and API Usage article for the full picture.

## What Makes It Different

- **Adaptive expenditure tracking** — Your TDEE updates as your body changes, not just when you recalculate
- **Transparent logic** — You can see the confidence level of your estimate and understand why targets change
- **No cloud dependency** — Your data lives locally. Export anytime as CSV. No account required.
- **Local-first food search** — USDA and Open Food Facts databases bundled offline. The API is a fallback, not a requirement.

## Getting Started

1. Complete onboarding with your profile details
2. Set your goal
3. Start logging food and weight
4. After 5+ days, your dynamic TDEE activates
5. Complete your first check-in

The app guides you through each step. No nutrition degree required.
