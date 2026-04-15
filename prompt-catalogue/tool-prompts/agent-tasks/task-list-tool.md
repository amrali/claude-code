# Task List Tool Prompt

**Source:** `src/tools/TaskListTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()`

## Description

Lists all tasks in the task list with status, owner, and blocking information.

## Prompt Content

```
Use this tool to list all tasks in the task list.

## When to Use This Tool

- To see what tasks are available to work on (status: 'pending', no owner, not blocked)
- To check overall progress on the project
- To find tasks that are blocked and need dependencies resolved
[When agent swarms enabled:]
- Before assigning tasks to teammates, to see what's available
- After completing a task, to check for newly unblocked work or claim the next available task
- **Prefer working on tasks in ID order** (lowest ID first) when multiple tasks are available

## Output

Returns a summary of each task:
- **id**: Task identifier (use with TaskGet, TaskUpdate)
- **subject**: Brief description of the task
- **status**: 'pending', 'in_progress', or 'completed'
- **owner**: Agent ID if assigned, empty if available
- **blockedBy**: List of open task IDs that must be resolved first

[When agent swarms enabled:]
## Teammate Workflow

When working as a teammate:
1. After completing your current task, call TaskList to find available work
2. Look for tasks with status 'pending', no owner, and empty blockedBy
3. **Prefer tasks in ID order** (lowest ID first) when multiple tasks are available
4. Claim an available task using TaskUpdate (set `owner` to your name), or wait for leader assignment
5. If blocked, focus on unblocking tasks or notify the team lead
```

## Notes

- Tool name: `TaskList` (from `src/tools/TaskListTool/constants.ts`)
- `DESCRIPTION = 'List all tasks in the task list'`
- `getPrompt()` adds teammate-specific workflow section when `isAgentSwarmsEnabled()` is true
