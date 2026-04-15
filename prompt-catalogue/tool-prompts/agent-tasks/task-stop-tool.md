# Task Stop Tool Prompt

**Source:** `src/tools/TaskStopTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `DESCRIPTION`

## Description

Stops a running background task by its ID.

## Prompt Content

```
- Stops a running background task by its ID
- Takes a task_id parameter identifying the task to stop
- Returns a success or failure status
- Use this tool when you need to terminate a long-running task
```

## Notes

- Tool name: `TaskStop` (from `src/tools/TaskStopTool/prompt.ts` as `TASK_STOP_TOOL_NAME`)
- `DESCRIPTION` is the complete prompt text
