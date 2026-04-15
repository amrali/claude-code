# Enter Plan Mode Tool Prompt

**Source:** `src/tools/EnterPlanModeTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getEnterPlanModeToolPrompt()`

## Description

Enters plan mode where Claude explores the codebase and designs an implementation approach for user approval before writing any code.

## Prompt Content

### External users version:

```
Use this tool proactively when you're about to start a non-trivial implementation task. Getting user sign-off on your approach before writing code prevents wasted effort and ensures alignment. This tool transitions you into plan mode where you can explore the codebase and design an implementation approach for user approval.

## When to Use This Tool

**Prefer using EnterPlanMode** for implementation tasks unless they're simple. Use it when ANY of these conditions apply:

1. **New Feature Implementation**: Adding meaningful new functionality
2. **Multiple Valid Approaches**: The task can be solved in several different ways
3. **Code Modifications**: Changes that affect existing behavior or structure
4. **Architectural Decisions**: The task requires choosing between patterns or technologies
5. **Multi-File Changes**: The task will likely touch more than 2-3 files
6. **Unclear Requirements**: You need to explore before understanding the full scope
7. **User Preferences Matter**: If you would use AskUserQuestion to clarify the approach, use EnterPlanMode instead

## When NOT to Use This Tool

Only skip EnterPlanMode for simple tasks:
- Single-line or few-line fixes (typos, obvious bugs, small tweaks)
- Adding a single function with clear requirements
- Tasks where the user has given very specific, detailed instructions
- Pure research/exploration tasks (use the Agent tool with explore agent instead)

## What Happens in Plan Mode

In plan mode, you'll:
1. Thoroughly explore the codebase using Glob, Grep, and Read tools
2. Understand existing patterns and architecture
3. Design an implementation approach
4. Present your plan to the user for approval
5. Use AskUserQuestion if you need to clarify approaches
6. Exit plan mode with ExitPlanMode when ready to implement

## Important Notes

- This tool REQUIRES user approval - they must consent to entering plan mode
- If unsure whether to use it, err on the side of planning - it's better to get alignment upfront than to redo work
- Users appreciate being consulted before significant changes are made to their codebase
```

### Ant users version (more conservative):

```
Use this tool when a task has genuine ambiguity about the right approach and getting user input before coding would prevent significant rework...

When in doubt, prefer starting work and using AskUserQuestion for specific questions over entering a full planning phase.
```

## Notes

- `getEnterPlanModeToolPrompt()` returns the `ant` variant when `process.env.USER_TYPE === 'ant'`, otherwise the external variant
- When `isPlanModeInterviewPhaseEnabled()` is true, the "What Happens in Plan Mode" section is omitted (detailed workflow arrives via `plan_mode` attachment)
- Tool name: `EnterPlanMode` (from `src/tools/EnterPlanModeTool/constants.ts`)
