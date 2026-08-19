# How Calorie Targets Are Calculated

A calorie target isn't something you pick from a menu. It's the output of a calculation that starts with who you are, gets refined by what you actually do, and is adjusted by what you want to achieve.

Here's how MacroPhase builds yours.

## Step 1: Your Baseline

When you first set up MacroPhase, your profile produces a starting estimate:

- **Mifflin-St Jeor** (default): Uses your weight, height, age, and sex
- **Katch-McArdle** (if body fat % is known): Uses your lean body mass, which is more accurate

This baseline is multiplied by your activity multiplier to estimate your starting TDEE.

> 📊 **In MacroPhase**
> The baseline is just a starting point. Once you have 5+ days of food logs and weight data, MacroPhase switches to a dynamic TDEE calculated from your actual intake and weight trends. The baseline gets overwritten by reality.

## Step 2: Dynamic TDEE

Once you have enough data, MacroPhase calculates your expenditure from the relationship between what you ate and how your trend weight changed.

The logic is simple:

- If your trend weight went **down** and you ate 2,000 cal/day → your TDEE was **above** 2,000
- If your trend weight went **up** and you ate 2,000 cal/day → your TDEE was **below** 2,000
- If your trend weight stayed **flat** → your TDEE was roughly **equal** to 2,000

The algorithm also accounts for the energy density of tissue gained or lost, because gaining muscle and gaining fat require different amounts of energy.

## Step 3: Goal Adjustment

Your goal determines where your target sits relative to TDEE:

- **Lose weight:** Target = TDEE minus a deficit
- **Maintain:** Target ≈ TDEE
- **Gain weight:** Target = TDEE plus a surplus

The deficit or surplus is calculated from your desired weekly rate of change.

> 💡 **Key Idea**
> A 500 cal/day deficit doesn't guarantee exactly 1 lb/week of fat loss. Your body adapts. That's why MacroPhase recalculates weekly instead of setting a number and forgetting about it.

## Step 4: Check-In Refinement

Each week, your check-in reviews progress and may recommend an adjustment. Changes are capped at 200 kcal per week to prevent wild swings.

## How Your Macros Are Set

Once your calorie target is determined, the three macros are calculated:

### Protein

Your weight × your protein tier:

- **Low:** 1.2 g/kg
- **Moderate:** 1.6 g/kg
- **High:** 2.0 g/kg
- **Extra High:** 2.4 g/kg

**Example:** A 90 kg person cutting weight at the High tier would target roughly 180g of protein per day.

> ⚠️ **Common Myth**
> More protein is always better. Going from 60g to 140g per day has a huge impact on satiety and muscle retention. Going from 220g to 300g usually won't. There's a point of diminishing returns, and it's lower than most people think.

### Fat

Based on your diet preference:

- **Balanced:** minimum 28% of calories
- **Low Fat:** minimum 20%
- **High Protein:** minimum 20%
- **Mediterranean:** minimum 35%
- **Keto:** minimum 75%

> ⚠️ **Common Myth**
> Eating fat does not directly become body fat. Body fat gain is driven by sustained calorie surplus, not by a specific macronutrient. You can eat a high-fat diet and lose weight, or eat a low-fat diet and gain weight. Calories are what matter most.

### Carbs

Everything remaining after protein and fat:

`Carbs = (Total calories - Protein calories - Fat calories) / 4`

> ⚠️ **Common Myth**
> Carbs do not automatically cause fat gain. If calories are controlled, high-carb diets can be just as effective for fat loss as lower-carb diets. Carb timing and composition matter less than total energy balance.

## Calorie Floors

MacroPhase enforces minimum targets to protect your health:

- **Standard:** 1,200 kcal
- **BMR-based:** 90% of your BMR (minimum 800 kcal)
- **Absolute:** 800 kcal hard limit

If the math calls for less, the target is clamped upward.

> ⚠️ **Common Myth**
> "Eat less, move more" has a floor. Very low calorie diets cause muscle loss, metabolic suppression, and hormonal disruption. That's why MacroPhase won't let your target drop below a safe threshold, even if the math says otherwise.

## Training Day Shifting

If you have a training schedule, MacroPhase can shift calories between days — surplus on training days, deficit on rest days. The weekly total stays the same. This is optional and configured in your program settings.

## The Bottom Line

Most people obsess over macro ratios when total calories and protein intake matter far more. If you're unsure where to start, hit your calorie target, prioritize protein, and let MacroPhase handle the rest.
