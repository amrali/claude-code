# Sleep Tool Prompt

**Source:** `src/tools/SleepTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `SLEEP_TOOL_PROMPT`

## Description

Waits for a specified duration. Can be interrupted by the user. Preferred over `Bash(sleep ...)` as it doesn't hold a shell process.

## Prompt Content

```
Wait for a specified duration. The user can interrupt the sleep at any time.

Use this when the user tells you to sleep or rest, when you have nothing to do, or when you're waiting for something.

You may receive <tick> prompts — these are periodic check-ins. Look for useful work to do before sleeping.

You can call this concurrently with other tools — it won't interfere with them.

Prefer this over `Bash(sleep ...)` — it doesn't hold a shell process.

Each wake-up costs an API call, but the prompt cache expires after 5 minutes of inactivity — balance accordingly.
```

## Notes

- Tool name exported as `SLEEP_TOOL_NAME = 'Sleep'`
- `DESCRIPTION = 'Wait for a specified duration'`
- The `<tick>` tag is the `TICK_TAG` constant from `src/constants/xml.ts`
- Prompt cache note: 5-minute inactivity expiry means excessive sleeping wastes cache warm-up cost
