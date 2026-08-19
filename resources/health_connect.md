# Health Connect Integration

MacroPhase can sync with Health Connect (Android's unified health data platform) to share nutrition and weight data with other health apps.

## What Health Connect Is

Health Connect is Google's centralized health data API. It lets apps share data like steps, weight, heart rate, sleep, and nutrition — without each app needing its own integration with every device.

## What Syncs

### From MacroPhase to Health Connect

- **Nutrition** — Calories, protein, carbs, and fat from your food logs
- **Weight** — Your body weight entries

### From Health Connect to MacroPhase

- **Steps** — Daily step count (if your device tracks it)
- **Active calories** — Exercise-related calorie burn
- **Sleep** — Sleep duration and stages
- **Heart rate** — Resting and active heart rate
- **Body composition** — Body fat %, lean mass (if your scale syncs to Health Connect)

## How to Set Up

1. Open **More → Integrations → Health Connect**
2. Grant the requested permissions
3. Toggle **Auto-Sync Nutrition** to sync food logs automatically
4. Toggle **Auto-Sync Weight** to sync weight entries

> 📊 **In MacroPhase**
> Sync happens in the background when you log food or weight. You can also trigger a manual sync from the Integrations screen.

## What Doesn't Sync

- Custom foods and recipes (these are MacroPhase-specific)
- AI coach conversations
- Memory and behavior data
- Progress photos
- Body measurements (waist, chest, etc.)

These stay in MacroPhase's local database only.

## Why Use It

- **Step data** — If your phone or watch tracks steps, Health Connect can bring that data into MacroPhase for more accurate expenditure estimates
- **Cross-app consistency** — If you use a workout app that syncs to Health Connect, your activity data flows into MacroPhase automatically
- **Backup layer** — Health Connect acts as an additional data store for weight and nutrition

## Troubleshooting

**Sync not working**

- Check that Health Connect permissions are granted
- Verify Health Connect is installed and enabled on your device
- Force a manual sync from the Integrations screen

**Duplicate data**

- MacroPhase handles deduplication when syncing. If you see duplicates, try disabling and re-enabling the sync.

**Weight not syncing**

- Health Connect requires a specific data format. MacroPhase handles the conversion, but some third-party weight apps may use non-standard formats.
