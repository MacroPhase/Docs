# Weight Trend Algorithms: EWMA vs 3-State

When you step on the scale every day, the numbers bounce around — water, salt, sleep, and glycogen all push the needle. MacroPhase smooths this out to find your **actual weight trend**, the direction your body is really moving.

We give you two ways to do that. You choose which fits your brain.

## Why Two Algorithms?

Different people want different things from a trend line.

Some want a **causal, lagged** trend — it only looks at the past and never revises what it drew yesterday. This feels honest and stable, even if it's slow to catch changes.

Others want the **smoothest possible line** — even if that means yesterday's trend point might shift slightly when today's data arrives. The priority is filtering out noise, not guaranteeing yesterday's number stays frozen.

Both are valid. Both are correct. They just optimize for different things.

## V1 — EWMA (Exponential Weighted Moving Average)

This is the original algorithm. It works like this:

```
trend[today] = 0.1 × scaleWeight[today] + 0.9 × trend[yesterday]
```

That's it. One formula, one parameter, one pass through the data.

- **Causal** — never revises the past
- **Lagged** — takes about 19 days for a change to be 50% reflected (alpha = 0.1)
- **Interpretable** — the math fits on a napkin
- **Best for** — users who want a stable, predictable line that never surprises them

> 💡 **Key Idea**
> V1 is like a slow friend. It doesn't panic when you have a salty pizza night. It takes about a week to notice you started a new diet. That's the tradeoff: stability vs responsiveness.

## V2 — 3-State Trend (Kalman Filter + Smoother)

This is the advanced option. Instead of a single moving average, it models your weight as a **physical system** with three hidden states:

1. **Position** — your true weight in kg
2. **Velocity** — how fast your weight is changing (kg/day)
3. **Acceleration** — whether that rate is speeding up or slowing down

Every day, the algorithm:

1. **Predicts** what your weight should be based on yesterday's velocity and acceleration
2. **Compares** that prediction against your actual scale reading
3. **Updates** all three states — but only lets a fraction of the scale noise through
4. **Smooths backward** — after the forward pass, it runs in reverse to refine every point using all available data (both past and future)

The result: **transient noise** (salt spikes, water retention, big meals) barely moves the line, while **genuine weight changes** (fat loss, muscle gain) are detected faster.

> 📊 **In MacroPhase**
> The 3-State trend also exposes your **velocity** (kg/day rate of change) behind the scenes. This feeds into the V3 expenditure algorithm, which uses the cleaner weight deltas to avoid overreacting when you start a new diet.

### How It Handles the Salt Spike

Let's say you ate a high-sodium meal and the scale jumps 2 kg overnight.

| Algorithm | What happens |
|-----------|-------------|
| **V1 (EWMA)** | The spike pushes your trend up by 0.2 kg. It takes days to come back down, even though you didn't actually gain fat. |
| **V2 (3-State)** | The filter recognizes the magnitude of the jump and attributes most of it to measurement noise. The trend barely moves. When the water weight drops the next day, the trend is already nearly correct. |

### The "Rewriting History" Part

V2 is **non-causal** — the backward smoothing pass means that when new data arrives, earlier trend points can shift slightly.

This isn't a bug. The RTS smoother is saying: "Given everything I know now, including what came after, here's the best estimate of what your weight actually was on that date." Your June 5th trend point might look slightly different on June 6th than it did on June 5th — but the change is bounded (typically < 0.1 kg).

> ⚠️ **Common Myth**
> "The trend changing retroactively means it's wrong." It means it's getting more accurate. Your scale weight on June 5th was always uncertain — a single measurement can't tell you your true weight. As more data comes in, the algorithm gets better at guessing what the real number probably was.

## Which Should You Use?

| If you... | Try |
|-----------|-----|
| Want a trend that never surprises you | V1 |
| Get spiky scale readings (salt, refeeds, carb-ups) | V2 |
| Don't log calories and just watch the trend | V1 |
| Use MacroPhase's coaching program and check-ins | V2 |
| Hate when numbers change retroactively | V1 |
| Want the smoothest possible line | V2 |

You can switch anytime in **Settings → Algorithms**. The trend recomputes instantly from your existing weigh-in history. No data lost, no reset required.

> 💡 **Key Idea**
> V1 and V2 are both honest — they just define "honest" differently. V1 says "I'll never change what I told you yesterday." V2 says "I'll always give you the best estimate possible, using everything I know." Neither is lying to you.

## What Changes When You Switch

- Your **trend weight** changes slightly (V2 is usually 0.1–0.3 kg lower during periods of water retention)
- Your **change rate** (kg/week) becomes more current — V2 recency-weights recent changes
- Your **prediction line** (the dashed forward projection on the weight chart) now has 3 modes: Weight Data, Blended, Goal Rate
- If you're using V3 expenditure, your TDEE estimate may shift slightly because the trend deltas are cleaner

Everything else — calorie targets, macro splits, check-in logic — stays the same.
