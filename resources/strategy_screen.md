# The Strategy Screen

The Strategy screen is where you see the big picture — your expenditure, your targets, your progress, and what's coming next.

## What's on the Screen

### Expenditure (TDEE)

Your current estimated Total Daily Energy Expenditure, displayed as a hero number with a trend line showing how it's changed over time.

- **High confidence** — You have 5+ logged days in the past 14 days and consistent data
- **Low confidence** — Sparse data, the estimate is uncertain

### Weight Trend

Your smoothed trend weight alongside your scale weight. The trend line shows the direction your weight is actually moving, stripped of daily fluctuations. You can choose between **V1 (EWMA)** and **V2 (3-State)** in Settings → Algorithms — V1 is slower and more stable, V2 handles salt spikes and water retention more gracefully.

> See the [Weight Trend Algorithms](weight_trend_algorithms) article to understand the difference.

### Calorie Target

Your current daily calorie goal, based on your TDEE and goal type. This number updates automatically after check-ins.

### Macro Breakdown

Your daily protein, fat, and carb targets. Tap to see details about how they're calculated.

### Change Rate

How fast your trend weight is changing per week. Positive for gaining, negative for losing, near-zero for maintaining.

## Check-In Section

If a check-in is available, the Strategy screen shows a check-in card with:

- Your current progress toward your goal
- The recommended adjustment (if any)
- Confidence indicator

Tap to complete your check-in and review the recommendation.

## Goal History

Your active goal is displayed with:

- Goal type (lose, maintain, gain)
- Target rate of change
- Start date and starting weight
- Current progress

> 📊 **In MacroPhase**
> The Strategy screen pulls from `DataRepository.calculateMetricsAcrossRange()`, which runs the full pipeline: BMR baseline, weight trend (EWMA or 3-State, depending on your settings), rolling window expenditure with goal-aware V3 adjustment, logging break detection, and data quality diagnostics.

## What the Numbers Mean

- **TDEE going up** — Your expenditure is increasing (more activity, muscle gain, or recovery from dieting)
- **TDEE going down** — Metabolic adaptation or reduced activity
- **Trend weight dropping steadily** — You're in a deficit and losing fat
- **Trend weight flat despite deficit** — Check logging accuracy or wait for the trend to catch up
- **Scale weight bouncing around** — Normal noise, trust the trend

> 💡 **Key Idea**
> The Strategy screen is updated every time you log food or weight. Check it weekly, not daily. The numbers need time to stabilize and reflect real changes.
