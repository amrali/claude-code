# Exit Plan Mode Tool Prompt

**Source:** `src/tools/ExitPlanModeTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `EXIT_PLAN_MODE_V2_TOOL_PROMPT`

## Description

Exits plan mode and signals to the user that planning is complete and the implementation plan is ready for review/approval. Does not accept the plan content — reads it from the plan file.

## Prompt Content

```
Use this tool when you are in plan mode and have finished writing your plan to the plan file and are ready for user approval.

## How This Tool Works
- You should have already written your plan to the plan file specified in the plan mode system message
- This tool does NOT take the plan content as a parameter - it will read the plan from the file you wrote
- This tool simply signals that you're done planning and ready for the user to review and approve
- The user will see the contents of your plan file when they review it

## When to Use This Tool
IMPORTANT: Only use this tool when the task requires planning the implementation steps of a task that requires writing code. For research tasks where you're gathering information, searching files, reading files or in general trying to understand the codebase - do NOT use this tool.

## Before Using This Tool
Ensure your plan is complete and unambiguous:
- If you have unresolved questions about requirements or approach, use AskUserQuestion first (in earlier phases)
- Once your plan is finalized, use THIS tool to request approval

**Important:** Do NOT use AskUserQuestion to ask "Is this plan okay?" or "Should I proceed?" - that's exactly what THIS tool does. ExitPlanMode inherently requests user approval of your plan.

## Examples

1. Initial task: "Search for and understand the implementation of vim mode in the codebase" - Do not use the exit plan mode tool because you are not planning the implementation steps of a task.
2. Initial task: "Help me implement yank mode for vim" - Use the exit plan mode tool after you have finished planning the implementation steps of the task.
3. Initial task: "Add a new feature to handle user authentication" - If unsure about auth method (OAuth, JWT, etc.), use AskUserQuestion first, then use exit plan mode tool after clarifying the approach.
```

## Notes

- Tool name: `ExitPlanMode` (from `src/tools/ExitPlanModeTool/constants.ts` as `EXIT_PLAN_MODE_TOOL_NAME`)
- The plan content must be written to the plan file before calling this tool
- An older `EXIT_PLAN_MODE_TOOL_PROMPT` also exists in the file (v1 format for backward compatibility)
