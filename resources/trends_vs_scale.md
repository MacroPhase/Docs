# Interpreting Trends vs Scale Weight

When you step on the scale, you see a single number. That number is influenced by water, food, sodium, sleep, stress, and a dozen other factors that have nothing to do with actual fat gain or loss.

The **trend weight** exists because your body doesn't change overnight — but the scale makes it look like it does.

## Two Numbers, Two Stories

- **Scale weight** — What the scale says today. Fluctuates 2-5 lbs daily. Reacting to this is like checking the stock market every hour and panicking.
- **Trend weight** — A smoothed average of your weigh-ins over time. This reflects what's actually happening to your body composition.

## Why the Trend Is More Honest

> **Example week:**
> - Monday: Scale 180.0
> - Tuesday: Scale 181.5
> - Wednesday: Scale 182.0
> - Thursday: Scale 180.5
> - Friday: Scale 183.0
> - Saturday: Scale 180.0
> - Sunday: Scale 179.5
>
> The scale swung by 3.5 lbs. If you panicked on Friday, you would have been wrong. The trend showed you were roughly maintaining the whole time.

The trend uses a mathematical smoothing algorithm to filter out daily noise. MacroPhase supports two: **V1 (EWMA)** — a simple weighted average that only looks at past weigh-ins — and **V2 (3-State)** — a more advanced model that tracks weight, rate of change, and acceleration as separate signals to produce an even smoother line.

> See the [Weight Trend Algorithms](weight_trend_algorithms) article for a full comparison.

## How to Read the Trend

- **Trending down consistently** — You're in a deficit. Keep going.
- **Trending up consistently** — You're in a surplus. Adjust if that's not the goal.
- **Flat** — You're at maintenance.
- **Slight oscillation** — Normal. The trend responds slowly to real changes.
- **Wrong direction despite adherence** — Review logging accuracy or check-in for an adjustment.

> ⚠️ **Common Myth**
> "The scale went up, so I gained fat." A single weigh-in tells you almost nothing about body composition. The trend needs 2+ weeks of data to be reliable. Judging progress from one day is like judging a movie from one frame.

## How Fast Does the Trend Respond?

The trend is designed to be slow enough to filter noise but fast enough to reflect real changes:

- **First 1-2 weeks of a new diet:** The trend may not move much. Give it time.
- **After 3-4 weeks:** If the trend isn't moving, you may need to adjust calories.
- **During refeeds or diet breaks:** The trend may spike up temporarily (water/glycogen rebound) even though fat loss continued.

> 💡 **Key Idea**
> Don't evaluate your progress based on less than two weeks of trend data. The trend needs time to separate signal from noise.

## Where to See Your Trend

- **Dashboard** — Weight Trend card
- **Strategy screen** — Both values side by side, plus change rate
- **Progress screen** — Full chart over time
- **AI Coach** — Can query your trend for context

> 📊 **In MacroPhase**
> The trend weight is also what feeds the TDEE algorithm. Accurate weigh-ins → accurate trend → accurate expenditure estimate → better calorie targets. The whole system compounds.
