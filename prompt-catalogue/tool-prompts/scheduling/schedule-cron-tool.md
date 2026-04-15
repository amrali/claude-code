# Schedule Cron Tool Prompt

**Source:** `src/tools/ScheduleCronTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `buildCronCreatePrompt()`, `buildCronDeletePrompt()`, `buildCronListPrompt()`

## Description

Three tools for scheduling, cancelling, and listing cron jobs. Jobs can be session-only (in-memory) or durable (persisted to `.claude/scheduled_tasks.json`).

## CronCreate Prompt Content

```
Schedule a prompt to be enqueued at a future time. Use for both recurring schedules and one-shot reminders.

Uses standard 5-field cron in the user's local timezone: minute hour day-of-month month day-of-week. "0 9 * * *" means 9am local — no timezone conversion needed.

## One-shot tasks (recurring: false)

For "remind me at X" or "at <time>, do Y" requests — fire once then auto-delete.
Pin minute/hour/day-of-month/month to specific values:
  "remind me at 2:30pm today to check the deploy" → cron: "30 14 <today_dom> <today_month> *", recurring: false
  "tomorrow morning, run the smoke test" → cron: "57 8 <tomorrow_dom> <tomorrow_month> *", recurring: false

## Recurring jobs (recurring: true, the default)

For "every N minutes" / "every hour" / "weekdays at 9am" requests:
  "*/5 * * * *" (every 5 min), "0 * * * *" (hourly), "0 9 * * 1-5" (weekdays at 9am local)

## Avoid the :00 and :30 minute marks when the task allows it

Every user who asks for "9am" gets `0 9`, and every user who asks for "hourly" gets `0 *` — which means requests from across the planet land on the API at the same instant. When the user's request is approximate, pick a minute that is NOT 0 or 30:
  "every morning around 9" → "57 8 * * *" or "3 9 * * *" (not "0 9 * * *")
  "hourly" → "7 * * * *" (not "0 * * * *")

Only use minute 0 or 30 when the user names that exact time and clearly means it.

## Durability [when durable mode enabled]

By default (durable: false) the job lives only in this Claude session. Pass durable: true to write to .claude/scheduled_tasks.json so the job survives restarts. Only use durable: true when the user explicitly asks for the task to persist.

## Runtime behavior

Jobs only fire while the REPL is idle (not mid-query). Recurring tasks auto-expire after [DEFAULT_MAX_AGE_DAYS] days — fire one final time, then are deleted. Returns a job ID you can pass to CronDelete.
```

## CronDelete Prompt Content

```
[When durable enabled:]
Cancel a cron job previously scheduled with CronCreate. Removes it from .claude/scheduled_tasks.json (durable jobs) or the in-memory session store (session-only jobs).

[When session-only:]
Cancel a cron job previously scheduled with CronCreate. Removes it from the in-memory session store.
```

## CronList Prompt Content

```
[When durable enabled:]
List all cron jobs scheduled via CronCreate, both durable (.claude/scheduled_tasks.json) and session-only.

[When session-only:]
List all cron jobs scheduled via CronCreate in this session.
```

## Notes

- Tool names: `CronCreate`, `CronDelete`, `CronList`
- The scheduling system is gated by `isKairosCronEnabled()` (GrowthBook flag `tengu_kairos_cron`, default `true`)
- Durable persistence is gated by `isDurableCronEnabled()` (GrowthBook flag `tengu_kairos_cron_durable`, default `true`)
- Local environment variable `CLAUDE_CODE_DISABLE_CRON=true` disables the whole system
- `DEFAULT_MAX_AGE_DAYS = DEFAULT_CRON_JITTER_CONFIG.recurringMaxAgeMs / (24 * 60 * 60 * 1000)`
- 5-field cron syntax; the scheduler adds jitter: recurring tasks up to 10% late (max 15 min), one-shot at :00/:30 up to 90s early
